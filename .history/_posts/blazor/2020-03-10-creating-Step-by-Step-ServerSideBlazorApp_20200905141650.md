---
title: Creating A Step-By-Step End-To-End Database Server-Side Blazor Application
date: 2020-03-10 10:26:00 Z
permalink: "/blog/creating-step-by-step-ServerSideBlazorApp"
categories:
- Blazor
tags:
- learning
summary: In this article, we will demonstrate how a list of Weather forecasts can be added to the database by each user. A user will only have the ability to see their own forecasts.
image: "/img/aws_cloud.png"
author: Girish Godage
layout: posts
prevurl: ''
nexturl: ''
discussion_id: 2020-01-07-creating-creating-step-by-step-ServerSideBlazorApp
---

## Creating A Step-By-Step End-To-End Database Server-Side Blazor Application

The primary benefit we have when using **server-side Blazor** is that we do not have to make web http calls from the client code to the server code. This reduces the code we need to write and eliminates many security concerns.

In this article, we will demonstrate how a list of **Weather forecasts** can be added to the database by each user. A user will only have the ability to see their own forecasts.

![image info](/img/blazor/3/1.png)

## Use SQL Server

![image info](/img/blazor/3/2.png)

The new project template in **Visual Studio** will allow you to create a database using [SQL Server Express LocalDB](https://docs.microsoft.com/en-us/aspnet/core/tutorials/first-mvc-app/working-with-sql?view=aspnetcore-2.2&tabs=visual-studio#sql-server-express-localdb). However, it can be problematic to install and configure. Using the free **SQL Server 2019 Developer server** (or the full **SQL Server**) is recommended.

Download and install **SQL Server 2019 Developer Edition** from the following link:

https://www.microsoft.com/en-us/sql-server/sql-server-downloads

## Create The Blazor Application

![image info](/img/blazor/3/3.png)


Open [Visual Studio](https://visualstudio.microsoft.com/).

![image info](/img/blazor/3/4.png)

Select **Create a new Project**.

![image info](/img/blazor/3/5.png)

Select **Blazor App** and click **Next**.

![image info](/img/blazor/3/6.png)

Name it **EndToEnd** and click **Create**.

![image info](/img/blazor/3/7.png)

Select **Blazor Server App**.

![image info](/img/blazor/3/8.png)

Click the Change link under Authentication.

![image info](/img/blazor/3/9.png)

Select **Individual User Accounts** and **Store user accounts in-app.**

![image info](/img/blazor/3/10.png)

Click **Create**.

![image info](/img/blazor/3/11.png)

The **project** will be created.

## Create The Database

![image info](/img/blazor/3/12.png)

Open the **SQL Server Object Explorer**.

![image info](/img/blazor/3/13.png)

Add a connection to your local database server if you don’t already have it in the SQL Server list.

For this tutorial, we do not want to use the SQL Express server on (localdb) that you may already see in the list.

![image info](/img/blazor/3/14.png)

You will specify just the server and **Connect**.

![image info](/img/blazor/3/15.png)

Expand the tree under the local **SQL server**, right-click on the **Databases** folder and select **Add New Database**.

![image info](/img/blazor/3/16.png)

Give the **database** a name and press **Enter**.

The **database** will be created.

![image info](/img/blazor/3/17.png)

**Right-Click** on the **Database** node and select **Properties**.

![image info](/img/blazor/3/18.png)

Open the **Properties** window if it is not already opened.

The **Properties** window for the **database** will display.

**Copy** the **Connection string** for the **database**.

![image info](/img/blazor/3/19.png)

Open the **appsettings.json** file.

![image info](/img/blazor/3/20.png)

**Paste** in the **connection string** for the **DefaultConnection** and **save** the file.

## Run the Application

![image info](/img/blazor/3/21.png)

Hit **F5** to run the application.

![image info](/img/blazor/3/22.png)

The application will open in your **web browser**.

**Click** the **Register** link.

![image info](/img/blazor/3/23.png)

Enter the information to create a **new account**.

Click the **Register** button.

![image info](/img/blazor/3/24.png)

Because this is the first time the database is being used, you will see a message asking you to run the **migration scripts** that will create the database objects needed to support the user membership code.

Click the **Apply Migrations** button.

![image info](/img/blazor/3/25.png)

After the message changes to Migrations Applied, refresh the page in the web browser.

![image info](/img/blazor/3/26.png)

After clicking refresh in your web browser, a popup will require you to click **Continue**.

![image info](/img/blazor/3/27.png)

Click the **Click here to confirm your account link**.

![image info](/img/blazor/3/28.png)

On the **Confirm email** page, click the name of the application to navigate to the home page.

![image info](/img/blazor/3/29.png)

Now you can click the **Log in** link to log in using the account you just created.

![image info](/img/blazor/3/30.png)

You will now be **logged into the application**.

You can click around the application and see that it works.

![image info](/img/blazor/3/31.png)

The **Fetch data** page currently shows random data. We will alter the application to allow us to **add, update, and delete** this data in the database.

**Close** the ***web browser*** to **stop** the application.

## Do Not Require Account Confirmation

![image info](/img/blazor/3/32.png)


If we do not intend to configure **email account verification** (using the directions at this link: https://bit.ly/2tf2ym4), we can open the **Startup.cs** file and change the following line:

```dotnetcli
services.AddDefaultIdentity<IdentityUser>(
                options => options.SignIn.RequireConfirmedAccount = true)
                .AddEntityFrameworkStores<ApplicationDbContext>();
```

To

```dotnetcli

services.AddDefaultIdentity<IdentityUser>(
                options => options.SignIn.RequireConfirmedAccount = false)
                .AddEntityFrameworkStores<ApplicationDbContext>();
```

## Create The Database

Create a new **Server Side Blazor** project and *right-click* on the **wwwroot** folder and select **Add** then **New Item…**

![image info](/img/blazor/3/33.png)

In the **SQL Server Object Explorer window**, in **Visual Studio**, we see the **tables** that the ***migration scripts added***.

![image info](/img/blazor/3/34.png)

*Right-click* on the **Tables** node and select **Add New Table**.

![image info](/img/blazor/3/35.png)

Paste the following script in the **T-SQL** window and then click the **Update** button:

```
CREATE TABLE [dbo].[WeatherForecast] (
    [Id]           INT           IDENTITY (1, 1) NOT NULL,
    [Date]         DATETIME      NULL,
    [TemperatureC] INT           NULL,
    [TemperatureF] INT           NULL,
    [Summary]      NVARCHAR (50) NULL,
    [UserName]     NVARCHAR (50) NULL,
    PRIMARY KEY CLUSTERED ([Id] ASC)
);
```
![image info](/img/blazor/3/36.png)

The script will prepare.

Click the **Update Database** button.

![image info](/img/blazor/3/37.png)

Back in the **Server Explorer** window, *right-click* on Tables and select **Refresh**.

![image info](/img/blazor/3/38.png)

The **WeatherForecast** table will display.

*Right-click* on the table and select **Show Table Data**.

![image info](/img/blazor/3/39.png)

We will enter some **sample data** so that we will be able to **test** the data connection in the next steps.

Set the **UserName** field to the **username** of the **user** that we **registered** an account for earlier.

## Create The Data Context

![image info](/img/blazor/3/40.png)

If you do not already have it installed, install EF Core Power Tools from:

https://marketplace.visualstudio.com/items?itemName=ErikEJ.EFCorePowerTools


![image info](/img/blazor/3/41.png)

(Note: Before installing, close **Visual Studio**)

(Note: Please give this project a **5** star review on **marketplace.visualstudio.com**!)

![image info](/img/blazor/3/42.png)

*Right-click* on the **project** node in the **Solution Explorer** and select **EF Core Power** Tools then **Reverse Engineer**.

![image info](/img/blazor/3/43.png)

Click the **Add** button.

![image info](/img/blazor/3/44.png)

*Connect* to the **database**.

Select the **database connection** in the dropdown and click **OK**.

![image info](/img/blazor/3/45.png)

Select the **WeatherForecast** table and click **OK**.

![image info](/img/blazor/3/46.png)

Set the values and click **OK**.

![image info](/img/blazor/3/47.png)

In the **Solution Explorer**, you will see the **Data Context** has been created.

Click the **OK** button to close the popup.

![image info](/img/blazor/3/48.png)

Open the **Startup.cs** file.

Add the following code to the **ConfigureServices** section:

```dotnetcli

// Read the connection string from the appsettings.json file
    // Set the database connection for the EndtoEndContext
    services.AddDbContext<EndToEndDB.Data.EndToEnd.EndtoEndContext>(options =>
    options.UseSqlServer(
        Configuration.GetConnectionString("DefaultConnection")));

```

Also, change this line:


```dotnetcli
    
    services.AddSingleton<WeatherForecastService>();
```
           


to this:

```dotnetcli
    
    // Scoped creates an instance for each user
    services.AddScoped<WeatherForecastService>();
```

![image info](/img/blazor/3/49.png)

**Save** the file.

Select **Build**, then **Rebuild Solution**.

## Read From The Database

![image info](/img/blazor/3/50.png)
            
**Delete** the **Data/WeatherForecast.cs** file in the project.

We will use the **Data/EndToEnd/WeatherForcast.cs** class file created by the **EF Core Tools instead**.

![image info](/img/blazor/3/51.png)

Open the **WeatherForecastService.cs** file.

**Replace all the code** with the following code:

```dotnetcli

using EndToEndDB.Data.EndToEnd;
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
namespace EndToEnd.Data
{
    public class WeatherForecastService
    {
        private readonly EndtoEndContext _context;
        public WeatherForecastService(EndtoEndContext context)
        {
            _context = context;
        }
        public async Task<List<WeatherForecast>>
            GetForecastAsync(string strCurrentUser)
        {
            // Get Weather Forecasts  
            return await _context.WeatherForecast
                 // Only get entries for the current logged in user
                 .Where(x => x.UserName == strCurrentUser)
                 // Use AsNoTracking to disable EF change tracking
                 // Use ToListAsync to avoid blocking a thread
                 .AsNoTracking().ToListAsync();
        }
    }
}

```

to this

```dotnetcli

    private readonly EndtoEndContext _context;
    
    public WeatherForecastService(EndtoEndContext context)
        {
            _context = context;
        }

```

Connects to the **database** using the *datacontext* we created with the **EF Core tools**.

![image info](/img/blazor/3/52.png)   

Finally, open the **FetchData.razor** file.

*Replace all the code* with the following code:


```dotnetcli

@page "/fetchdata"
@using EndToEnd.Data
@using EndToEndDB.Data.EndToEnd
@inject AuthenticationStateProvider AuthenticationStateProvider
@*
    Using OwningComponentBase ensures that the service and related services
    that share its scope are disposed with the component.
    Otherwise DbContext in ForecastService will live for the life of the
    connection, which may be problematic if clients stay
    connected for a long time.
    We access WeatherForecastService using @Service
*@
@inherits OwningComponentBase<WeatherForecastService>
<h1>Weather forecast</h1>
<!-- AuthorizeView allows us to only show sections of the page -->
<!-- based on the security on the current user -->
<AuthorizeView>
    <!-- Show this section if the user is logged in -->
    <Authorized>
        <h4>Hello, @context.User.Identity.Name!</h4>
        @if (forecasts == null)
        {
            <!-- Show this if the current user has no data... yet... -->
            <p><em>Loading...</em></p>
        }
        else
        {
            <!-- Show the forecasts for the current user -->
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
                            <td>@forecast.Date.Value.ToShortDateString()</td>
                            <td>@forecast.TemperatureC</td>
                            <td>@forecast.TemperatureF</td>
                            <td>@forecast.Summary</td>
                        </tr>
                    }
                </tbody>
            </table>
        }
    </Authorized>
    <!-- Show this section if the user is not logged in -->
    <NotAuthorized>
        <p>You're not signed in.</p>
    </NotAuthorized>
</AuthorizeView>
```

```dotnetcli


@code {
    // AuthenticationState is available as a CascadingParameter
    [CascadingParameter]
    private Task<AuthenticationState> authenticationStateTask { get; set; }
    List<WeatherForecast> forecasts;
    protected override async Task OnInitializedAsync()
    {
        // Get the current user
        var user = (await authenticationStateTask).User;
        // Get the forecasts for the current user
        // We access WeatherForecastService using @Service
        forecasts = await @Service.GetForecastAsync(user.Identity.Name);
    }
}

```

This code simply calls the **GetForecastAsync** method we created in the previous step, passing the **username** of the currently logged in user.

In a normal web application we would have to make a *http web call from this client code to the code that connects to the database*.

With server-side **Blazor** we don’t have to do that, yet the call is still secure because the pages are rendered on the server.

![image info](/img/blazor/3/53.png)   

**Build** and run the project.

If we are not logged in, and we go to the **Fetch data** page, we will see a message indicating we are not signed in.

![image info](/img/blazor/3/54.png)   

Click the **Login** button.

![image info](/img/blazor/3/55.png)   

*Log in as the user we created data* for earlier.

![image info](/img/blazor/3/56.png)  

After you are logged in, switch to the **Fetch data** page and you will see the data for the user we entered earlier.

## Inserting Data Into The Database

![image info](/img/blazor/3/57.png)  

Open the **WeatherForecastService.cs** file and add the following method:

```dotnetcli
public Task<WeatherForecast>
            CreateForecastAsync(WeatherForecast objWeatherForecast)
        {
            _context.WeatherForecast.Add(objWeatherForecast);
            _context.SaveChanges();
            return Task.FromResult(objWeatherForecast);
        }

```

![image info](/img/blazor/3/58.png)  

Open the **FetchData.razor** file.

Add the following HTML markup directly under the existing table element:

```dotnetcli
    <p>
        <!-- Add a new forecast -->
        <button class="btn btn-primary"
                @onclick="AddNewForecast">
            Add New Forecast
        </button>
    </p>
```

This adds a button to allow a new forecast to be added.

Add the following code below the previous code:

```dotnetcli
    @if (ShowPopup)
        {
            <!-- This is the popup to create or edit a forecast -->
            <div class="modal" tabindex="-1" style="display:block" role="dialog">
                <div class="modal-dialog">
                    <div class="modal-content">
                        <div class="modal-header">
                            <h3 class="modal-title">Edit Forecast</h3>
                            <!-- Button to close the popup -->
                            <button type="button" class="close"
                                    @onclick="ClosePopup">
                                <span aria-hidden="true">X</span>
                            </button>
                        </div>
                        <!-- Edit form for the current forecast -->
                        <div class="modal-body">
                            <input class="form-control" type="text"
                                    placeholder="Celsius forecast"
                                    @bind="objWeatherForecast.TemperatureC" />
                            <input class="form-control" type="text"
                                    placeholder="Fahrenheit forecast"
                                    @bind="objWeatherForecast.TemperatureF" />
                            <input class="form-control" type="text"
                                    placeholder="Summary"
                                    @bind="objWeatherForecast.Summary" />
                            <br />
                            <!-- Button to save the forecast -->
                            <button class="btn btn-primary"
                                    @onclick="SaveForecast">
                                Save
                            </button>
                        </div>
                    </div>
                </div>
            </div>
        }
```

This adds a form (that will be displayed a popup because the class for the DIV is modal), that allows the user to enter (and later edit) data for a forecast.

We do not need JavaScript to make this **popup** show. We only need to wrap this code with:

```dotnetcli
     @if (ShowPopup)
        {
            ...
        }
```

           


When the **ShowPopup** value is true the **popup** will show. When the value is false, the **popup** will disappear.

Add the following code to the *@code* section:


```dotnetcli


    WeatherForecast objWeatherForecast = new WeatherForecast();
    bool ShowPopup = false;
    void ClosePopup()
    {
        // Close the Popup
        ShowPopup = false;
    }
    void AddNewForecast()
    {
        // Make new forecast
        objWeatherForecast = new WeatherForecast();
        // Set Id to 0 so we know it is a new record
        objWeatherForecast.Id = 0;
        // Open the Popup
        ShowPopup = true;
    }
    async Task SaveForecast()
    {
        // Close the Popup
        ShowPopup = false;
        // Get the current user
        var user = (await authenticationStateTask).User;
        // A new forecast will have the Id set to 0
        if (objWeatherForecast.Id == 0)
        {
            // Create new forecast
            WeatherForecast objNewWeatherForecast = new WeatherForecast();
            objNewWeatherForecast.Date = System.DateTime.Now;
            objNewWeatherForecast.Summary = objWeatherForecast.Summary;
            objNewWeatherForecast.TemperatureC =
            Convert.ToInt32(objWeatherForecast.TemperatureC);
            objNewWeatherForecast.TemperatureF =
            Convert.ToInt32(objWeatherForecast.TemperatureF);
            objNewWeatherForecast.UserName = user.Identity.Name;
            // Save the result
            var result =
            @Service.CreateForecastAsync(objNewWeatherForecast);
        }
        else
        {
            // This is an update
        }
        // Get the forecasts for the current user
        forecasts =
        await @Service.GetForecastAsync(user.Identity.Name);
    }
```

![image info](/img/blazor/3/59.png)  

When you run the project, you can click the **Add New Forecast** button to add an entry.

![image info](/img/blazor/3/60.png)  

The form only requires a **Fahrenheit, Celsius, and a summary**, because the other values (*date and username*), will be set by the code.

![image info](/img/blazor/3/61.png)

After clicking the **Save** button, the entry is saved to the database and displayed.

## Updating The Data

![image info](/img/blazor/3/62.png)

Open the **WeatherForecastService.cs** file and **add** the following method:

```dotnetcli
     public Task<bool>
            UpdateForecastAsync(WeatherForecast objWeatherForecast)
        {
            var ExistingWeatherForecast =
                _context.WeatherForecast
                .Where(x => x.Id == objWeatherForecast.Id)
                .FirstOrDefault();
            if (ExistingWeatherForecast != null)
            {
                ExistingWeatherForecast.Date =
                    objWeatherForecast.Date;
                ExistingWeatherForecast.Summary =
                    objWeatherForecast.Summary;
                ExistingWeatherForecast.TemperatureC =
                    objWeatherForecast.TemperatureC;
                ExistingWeatherForecast.TemperatureF =
                    objWeatherForecast.TemperatureF;
                _context.SaveChanges();
            }
            else
            {
                return Task.FromResult(false);
            }
            return Task.FromResult(true);
        }
```

![image info](/img/blazor/3/63.png)

Open the **FetchData.razor** file.

**Replace** the existing **table** element with the following markup that adds an edit button in the last column (that calls the **EditForecast** method we will add in the next step):

```dotnetcli

    <table class="table">
        <thead>
            <tr>
                <th>Date</th>
                <th>Temp. (C)</th>
                <th>Temp. (F)</th>
                <th>Summary</th>
                <th></th>
            </tr>
        </thead>
        <tbody>
            @foreach (var forecast in forecasts)
            {
                <tr>
                    <td>@forecast.Date.Value.ToShortDateString()</td>
                    <td>@forecast.TemperatureC</td>
                    <td>@forecast.TemperatureF</td>
                    <td>@forecast.Summary</td>
                    <td>
                        <!-- Edit the current forecast -->
                        <button class="btn btn-primary"
                                @onclick="(() => EditForecast(forecast))">
                            Edit
                        </button>
                    </td>
                </tr>
            }
        </tbody>
    </table>



```

Add the following code to the *@code* section:

```dotnetcli
    void EditForecast(WeatherForecast weatherForecast)
    {
        // Set the selected forecast
        // as the current forecast
        objWeatherForecast = weatherForecast;
        // Open the Popup
        ShowPopup = true;
    }
```

This sets the current record to the **objWeatherForecast** object that the **popup** is bound to, and opens the **popup**.

Finally, change the existing **SaveForecast** method to the following:

```dotnetcli
    
    async Task SaveForecast()
    {
        // Close the Popup
        ShowPopup = false;
        // Get the current user
        var user = (await authenticationStateTask).User;
        // A new forecast will have the Id set to 0
        if (objWeatherForecast.Id == 0)
        {
            // Create new forecast
            WeatherForecast objNewWeatherForecast = new WeatherForecast();
            objNewWeatherForecast.Date = System.DateTime.Now;
            objNewWeatherForecast.Summary = objWeatherForecast.Summary;
            objNewWeatherForecast.TemperatureC =
            Convert.ToInt32(objWeatherForecast.TemperatureC);
            objNewWeatherForecast.TemperatureF =
            Convert.ToInt32(objWeatherForecast.TemperatureF);
            objNewWeatherForecast.UserName = user.Identity.Name;
            // Save the result
            var result =
            @Service.CreateForecastAsync(objNewWeatherForecast);
        }
        else
        {
            // This is an update
            var result =
            @Service.UpdateForecastAsync(objWeatherForecast);
        }
        // Get the forecasts for the current user
        forecasts =
        await @Service.GetForecastAsync(user.Identity.Name);
    }



```

This simply adds one line:

```dotnetcli
    // This is an update
    var result =  @Service.UpdateForecastAsync(objWeatherForecast);
```

To the existing method to handle an update rather than an insert.            

![image info](/img/blazor/3/64.png)


When we run the application, we now have an **Edit button** to edit the existing record.


![image info](/img/blazor/3/65.png)

The existing record will display in the **popup**, allowing us to *edit the data and save it*.

![image info](/img/blazor/3/66.png)

The updated record is then displayed in the table.

## Deleting The Data

![image info](/img/blazor/3/67.png)

Open the **WeatherForecastService.cs** file and **add** the following method:

```dotnetcli

    public Task<bool>
            DeleteForecastAsync(WeatherForecast objWeatherForecast)
    {
        var ExistingWeatherForecast =
            _context.WeatherForecast
            .Where(x => x.Id == objWeatherForecast.Id)
            .FirstOrDefault();
        if (ExistingWeatherForecast != null)
        {
            _context.WeatherForecast.Remove(ExistingWeatherForecast);
            _context.SaveChanges();
        }
        else
        {
            return Task.FromResult(false);
        }
        return Task.FromResult(true);
    }

```

![image info](/img/blazor/3/68.png)

Open the **FetchData.razor** file.

Add code markup for a **Delete** button under the code markup for the current Save button in the popup:

