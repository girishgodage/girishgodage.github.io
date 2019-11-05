---
title: ASP. NET Core - Dependency-injection
date: 2019-11-01 05:54:00 Z
permalink: "/blog/customize-dependency-injection"
categories:
- AspDotnetCoreCustomize
tags:
- customization
- learning
summary: In the third part we'll take a look into the ASP.NET Core dependency injection
  and how to customize it to use a different dependency injection container if needed.
image: "/img/aspnetcore.png"
author: Girish Godage
layout: posts
prevurl: "/blog/customize-configuration"
nexturl: "/blog/customize-https"
discussion_id: 2019-11-01-customize-dependency-injection
---

## Dependency Injection

In the third part we'll take a look into the ASP.NET Core dependency injection and how to customize it to use a different dependency injection container if needed.

## Why using a different dependency injection container?

In the most projects you don't really need to use a different dependency injection container than the exiting one. The DI implementation in ASP.NET Core supports the main basic features and works well and pretty fast. Anyway, some other DI container support some interesting features you maybe want to use in your application.

- Maybe you like to create an application that supports modules as lightweight dependencies.
  - E.g. modules you want to put into a specific directory and they get automatically registered in your application
  - This could be done with NInject.
- Maybe you want to configure the services in a configuration file outside the application, in an XML or JSON file instead in C# only
  - This is a common feature in various DI containers, but not yet supported in ASP.NET Core.
- Maybe you don't want to have an immutable DI container, because you want to add services at runtime.
  - This is also a common feature in some DI containers.

## A look at the ConfigureServices Method

Create a new ASP.NET Core project and open the `Startup.cs`, you will find the method to configure the services which looks like this:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<CookiePolicyOptions>(options =>
    {
        // This lambda determines whether user 
        // consent for non-essential cookies is 
        // needed for a given request.
        options.CheckConsentNeeded = context => true;
    });
    
    services.AddTransient<IService, MyService>(); // custom service

    services.AddControllersWithViews();
    services.AddRazorPages();
}
```

This method gets the `IServiceCollection`, which already filled with a bunch of services which are needed by ASP.NET Core. This services got added by the hosting services and parts of ASP.NET Core that got executed before the method `ConfigureSercices` is called.

Inside the method some more services gets added. First a configuration class that contains cookie policy options is added to the `ServiceCollection`. In this sample I also add a custom service called `MyService` that implements the `IService` interface. After that the method `AddMvc()` adds another bunch of services needed by the MVC framework. Until yet we have around 140 services registered to the `IServiceCollection`. But the service collections isn't the actual dependency injection container. 

The actual DI container is wrapped in the so called service provider, which will be created out of the service collection. The `IServiceCollection` has an extension method registered to create a `IServiceProvider` out of the service collection.

```csharp
IServiceProvider provider = services.BuildServiceProvider()
```

The `ServiceProvider` than contains the immutable container that cannot be changed at runtime. With the default method `ConfigureServices` the `IServiceProvider` gets created in the background after this method was called, but it is possible to change the method a little bit:

```csharp
public IServiceProvider ConfigureServices(IServiceCollection services)
{
    services.Configure<CookiePolicyOptions>(options =>
    {
        // This lambda determines whether user 
        // consent for non-essential cookies is 
        // needed for a given request.
        options.CheckConsentNeeded = context => true;
    });
    
    services.AddTransient<IService, MyService>(); // custom service

    services.AddControllersWithViews();
    services.AddRazorPages();
    
    return services.BuildServiceProvider()
}
```

I changed the return type to `IServiceProvider` and return the `ServiceProvider` created with the method `BuildServiceProvider()`. This change will still work in ASP.NET Core. 

## Use a different `ServiceProvider`

To change to a different or custom DI container you need to replace the default implementation of the `IServiceProvider` with a different one. Additionally you need to find a way to move the already registered services to the new container.

The next code sample uses Autofac as a third party container. 

~~~ shell
dotnet add package Autofac.Extensions.DependencyInjection
~~~

I use Autofac in this snippet because you are easily able to see what is happening here:

```csharp
public IServiceProvider ConfigureServices(IServiceCollection services)
{
     services.Configure<CookiePolicyOptions>(options =>
    {
        // This lambda determines whether user 
        // consent for non-essential cookies is 
        // needed for a given request.
        options.CheckConsentNeeded = context => true;
    });
    
    // services.AddTransient<IService, MyService>(); // custom service

    services.AddControllersWithViews();
    services.AddRazorPages();

    // create a Autofac container builder
    var builder = new ContainerBuilder();

    // read service collection to Autofac
    builder.Populate(services);

    // use and configure Autofac
    builder.RegisterType<MyService>().As<IService>(); // custom service

    // build the Autofac container
    ApplicationContainer = builder.Build();

    // creating the IServiceProvider out of the Autofac container
    return new AutofacServiceProvider(ApplicationContainer);
}

// IContainer instance in the Startup class 
public IContainer ApplicationContainer { get; private set; }
```

Also Autofac works with a kind of a service collection inside the `ContainerBuilder` and it creates the actual container out of the `ContainerBuilder`. To get the registered services out of the `IServiceCollection` into the `ContainerBuilder`, Autofac uses the `Populate()` method. This copies all the existing services to the Autofac container.

Our custom service `MyService` now gets registered using the Autofac way.

After that, the container gets build and stored in a property of type `IContainer`. In the last line of the method `ConfigureServices` we create a `AutofacServiceProvider` and pass in the `IContainer`. This is the `IServiceProvider` we need to return to use Autofac within our application.

## Introducing Scrutor

You don't always need to replace the existing .NET Core DI container to get and use nice features. In the beginning I mentioned the auto registration of services. This can also be done with a nice NuGet package called [Scrutor](https://www.nuget.org/packages/Scrutor/) by [Kristian Hellang](https://twitter.com/khellang) ([https://kristian.hellang.com/](https://kristian.hellang.com/)). Scrutor extends the IServiceCollection to automatically register services to the .NET Core DI container.

> "Assembly scanning and decoration extensions for Microsoft.Extensions.DependencyInjection"
> https://github.com/khellang/Scrutor

[Andrew Lock](https://andrewlock.net) published a pretty detailed blog post about Scrutor. It doesn't make sense to repeat that. Read that awesome post and learn more about it: [Using Scrutor to automatically register your services with the ASP.NET Core DI container](https://andrewlock.net/using-scrutor-to-automatically-register-your-services-with-the-asp-net-core-di-container/)

## Update on ASP.NET Core 3.0

The last sections only work in ASP.NET Core prior to version 3.0. Since the Hosting model changed in ASP.NET Core 3.0 the Startup method `ConfigureService()`, cannot return an `IServiceProvider` anymore. To register a custom IoC container you need to register a different `IServiceProviderFactory`. In that case an `AutofacServiceProviderFactory`, if you use Autofac.

I created a small extension method to register Autofac:

~~~ csharp
public static class IHostBuilderExtension
{
    public static IHostBuilder UseAutofacServiceProviderFactory(
        this IHostBuilder hostbuilder)
    {
        hostbuilder.UseServiceProviderFactory<ContainerBuilder>(
            new AutofacServiceProviderFactory());
        return hostbuilder;
    }
}
~~~

The `AutofacServiceProviderFactory` is provided by the latest version of the `Autofac.Extensions.DependencyInjection` package.

And this is used in the `Program.cs` like this:

~~~ csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .UseAutofacServiceProviderFactory()
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder
                    .UseStartup<Startup>();
            });
}
~~~

If you have this in place you are able to use Autofac as you like. In the `Startup.cs` you are able to use the `IServiceCollection` to register the dependencies or to add an additional method called `ConfigureContainer` to do it the Autofac way:

~~~ csharp
// ConfigureContainer is where you can register things directly
// with Autofac. This runs after ConfigureServices so the things
// here will override registrations made in ConfigureServices.
// Don't build the container; that gets done for you. If you
// need a reference to the container, you need to use the
// "Without ConfigureContainer" mechanism shown later.
public void ConfigureContainer(ContainerBuilder builder)
{
    builder.RegisterType<MyService>().As<IService>(); // custom service
}
~~~

## Conclusion

Using this approach you are able to use any .NET Standard compatible DI container to replace the existing one. If the container of your choice doesn't provide a `ServiceProvider`, create your own that implements `IServiceProvider` and uses the DI container inside. If the container of your choice doesn't provide a method to populate the registered services into the container, create your own method. Loop over the registered services and add them to the other container.

Actually the last step sounds easy, but can be a hard task. Because you need to translate all the possible `IServiceCollection` registrations into registrations of the different container. The complexity of that task depends on the implementation details of the other one.

Anyway, you have the choice to use any DI container which is compatible to the .NET Standard. You have the choice to change a lot of default implementations in ASP.NET Core. 

So you can with the default HTTPS behavior on Windows. To learn more about that please read the next chapter.

---
{% include netcore_customize.md %}

{% include blogslide.html %}

