---
title: ASP. NET Core - HTTPS
date: 2019-11-01 05:54:00 Z
permalink: "/blog/customize-https"
categories:
- AspDotnetCoreCustomize
tags:
- customization
- learning
summary: HTTPS is on by default now and a first class feature. On Windows the certificate which is needed to enable HTTPS is loaded from the windows certificate store. If you create a project on Linux and Mac the certificate is loaded from a certificate file. 
image: "/img/aspnetcore.png"
author: Girish Godage
layout: posts
prevurl: "/blog/customize-dependency-injection"
nexturl: "/blog/customize-hostedservices"
discussion_id: 2019-11-01-customize-https
---

## HTTPS

HTTPS is on by default now and a first class feature. On Windows the certificate which is needed to enable HTTPS is loaded from the windows certificate store. If you create a project on Linux and Mac the certificate is loaded from a certificate file.

> Even if you want to create a project to run it behind an IIS or an NGinX webserver HTTPS is enabled. Usually you would manage the certificate on the IIS or NGinX webserver in that case. But this shouldn't be a problem and you shouldn't disable HTTPS in the ASP.NET Core settings.
>
> To manage the certificate within the ASP.NET Core application directly makes sense if you run services behind the firewall, services which are not accessible from the internet. Services like background services for a micro service based application, or services in a self hosted ASP.NET Core application.

There are some scenarios where it makes sense to also load the certificate from a file on Windows. This could be in an application that you will run on docker for Windows, and also on docker for Linux.

Personally I like the flexible way to load the certificate from a file.

## Setup Kestrel

As well as in the first two parts of this blog series, we need to override the default `WebHostBuilder` a little bit. With ASP.NET Core 3.0 it is possible to replace the default Kestrel based hosting with an hosting based on an `HttpListener`. This means the Kestrel webserver is configured somehow to the host builder. You are able to add and configure Kestrel manually by **using** it. That means by calling the `UseKestrel()` method on the `IWebHostBuilder`:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {                
                webBuilder
                    .UseKestrel(options => 
                    {	
                    })
                    .UseStartup<Startup>();
            }
}
```

This method accepts an action to configure the Kestrel webserver. What we actually need to do is to configure the addresses and ports the webserver is listening on. For the HTTPS port we also need to configure how the certificate should be loaded.

```csharp
.UseKestrel(options => 
{
	options.Listen(IPAddress.Loopback, 5000);
	options.Listen(IPAddress.Loopback, 5001, listenOptions =>
	{
		listenOptions.UseHttps("certificate.pfx", "topsecret");
	});
})
```

In this snippet we add to addresses and ports to listen on. The second one is defined as secure endpoint configured to use HTTPS. The method `UseHttps()` is overloaded multiple times, to load certificates from the windows certificate store as well as from files. In this case we use a file called `certificate.pfx` located in the project folder.

> Reminder to myself: Replacing the host actually would be an idea for an eleventh part of this series.

To create such a certificate file to just play around with this configuration open the certificate store and export the development certificate created by visual studio.

## For your safety

Use the following line **ONLY** to play around with this configuration:

```csharp
listenOptions.UseHttps("certificate.pfx", "topsecret");
```

The problem is the hard coded password. **Never ever** store a password in a code file that gets pushed to any source code repository. Ensure you load the password from the configuration API of ASP.NET Core. Use the user secrets on your local development machine and use environment variables on a server. On Azure use the Application Settings to store the passwords. Passwords will be hidden on the Azure Portal UI, if they are marked as passwords.

## Conclusion

This is just a small customization. Anyway, this helps if you want to share the code between different platforms, if you want to run your application on Docker and don't want to care about certificate stores, etc.

Usually, if you run your application behind an web server like IIS or NGinX, you don't need to care about certificates in your ASP.NET Core 3.0 application. But you need to if you host your application inside another application, on Docker or without an IIS or NGinX.

ASP.NET Core 3.0 has a new feature to **run tasks in the background** inside the application. To learn more about that, read the next chapter.

---
{% include netcore_customize.md %}

{% include blogslide.html %}

