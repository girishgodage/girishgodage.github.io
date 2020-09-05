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

Save the file.

Select Build, then Rebuild Solution.


            
