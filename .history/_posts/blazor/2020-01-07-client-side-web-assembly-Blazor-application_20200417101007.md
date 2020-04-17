---
title: Client Side Blazor Authentication Using Azure AD and a Custom AuthenticationStateProvider
date: 2020-01-07 10:41:00 Z
permalink: "/blog/Client-Side-Web-Assembly-Blazor-Application"
categories:
- Blazor
tags:
- learning
summary: In this article, using client (WebAssembly) Blazor, we will demonstrate how a list of Weather forecasts can be added to the database by each user. A user will only have the ability to see their own forecasts.
image: "/img/aws_cloud.png"
author: Girish Godage
layout: posts
prevurl: ''
nexturl: ''
discussion_id: 2020-01-07-Client-Side-Web-Assembly-Blazor-Application
---

### Client Side Blazor Authentication Using Azure AD and a Custom AuthenticationStateProvider

 In this article, using client (WebAssembly) Blazor, we will demonstrate how a list of Weather forecasts can be added to the database by each user. A user will only have the ability to see their own forecasts.

 ![image info](/img/blazor/1/BlazorClientADEndToEnd.gif)

##  Walk-Thru

![image info](/img/blazor/1/Creating-A-Step-By-Step-End-To-End-Database-image.png)

When we navigate to the **Fetch data** page, without being logged in, it will indicate that you **Must be logged** in.

To log in, click the **Log In** link.

![image info](/img/blazor/1/Creating-A-Step-By-Step-End-To-End-Datab_58BF-image.png)

You can log in with an **Azure Active Directory** account that has [authorization to log into the Azure registered application](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/developer-guidance-for-integrating-applications).

![image info](/img/blazor/1/3.png)

Once a user is *logged in*, navigating to the **Fetch data** page displays their data.

The **logged in user's name** is also displayed at the top of the application.

The user can *add* or *edit* **Weather Forecasts** that will be saved to the database.

 
## Starting With The Blazor Client AD Code

![image info](/img/blazor/1/BlazorClientADAnimation.gif)

You can easily implement **authentication** for your **Client Side Blazor applications** using **Azure Active Directory**.

We do this by [Implementing a custom AuthenticationStateProvider](https://docs.microsoft.com/en-us/aspnet/core/security/blazor/?view=aspnetcore-3.1&tabs=visual-studio#implement-a-custom-authenticationstateprovider).

## Walk-Thru

![image info](/img/blazor/1/5.png)

When we navigate to the **Fetch data** page, without being *logged in*, it will indicate that you **Must be logged in**.

To *log in*, click the **Log In link**.

![image info](/img/blazor/1/6.png)

You can log in with an **Azure Active Directory** account that has [authorization to log into the Azure registered application](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/developer-guidance-for-integrating-applications).


![image info](/img/blazor/1/7.png)

Once the user is *logged in*, navigating to the **Fetch data** page displays the data.

The **logged in user's name** is *also displayed* at the top of the application.

## Step 1 : Install Blazor Client Side (Blazor WebAssembly)

See the following directions to install Blazor WebAssembly (Client Side Blazor):

https://docs.microsoft.com/en-us/aspnet/core/blazor/get-started?view=aspnetcore-3.1&tabs=visual-studio

 

## Create The Project

Open **Visual Studio 2019 (or higher)**.

![image info](/img/blazor/1/8.png)

Create a **new project**.

![image info](/img/blazor/1/9.png)

Select **Blazor App**.

![image info](/img/blazor/1/10.png)

Name the project **BlazorClientAD**

![image info](/img/blazor/1/11.png)

Select the **ASP.NET 3.1 (or higher)** Blazor WebAssembly template and select **ASP.NET Core hosted**.

 ![image info](/img/blazor/1/12.png)

 The **Solution** will be created.

Open the **launchSettings.json** file.

![image info](/img/blazor/1/13.png)

Note the **sslPort**.

You will need it in the next section.

## Set-Up The Application In Azure

![image info](/img/blazor/1/14.png)

You will need a [free Azure Account](https://azure.microsoft.com/en-us/free) for the following steps.

Log into https://portal.azure.com/ and select the **Azure Active Directory** node.

![image info](/img/blazor/1/15.png)

Next, select **App registrations**.

![image info](/img/blazor/1/16.png)

Select **New registration**.

![image info](/img/blazor/1/17.png)

Create a Registration.

For Redirect URI, select Web, and use the following url (replacing {your sslPort} with the number you copied earlier):

> https://localhost:{your sslPort}/signin-oidc

![image info](/img/blazor/1/18.png)

On the Overview node for the application, copy the **Application (client) ID**.

![image info](/img/blazor/1/19.png)

Click on the **Authentication** node and **enable ID Tokens**.

## Configure The Application

![image info](/img/blazor/1/20.png)

Return to **Visual Studio**, and add a **appsettings.json** file to the **Server** project using the following code (replacing **{Your Application (client ID)}** with the ID you copied in the previous step):

```dotnetcli

{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/common",
    "ClientId": "{Your Application (client) ID}",
    "CallbackPath": "/signin-oidc"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  },
  "AllowedHosts": "*"
}

```
![image info](/img/blazor/1/21.png)

*Right-click* on the **Server** project and select **Manage NuGet Packages**…

![image info](/img/blazor/1/22.png)

Install:

> Microsoft.AspNetCore.Authentication.AzureAD.UI


![image info](/img/blazor/1/23.png)

Open the **Startup.cs** file in the **Server** project.


Add the following **using statements**:

```dotnetcli

using Microsoft.IdentityModel.Tokens;
using Microsoft.Extensions.Configuration;
using Microsoft.AspNetCore.Authentication.AzureAD.UI;
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Authentication.OpenIdConnect;
using System.Threading.Tasks;

```


Add the following to the top of the class:

```dotnetcli

        public IConfiguration Configuration { get; }
        public Startup(IConfiguration configuration)
        {
            Configuration = configuration;
        }
```

Add the following to the end of the ConfigureServices section:

```dotnetcli

services.AddAuthentication(AzureADDefaults.AuthenticationScheme)
    .AddAzureAD(options => Configuration.Bind("AzureAd", options));
services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme,
options =>
{
    options.TokenValidationParameters = new TokenValidationParameters
    {
        // Instead of using the default validation (validating against a 
        // single issuer value, as we do in
        // line of business apps), we inject our own multitenant 
        // validation logic
        ValidateIssuer = false,
        // If the app is meant to be accessed by entire organizations, 
        // add your issuer validation logic here.
        // IssuerValidator = 
        // (issuer, securityToken, validationParameters) => {
        //    if (myIssuerValidationLogic(issuer)) return issuer;
        //}
    };
    options.Events = new OpenIdConnectEvents
    {
        OnTicketReceived = context =>
        {
            // This is called on successful authentication
            // This is an opportunity to write to the database 
            // or alter internal roles ect.
            return Task.CompletedTask;
        },
        OnAuthenticationFailed = context =>
        {
            context.Response.Redirect("/Error");
            context.HandleResponse(); // Suppress the exception
            return Task.CompletedTask;
        }
    };
});

```

Add the following to the **Configure** section (under the **UseRouting**() line):

```dotnetcli

        app.UseAuthentication();
        app.UseAuthorization();
```

![image info](/img/blazor/1/24.png)

In the Client project, open the **_Imports.razor** file and add the following lines:


```dotnetcli

@using Microsoft.AspNetCore.Authorization
@using Microsoft.AspNetCore.Components.Authorization

```

## Server Side User Security

![image info](/img/blazor/1/25.png)

All security must be enforced in the **Server** project.

Add a file called **BlazorUser.cs** using the following code:


```csharp-interactive

using System;
using System.Collections.Generic;
using System.Text;
namespace BlazorClientAD.Shared
{
    public class BlazorUser
    {
        public string UserName { get; set; }
    }
}

```

![image info](/img/blazor/1/26.png)

Add a file to the Controllers folder in the Server project called **UserController.cs** using the following code:

```csharp-interactive

using BlazorClientAD.Shared;
using Microsoft.AspNetCore.Mvc;
namespace BlazorClientAD.Server.Controllers
{
    [ApiController]
    public class UserController : Controller
    {
        [HttpGet("api/user/GetUser")]
        public BlazorUser GetUser()
        {
            BlazorUser objBlazorUser = new BlazorUser();
            if (this.User.Identity.IsAuthenticated)
            {
                objBlazorUser.UserName = this.User.Identity.Name;
            }
            else
            {
                objBlazorUser.UserName = ""; // Not logged in
            }
            return objBlazorUser;
        }
    }
}

```

This code provides a method for the **Blazor client** side **custom authentication provider** (that we will create later), to determine if the user is *logged in*.


![image info](/img/blazor/1/27.png)

In the Controllers folder in the Server project, open the **WeatherForecastController.cs** file and add the following using statement:

```dotnetcli

using Microsoft.AspNetCore.Authorization;

```


Add the following above the public IEnumerable<WeatherForecast> Get() method:

 
```
        [Authorize]
``` 

This will prevent *non-authenticated users* from calling this method.

This is where we are implementing the required **server side security** that **cannot** be bypassed by a user manipulating **client side** code in the **Blazor WebAssembly** project.

 

## Implement A Custom AuthenticationStateProvider

![image info](/img/blazor/1/28.png)

In the **Client** project, create a **Util** folder and a file called **CustomAuthenticationProvider.cs** with the following code:


```csharp-interactive

using BlazorClientAD.Shared;
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Authorization;
using System.Net.Http;
using System.Security.Claims;
using System.Threading.Tasks;
namespace BlazorClientAD
{
    public class CustomAuthenticationProvider : AuthenticationStateProvider
    {
        private readonly HttpClient _httpClient;
        public CustomAuthenticationProvider(HttpClient httpClient)
        {
            _httpClient = httpClient;
        }
        public override async Task<AuthenticationState>
            GetAuthenticationStateAsync()
        {
            ClaimsPrincipal user;
            // Call the GetUser method to get the status
            // This only sets things like the AuthorizeView
            // and the AuthenticationState CascadingParameter
            var result =
                await _httpClient.GetJsonAsync<BlazorUser>("api/user/GetUser");
            // Was a UserName returned?
            if (result.UserName != "")
            {
                // Create a ClaimsPrincipal for the user
                var identity = new ClaimsIdentity(new[]
                {
                   new Claim(ClaimTypes.Name, result.UserName),
                }, "AzureAdAuth");
                user = new ClaimsPrincipal(identity);
            }
            else
            {
                user = new ClaimsPrincipal(); // Not logged in
            }
            return await Task.FromResult(new AuthenticationState(user));
        }
    }
}
```

The purpose of this code is to call the **GetUser** method, created earlier, to determine if the user is *logged in*.

If the user is *logged in*, it creates a **ClaimsPrincipal** with a **user**, otherwise it creates a **ClaimsPrincipal** without a **user**. The **Blazor security** knows that a **ClaimsPrincipal** with a **user** indicates that a user is *logged in*.

In the next step, this class will be registered as the **AuthenticationStateProvider**, and the **Blazor security** will allow us to show and hide elements using the built-in **authentication** tags and interfaces.

![image info](/img/blazor/1/29.png)

In the **Client** project, open the **Program.cs** file, and replace all the code with the following:

```dotnetcli

using System.Threading.Tasks;
using Microsoft.AspNetCore.Blazor.Hosting;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.AspNetCore.Components.Authorization;
namespace BlazorClientAD.Client
{
    public class Program
    {
        public static async Task Main(string[] args)
        {
            var builder = WebAssemblyHostBuilder.CreateDefault(args);
            // Use our CustomAuthenticationProvider as the 
            // AuthenticationStateProvider
            builder.Services.AddScoped<AuthenticationStateProvider,
                CustomAuthenticationProvider>();
            // Add Authentication support
            builder.Services.AddOptions();
            builder.Services.AddAuthorizationCore();
            builder.RootComponents.Add<App>("app");
            await builder.Build().RunAsync();
        }
    }
}
```

![image info](/img/blazor/1/30.png)

In the **Client** project, open the **App.razor** file and add replace all the code with the following:


```dotnetcli

<Router AppAssembly="@typeof(Program).Assembly">
    <Found Context="routeData">
        <AuthorizeRouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <CascadingAuthenticationState>
            <LayoutView Layout="@typeof(MainLayout)">
                <p>Sorry, there's nothing at this address.</p>
            </LayoutView>
        </CascadingAuthenticationState>
    </NotFound>
</Router>

```


This will implement authentication in the routing.

## Create the Login / Logout Buttons

![image info](/img/blazor/1/31.png)

In the **Client** project, add a **LoginDisplay.razor** file to the **Shared** directory using the following code:

```
<AuthorizeView>
    <Authorized>
        <h6>Hello, @context.User.Identity.Name! </h6>
        <a href="AzureAD/Account/SignOut">[Log out]</a>
    </Authorized>
    <NotAuthorized>
        <a href="AzureAD/Account/SignIn">[Log in]</a>
    </NotAuthorized>
</AuthorizeView>

```

Notice this directs the user to endpoints that begin with AzureAD/Account.

These endpoints will be serviced by code from the **Microsoft.AspNetCore.Authentication.AzureAD.UI** NuGet package that was installed earlier.

![image info](/img/blazor/1/32.png)

In the **Client** project, open the **MainLayout.razor** file and add replace all the code with the following:

 
```
@inherits LayoutComponentBase
<div class="sidebar">
    <NavMenu />
</div>
<div class="main">
    <div class="top-row px-4">
        <LoginDisplay />
    </div>
    <div class="content px-4">
        @Body
    </div>
</div>
 
```

This adds the **LoginDisplay** control to the top of the page in the application.

![image info](/img/blazor/1/33.png)

In the **Client** project, open the **Index.razor** file in the Pages directory, and add the following to the top of the file:

``` 

@page "/Account/SignOut"
 
```

When the *user* logs out of the application, the *user* will be sent to the **Account/SignOut** url. This code allows the Index.razor page to also service that destination.

 

## Consume The Blazor Client Side Security

![image info](/img/blazor/1/34.png)

Finally, in the Client project, open the FetchData.razor file, in the Pages directory, and add replace all the code with the following:

``` 

@page "/fetchdata"
@using BlazorClientAD.Shared
@inject HttpClient Http
<h1>Weather forecast</h1>
<p>This component demonstrates fetching data from the server.</p>
<AuthorizeView>
    <Authorized>
        @if (forecasts == null)
        {
            <p><em>Loading...</em></p>
        }
        else
        {
            <table class="table">
                <thead>
                    <tr>
                        <th>Date</th>
                        <th>Temp. (C)</th>
                        <th>Temp. (F)</th>
                        <th>Summary</th>
                    </tr>
                </thead>
                <tbody>
                    @foreach (var forecast in forecasts)
                    {
                        <tr>
                            <td>@forecast.Date.ToShortDateString()</td>
                            <td>@forecast.TemperatureC</td>
                            <td>@forecast.TemperatureF</td>
                            <td>@forecast.Summary</td>
                        </tr>
                    }
                </tbody>
            </table>
        }
    </Authorized>
    <NotAuthorized>
        <p>Must be logged in</p>
    </NotAuthorized>
</AuthorizeView>
@code {
    [CascadingParameter]
    private Task<AuthenticationState>
        authenticationStateTask
    { get; set; }
    private WeatherForecast[] forecasts;
    protected override async Task OnInitializedAsync()
    {
        var authState = await authenticationStateTask;
        var user = authState.User;
        if (user.Identity != null)
        {
            if (user.Identity.IsAuthenticated)
            {
                forecasts =
                    await Http
                    .GetJsonAsync<WeatherForecast[]>("WeatherForecast");
            }
        }
    }
}
 
```

The **AuthorizeView** tag and **AuthenticationState** Cascading Parameter will be triggered by the custom authentication provider created earlier.

**Note:** This is helpful for the application to determine what should be shown, but, because **Blazor Client Side security** can be bypassed, security must always be enforced in the **server side code**.