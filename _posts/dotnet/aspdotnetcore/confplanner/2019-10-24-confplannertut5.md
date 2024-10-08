---
title: Add ability to log in to the website
date: 2019-10-24 05:54:00 Z
permalink: "/blog/Addauthfeatures"
categories:
- AspDotnetCore
tags:
- learning
summary: In this module we're going to add the capability for users to register and
  sign-in on the front-end web app with a username and password. We'll do this using
  ASP.NET Core Identity.
image: "/img/aspnetcore.png"
author: Girish Godage
layout: posts
prevurl: "/blog/Addfront-endrenderagendasetupfront-endmodels"
nexturl: "/blog/Addpersonalagenda"
discussion_id: 2019-10-24-confplannertut5
---

# Add ability to log in to the website

In this module we're going to add the capability for users to register and sign-in on the front-end web app with a username and password. We'll do this using ASP.NET Core Identity.

## Scaffold in ASP.NET Core Identity and Default UI
> We'll start by scaffolding the default Identity experience into the front-end web app.

### Adding Identity using Visual Studio
1. Right-mouse click on the FrontEnd project in Solution Explorer and select "Add" and then "New Scaffolded Item..."
![](/img/aspdotnetcore/confplanner/5/60.png)
1. Select the "Identity" category from the left-hand menu and then select "Identity" from the list and click the "Add" button
![](/img/aspdotnetcore/confplanner/5/61.png)
1. In the "Add Identity" dialog, click the '+' button to add a data context class. Call it `FrontEnd.Data.IdentityDbContext`.
1. In the same dialog, click the '+' button to add a user class. Call it `FrontEnd.Data.User`.
![](/img/aspdotnetcore/confplanner/5/64.png)
1. Click the "Add" button

### Adding Identity using the Command Line
1. Open a command line to the **FrontEnd** project folder
1. If you haven't done this already, install the command-line scaffolding tool.
   ```
   dotnet tool install -g dotnet-aspnet-codegenerator
   ```
1. Add the `Microsoft.VisualStudio.Web.CodeGeneration.Design` version `3.0.0` package to the project.
   ```
   dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0
   ```
1. Run this command to generate the code to use Identity.
   ```
   dotnet aspnet-codegenerator identity --dbContext FrontEnd.Data.IdentityDbContext --userClass FrontEnd.Data.User --useDefaultUI
   ```

## Organize the newly created files
> Note the new files added to the project in the "Areas/Identity" folder. We're going to clean these up a little to better match this project's conventions.

1. Delete the `_ValidationScriptsPartial.cshtml` file in the `/Areas/Identity/Pages` folder, as we already have one in our project's regular pages folder.
1. Delete the `ScaffoldingReadme.txt` file.

## Add the Identity links to the site header
> The scaffolded out Identity system includes a Razor partial view that contains the Identity-related UI for the site header, e.g. Login and Register links, user name once logged in, etc. We need to add a call to this partial from our site's own layout page:

1. Open the `_Layout.cshtml` file and find the following line: `<div class="navbar-collapse collapse d-sm-inline-flex flex-sm-row-reverse">`
1. Immediately after this line, add a call to render the newly added `_LoginPartial.cshtml` using the `<partial />` Tag Helper:
    ``` html
    <div class="navbar-collapse collapse d-sm-inline-flex flex-sm-row-reverse">
        <partial name="_LoginPartial" />
        <ul class="navbar-nav flex-grow-1">
            <li class="nav-item">
                <a class="nav-link text-dark" asp-area="" asp-page="/Index">Home</a>
            </li>
        </ul>
    </div>
    ```

## Update the app to support admin users
> Identity supports simple customization of the classes representing users, and when using the default Entity Framework Core, these changes will result in automatic schema updates for storage. We can also customize the default Identity UI by just scaffolding in the pages we want to change. Let's add the ability to create an admin user.

### Customize the `User` class to support admin users
1. Open the newly created `User` class in the `/Areas/Identity/Data` folder
1. Add a `bool` property called `IsAdmin` to indicate whether the user is an admin:
    ``` c#
    public class User : IdentityUser
    {
        public bool IsAdmin { get; set; }
    }
    ```

### Generate the Entity Framework migration for our Identity schema

#### Visual Studio: Package Manager Console 

1. In Visual Studio, select the Tools -> NuGet Package Manager -> Package Manager Console

1. Run the following commands in the Package Manager Console
   ```console
   Add-Migration CreateIdentitySchema
   Update-Database
   ```
![](/img/aspdotnetcore/confplanner/5/65.png)
#### Command line

1. Run the following commands in the command prompt:
    ```console
    dotnet build
    dotnet ef migrations add CreateIdentitySchema
    dotnet ef database update
    ```

### Add the authentication middleware
> We need to ensure that the request pipeline contains the Authentication middleware before any other middleware that represents resources we want to potentially authorize, e.g. Razor Pages
1. Open the `Startup.cs` file
1. In the `Configure` method, add a call to add the Authentication middleware before the Authorization middleware:
   ``` c#
    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }
        else
        {
            app.UseExceptionHandler("/Error");
            app.UseHsts();
        }

        app.UseHttpsRedirection();
        app.UseStaticFiles();

        app.UseRouting();

        app.UseAuthentication();
        app.UseAuthorization();

        app.UseEndpoints(endpoints =>
        {
            endpoints.MapRazorPages();
        });
    }
    ```

### Allow creation of an admin user
> Let's make it so the site allows creation of an admin user when there isn't one already. The first user to access the site will be deemed the administrator.

1. Create a new class `AdminService` in the `Services` folder. This class will be responsible for managing the creation key generation and tracking whether the site should allow creating admin users.
    ``` c#
    public class AdminService
    {
        private readonly IdentityDbContext _dbContext;

        private bool _adminExists;

        public AdminService(IdentityDbContext dbContext)
        {
            _dbContext = dbContext;
        }

        public async Task<bool> AllowAdminUserCreationAsync()
        {
            if (_adminExists)
            {
                return false;
            }
            else
            {
                if (await _dbContext.Users.AnyAsync(user => user.IsAdmin))
                {
                    // There are already admin users so disable admin creation
                    _adminExists = true;
                    return false;
                }

                // There are no admin users so enable admin creation
                return true;
            }
        }
    }
    ```
1. Extract an interface from the class and call it `IAdminService`
    ``` c#
    public interface IAdminService
    {
        Task<bool> AllowAdminUserCreationAsync();
    }
    ```
1. In the `Startup` class, modify the `ConfigureServices` method to add the new service to the DI container:
    ``` c#
    services.AddSingleton<IAdminService, AdminService>();
    ```

> We now need to override the default Register page to enable creating the admin account when the first user is registered.

1. Run the Identity scaffolder again, but this time select the `Account\Register` page in the list of files to override and select the `IdentityDbContext (FrontEnd.Data)`

![](/img/aspdotnetcore/confplanner/5/67.png)
   * On command line, run
     ```
     dotnet aspnet-codegenerator identity --dbContext FrontEnd.Data.IdentityDbContext --files Account.Register
     ```
1. Update the `RegisterModel` class in the `Register.cshtml.cs` file to accept `IAdminService` as a parameter and it them to a local member:
    ``` c#
    [AllowAnonymous]
    public class RegisterModel : PageModel
    {
        private readonly SignInManager<User> _signInManager;        
        private readonly UserManager<User> _userManager;
        private readonly ILogger<RegisterModel> _logger;
        private readonly IEmailSender _emailSender;
        private readonly IAdminService _adminService;

        public RegisterModel(
            UserManager<User> userManager,
            SignInManager<User> signInManager,
            ILogger<RegisterModel> logger,
            IEmailSender emailSender,
            IAdminService adminService)
        {
            _userManager = userManager;
            _signInManager = signInManager;
            _logger = logger;
            _emailSender = emailSender;
            _adminService = adminService;
        }

        ...
    ```

1. Add code to the `OnPostAsync` that marks the new user as an admin if the `IAdminService.AllowAdminUserCreationAsync` returns true before creating the user:
    ``` c#
    if (await _adminService.AllowAdminUserCreationAsync())
    {
        // Set as admin user
        user.IsAdmin = true;
    }

    var result = await _userManager.CreateAsync(user, Input.Password);
    ```
1. Update the code that logs a message when users are created to indicate when an admin user is created:
    ``` c#
    if (user.IsAdmin)
    {
        _logger.LogInformation("Admin user created a new account with password.");
    }
    else
    {
        _logger.LogInformation("User created a new account with password.");
    }
    ```

> If you run the app at this point, you'll see an exception stating that you can't inject a scoped type into a type registered as a singleton. This is the DI system protecting you from a common anti-pattern that can arise when using IoC containers. Let's fix the `AdminService` to use the scoped `IdentityDbContext` correctly.

1. Open the `AdminService.cs` file and change the code to accept an `IServiceProvider` instead of the `IdentityDbContext` in its constructor:
    ``` csharp
    public class AdminService : IAdminService
    {
        private readonly IServiceProvider _serviceProvider;

        private bool _adminExists;

        public AdminService(IServiceProvider serviceProvider)
        {
            _serviceProvider = serviceProvider;
        }

        // ...
    ```

    ![](/img/aspdotnetcore/confplanner/5/68.png)
1. Now update the `AllowAdminUserCreationAsync` method to create a service scope so we can ask for an instance of the `IdentityDbContext` within a scoped context:
    ``` csharp
    public async Task<bool> AllowAdminUserCreationAsync()
    {
        if (_adminExists)
        {
            return false;
        }
        else
        {
            using (var scope = _serviceProvider.CreateScope())
            {
                var dbContext = scope.ServiceProvider.GetRequiredService<IdentityDbContext>();

                if (await dbContext.Users.AnyAsync(user => user.IsAdmin))
                {
                    // There are already admin users so disable admin creation
                    _adminExists = true;
                    return false;
                }

                // There are no admin users so enable admin creation
                return true;
            }
        }
    }
    ```
1. Re-launch the application and now you shouldn't get an exception.

 ![](/img/aspdotnetcore/confplanner/5/69.png)

  ![](/img/aspdotnetcore/confplanner/5/70.png)
# Adding admin section

## Add an admin policy
> Rather than looking up the user in the database each time the app needs to check if a user is an admin, we can read this information once when the user logs in, then store it as an additional claim on the user identity. We also need to add an authoriation policy to the app that corresponds to this claim, that we can use to protect resources we only want admins to be able to access.

1. Add a new class `ClaimsPrincipalFactory` in the `/Areas/Identity` folder and add code that adds an admin claim for users who are admins:
    ``` c#
    public class ClaimsPrincipalFactory : UserClaimsPrincipalFactory<User>
    {
        private readonly IApiClient _apiClient;

        public ClaimsPrincipalFactory(IApiClient apiClient, UserManager<User> userManager, IOptions<IdentityOptions> optionsAccessor)
            : base(userManager, optionsAccessor)
        {
            _apiClient = apiClient;
        }

        protected override async Task<ClaimsIdentity> GenerateClaimsAsync(User user)
        {
            var identity = await base.GenerateClaimsAsync(user);

            if (user.IsAdmin)
            {
                identity.MakeAdmin();
            }

            return identity;
        }
    }
    ```
1. Add a new class file `AuthHelpers.cs` in the `Infrastructure` folder and add the following helper methods for reading and setting the admin claim:
    ``` c#
    namespace FrontEnd.Infrastructure
    {
        public static class AuthConstants
        {
            public static readonly string IsAdmin = nameof(IsAdmin);
            public static readonly string IsAttendee = nameof(IsAttendee);
            public static readonly string TrueValue = "true";
        }
    }

    namespace System.Security.Claims
    {
        public static class AuthnHelpers
        {
            public static bool IsAdmin(this ClaimsPrincipal principal) =>
                principal.HasClaim(AuthConstants.IsAdmin, AuthConstants.TrueValue);

            public static void MakeAdmin(this ClaimsPrincipal principal) =>
                principal.Identities.First().MakeAdmin();

            public static void MakeAdmin(this ClaimsIdentity identity) =>
                identity.AddClaim(new Claim(AuthConstants.IsAdmin, AuthConstants.TrueValue));
        }
    }

    namespace Microsoft.Extensions.DependencyInjection
    {
        public static class AuthzHelpers
        {
            public static AuthorizationPolicyBuilder RequireIsAdminClaim(this AuthorizationPolicyBuilder builder) =>
                builder.RequireClaim(AuthConstants.IsAdmin, AuthConstants.TrueValue);
        }
    }
    ```

1. Register the custom `UserClaimsPrincipalFactory<User>` in the `IdentityHostingStartup` class:

    ```csharp
    services.AddDefaultIdentity<User>()
            .AddEntityFrameworkStores<IdentityDbContext>()
            .AddClaimsPrincipalFactory<ClaimsPrincipalFactory>();
    ```
1. Add authorization services with an admin policy to the `ConfigureServices()` method of `Startup.cs` that uses the just-added helper methods to require the admin claim:

    ```csharp
    services.AddAuthorization(options =>
    {
        options.AddPolicy("Admin", policy =>
        {
            policy.RequireAuthenticatedUser()
                  .RequireIsAdminClaim();
        });
    });
    ```
    ![](/img/aspdotnetcore/confplanner/5/71.png)
1. Add `System.Security.Claims` to the list of usings in `Index.cshtml.cs`, then use the helper method in the page model to determine if the current user is an administrator.

    ```csharp
    public bool IsAdmin { get; set; }

    public async Task OnGetAsync(int day = 0)
    {
        IsAdmin = User.IsAdmin();

        // More stuff here
        // ...
    }
    ```
    ![](/img/aspdotnetcore/confplanner/5/72.png)
1. On the `Index` razor page, add an edit link to allow admins to edit sessions. You'll add the following code directly after the session `foreach` loop:

    ```html
	<div class="card-footer">
	    <ul class="list-inline mb-0">
		@foreach (var speaker in session.Speakers)
		{
		    <li class="list-inline-item">
			    <a asp-page="Speaker" asp-route-id="@speaker.Id">@speaker.Name</a>
		    </li>
		}
		@if (Model.IsAdmin)
		{
		    <li>
			    <a asp-page="/Admin/EditSession" asp-route-id="@session.Id" class="btn btn-default btn-xs">Edit</a>
		    </li>
		}
	    </ul>
	</div>
    ```
1. Add a nested `Admin` folder to the `Pages` folder then add an `EditSession.cshtml` razor page and `EditSession.cshtml.cs` page model to it.
1. Next, we'll protect pages in the `Admin` folder with an Admin policy by making the following change to the `services.AddRazorPages()` call in `Startup.ConfigureServices`:

   ```csharp
   services.AddRazorPages(options =>
   {
       options.Conventions.AuthorizeFolder("/Admin", "Admin");
   });
   ```
   ![](/img/aspdotnetcore/confplanner/5/75.png)
    
## Add a form for editing a session
1. Change `EditSession.cshtml.cs` to render the session in the edit form:

   ```csharp
   public class EditSessionModel : PageModel
   {
      private readonly IApiClient _apiClient;

      public EditSessionModel(IApiClient apiClient)
      {
         _apiClient = apiClient;
      }

      public Session Session { get; set; }

      public async Task OnGetAsync(int id)
      {
         var session = await _apiClient.GetSessionAsync(id);
         Session = new Session
         {
             Id = session.Id,
             TrackId = session.TrackId,
             Title = session.Title,
             Abstract = session.Abstract,
             StartTime = session.StartTime,
             EndTime = session.EndTime
         };
      }
   }
   ```

1. Add the "{id}" route to the `EditSession.cshtml` form:

    ```html
    @page "{id}"
    @model EditSessionModel
    ```

1. Add the following edit form to `EditSession.cshtml`:

   ```html
   <h3>Edit Session</h3>

   <form method="post" class="form-horizontal">
       <div asp-validation-summary="All" class="text-danger"></div>
       <input asp-for="Session.Id" type="hidden" />
       <input asp-for="Session.TrackId" type="hidden" />
       <div class="form-group">
           <label asp-for="Session.Title" class="col-md-2 control-label"></label>
           <div class="col-md-10">
               <input asp-for="Session.Title" class="form-control" />
               <span asp-validation-for="Session.Title" class="text-danger"></span>
           </div>
       </div>
       <div class="form-group">
           <label asp-for="Session.Abstract" class="col-md-2 control-label"></label>
           <div class="col-md-10">
               <textarea asp-for="Session.Abstract" class="form-control"></textarea>
               <span asp-validation-for="Session.Abstract" class="text-danger"></span>
           </div>
       </div>
       <div class="form-group">
           <label asp-for="Session.StartTime" class="col-md-2 control-label"></label>
           <div class="col-md-10">
               <input asp-for="Session.StartTime" class="form-control" />
               <span asp-validation-for="Session.StartTime" class="text-danger"></span>
           </div>
       </div>
       <div class="form-group">
           <label asp-for="Session.EndTime" class="col-md-2 control-label"></label>
           <div class="col-md-10">
               <input asp-for="Session.EndTime" class="form-control" />
               <span asp-validation-for="Session.EndTime" class="text-danger"></span>
           </div>
       </div>
       <div class="form-group">
           <div class="col-md-offset-2 col-md-10">
               <button type="submit" class="btn btn-primary">Save</button>
               <button type="submit" asp-page-handler="Delete" class="btn btn-danger">Delete</button>
           </div>
       </div>
   </form>

   @section Scripts {
        <partial name="_ValidationScriptsPartial" />
   }
   ```
1. Add code to handle the `Save` and `Delete` button actions in `EditSession.cshtml.cs`:

   ```csharp
   public async Task<IActionResult> OnPostAsync()
   {
      if (!ModelState.IsValid)
      {
          return Page();
      }

      await _apiClient.PutSessionAsync(Session);

      return Page();
   }

   public async Task<IActionResult> OnPostDeleteAsync(int id)
   {
      var session = await _apiClient.GetSessionAsync(id);

      if (session != null)
      {
          await _apiClient.DeleteSessionAsync(id);
      }

      return Page();
   }
   ```

1. Add a `[BindProperty]` attribute to the `Session` property in `EditSession.cshtml.cs` to make sure properties get bound
on form posts:
   ```csharp
   [BindProperty]
   public Session Session { get; set; }
   ```

1. The form should be fully functional.

## Add success message to form post and use the [PRG](https://en.wikipedia.org/wiki/Post/Redirect/Get) pattern

1. Add a `TempData` decorated `Message` property and a `ShowMessage` property to `EditSession.cshtml.cs`:

   ```csharp
   [TempData]
   public string Message { get; set; }

   public bool ShowMessage => !string.IsNullOrEmpty(Message);
   ```

1. Set a success message in the `OnPostAsync` and `OnPostDeleteAsync` methods and change `Page()` to `RedirectToPage()`:

   ```csharp
   public async Task<IActionResult> OnPostAsync()
   {
      if (!ModelState.IsValid)
      {
          return Page();
      }
      
      Message = "Session updated successfully!";

      await _apiClient.PutSessionAsync(Session);

      return RedirectToPage();
   }

   public async Task<IActionResult> OnPostDeleteAsync(int id)
   {
      var session = await _apiClient.GetSessionAsync(id);

      if (session != null)
      {
          await _apiClient.DeleteSessionAsync(id);
      }
      
      Message = "Session deleted successfully!";

      return RedirectToPage("/Index");
   }
   ```
 ![](/img/aspdotnetcore/confplanner/5/77.png)

 ![](/img/aspdotnetcore/confplanner/5/78.png)
1. Update `EditSession.cshtml` to show the message after posting. Add the following code directly below the `<h3>` tag at the top:

   ```html
   @if (Model.ShowMessage)
   {
       <div class="alert alert-success alert-dismissible" role="alert">
           <button type="button" class="close" data-dismiss="alert" aria-label="Close"><span aria-hidden="true">&times;</span>   </button>
           @Model.Message
       </div>
   }
   ```
 ![](/img/aspdotnetcore/confplanner/5/79.png)
> TempData-backed properties also flow across pages, so we can update the Index page to show the message value too, e.g. when the session is deleted

1. Copy the message display markup from the top of the `EditSession.cshtml` file to the top of the `Index.cshtml` file:
    ``` html
    @if (Model.ShowMessage)
    {
        <div class="alert alert-success alert-dismissible" role="alert">
            <button type="button" class="close" data-dismiss="alert" aria-label="Close"><span aria-hidden="true">&times;</span>   </button>
            @Model.Message
        </div>
    }
    ```
 ![](/img/aspdotnetcore/confplanner/5/80.png)   
1. Copy the properties from the `EditSession.cshtml.cs` Page Model class file to the `Index.cshtml.cs` Page Model too:
    ``` csharp
    [TempData]
   public string Message { get; set; }

   public bool ShowMessage => !string.IsNullOrEmpty(Message);
    ```
![](/img/aspdotnetcore/confplanner/5/81.png)     
1. Rebuild and run the app then delete a session and observe it redirect to the home page and display the success message

## Create a Tag Helper for setting authorization requirements for UI elements
We're currently using `if` blocks to determine whether to show parts of the UI based the user's auth policies. We can clean up this code by creating a custom [Tag Helper](https://docs.microsoft.com/en-us/aspnet/core/mvc/views/tag-helpers/intro).

1. Create a new folder called `TagHelpers` in the root of the *FrontEnd* project. Right-click on the folder, select *Add* / *New Item...* / *Razor Tag Helper*. Name the Tag Helper `AuthzTagHelper.cs`.
1. Modify the `HtmlTargetElement` attribute to bind to all elements with an "authz" attribute:
   ```csharp
   [HtmlTargetElement("*", Attributes = "authz")]
   ```
1. Add an additional `HtmlTargetElement` attribute to bind to all elements with an "authz-policy" attribute:
   ```csharp
   [HtmlTargetElement("*", Attributes = "authz-policy")]
   ```
1. Inject the `AuthorizationService` as shown:
   ```csharp
   private readonly IAuthorizationService _authzService;

   public AuthzTagHelper(IAuthorizationService authzService)
   {
       _authzService = authzService;
   }
   ```
1. Add the following properties which will represent the `auth` and `authz` attributes we're binding to:
   ```csharp
   [HtmlAttributeName("authz")]
   public bool RequiresAuthentication { get; set; }

   [HtmlAttributeName("authz-policy")]
   public string RequiredPolicy { get; set; }
   ```
1. Add a `ViewContext` property:
   ```csharp
   [ViewContext]
   public ViewContext ViewContext { get; set; }
   ```
1. Mark the `ProcessAsync` method as `async`.
1. Add the following code to the `ProcessAsync` method:
   ```csharp
   public override async Task ProcessAsync(TagHelperContext context, TagHelperOutput output)
   {
       var requiresAuth = RequiresAuthentication || !string.IsNullOrEmpty(RequiredPolicy);
       var showOutput = false;

       if (context.AllAttributes["authz"] != null && !requiresAuth && !ViewContext.HttpContext.User.Identity.IsAuthenticated)
       {
           // authz="false" & user isn't authenticated
           showOutput = true;
       }
       else if (!string.IsNullOrEmpty(RequiredPolicy))
       {
           // authz-policy="foo" & user is authorized for policy "foo"
           var authorized = false;
           var cachedResult = ViewContext.ViewData["AuthPolicy." + RequiredPolicy];
           if (cachedResult != null)
           {
               authorized = (bool)cachedResult;
           }
           else
           {
               var authResult = await _authzService.AuthorizeAsync(ViewContext.HttpContext.User, RequiredPolicy);
               authorized = authResult.Succeeded;
               ViewContext.ViewData["AuthPolicy." + RequiredPolicy] = authorized;
           }

           showOutput = authorized;
       }
       else if (requiresAuth && ViewContext.HttpContext.User.Identity.IsAuthenticated)
       {
           // authz="true" & user is authenticated
           showOutput = true;
       }

       if (!showOutput)
       {
           output.SuppressOutput();
       }
   }
   ```
1. Register the new Tag Helper in the `_ViewImports.cshtml` file:
   ```html
   @namespace FrontEnd.Pages
   @addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
   @addTagHelper *, FrontEnd
   ```
1. We can now update the `Index.cshtml` page to replace the `if` block which controls the Edit button's display with declarative code using our new Tag Helper. Remove the `if` block and add `authz="true` to the `<a>` which displays the edit button:
   ```html
	<div class="card-footer">
	    <ul class="list-inline mb-0">
		@foreach (var speaker in session.Speakers)
		{
		    <li class="list-inline-item">
			<a asp-page="Speaker" asp-route-id="@speaker.Id">@speaker.Name</a>
		    </li>
		}
        </ul>
        <a authz-policy="Admin" asp-page="/Admin/EditSession" asp-route-id="@session.Id" class="btn btn-default btn-xs">Edit</a>
	</div>
    ```
    ![](/img/aspdotnetcore/confplanner/5/83.png) 
    ![](/img/aspdotnetcore/confplanner/5/85.png) 
    ![](/img/aspdotnetcore/confplanner/5/86.png)
    ![](/img/aspdotnetcore/confplanner/5/87.png)
    ![](/img/aspdotnetcore/confplanner/5/88.png)
    ![](/img/aspdotnetcore/confplanner/5/89.png)
    ![](/img/aspdotnetcore/confplanner/5/90.png)
    ![](/img/aspdotnetcore/confplanner/5/91.png)
    ![](/img/aspdotnetcore/confplanner/5/92.png)

---
{% include conf_sessions.md %}

{% include blogslide.html %}

