---
title: Building the Front End
date: 2019-10-24 05:54:00 Z
permalink: "/blog/Addfront-endrenderagendasetupfront-endmodels"
categories:
- AspDotnetCore
tags:
- learning
summary: In this tutorial, we'll add the front end web site, with a public (anonymous)
  home page showing the conference agenda
image: "/img/aspnetcore.png"
author: Girish Godage
layout: posts
prevurl: "/blog/BuildoutBackEndandRefactor"
nexturl: "/blog/Addauthfeatures"
discussion_id: 2019-10-24-confplannertut4
---

## Building the Front End

In this session, we'll add the front end web site, with a public anonymous home page showing the conference agenda.

## Add a FrontEnd project

> We'll start by creating the new front end project for the web site.

### Adding the FrontEnd Project using Visual Studio

1. If using Visual Studio, right-click on the Solution and select *Add* / *New Project...*.
1. Select *.NET Core* from the project types on the left and select the *ASP.NET Core Web Application* template. Name the project "FrontEnd", name the solution "ConferencePlanner", and press OK.

![](/img/aspdotnetcore/confplanner/4/45.png)

1. Select *ASP.NET Core 3.0* from the drop-down list in the top-left corner
1. Select the *Web Application* template and click *OK*

![](/img/aspdotnetcore/confplanner/4/44.png)

1. Right-click on the *FrontEnd* project and add a reference to the *ConferenceDTO* project.

### Adding the FrontEnd Project via the Command Line

1. Open a command prompt and navigate to the root `ConferencePlanner` directory.
1. Run the following command:

   

``` console
   dotnet new webapp -o FrontEnd
   ```

1. Next we'll need to add a reference to the ConferenceDTO project from the new FrontEnd project. From the command line, navigate to the FrontEnd project directory and execute the following command:

   

``` console
   dotnet add reference ../ConferenceDTO/ConferenceDTO.csproj
   ```

## Delete unwanted content

> We'll clear out some content from the template that we don't need

1. Open */Pages/Index.cshtml* and delete all the HTML content (after line 6)

![](/img/aspdotnetcore/confplanner/4/46.png)

## Create and wire-up an API service client

> We'll create a class to talk to our backend web API service

### Create the API service client class

1. Create a folder called *Services* in the root of the project
1. In this folder, add a new interface called `IApiClient` with the following members:

   

``` csharp
   using System;
   using System.Collections.Generic;
   using System.Linq;
   using System.Threading.Tasks;
   using ConferenceDTO;

   namespace FrontEnd.Services
   {
       public interface IApiClient
       {
          Task<List<SessionResponse>> GetSessionsAsync();
          Task<SessionResponse> GetSessionAsync(int id);
          Task<List<SpeakerResponse>> GetSpeakersAsync();
          Task<SpeakerResponse> GetSpeakerAsync(int id);
          Task PutSessionAsync(Session session);
          Task<bool> AddAttendeeAsync(Attendee attendee);
          Task<AttendeeResponse> GetAttendeeAsync(string name);
          Task DeleteSessionAsync(int id);
       }
   }
   ```

   ![](/img/aspdotnetcore/confplanner/4/47.png)

1. Add a reference to the *Microsoft.AspNet.WebApi.Client* NuGet package in the FrontEnd project:

   
   

``` 
   dotnet add package Microsoft.AspNet.WebApi.Client
   ```

   ![](/img/aspdotnetcore/confplanner/4/48.png)     

1. Staying in this folder, add a new class called `ApiClient` that implements the `IApiClient` interface by using `HttpClient` to call out to our BackEnd API application and JSON serialize/deserialize the payloads:

   

``` csharp
   using System;
   using System.Collections.Generic;
   using System.Linq;
   using System.Net;
   using System.Net.Http;
   using System.Threading.Tasks;
   using ConferenceDTO;

   namespace FrontEnd.Services
   {
       public class ApiClient : IApiClient
       {
           private readonly HttpClient _httpClient;

           public ApiClient(HttpClient httpClient)
           {
               _httpClient = httpClient;
           }

           public async Task<bool> AddAttendeeAsync(Attendee attendee)
           {
               var response = await _httpClient.PostAsJsonAsync($"/api/attendees", attendee);
                
               if (response.StatusCode == HttpStatusCode.Conflict)
               {
                   return false;
               }

               response.EnsureSuccessStatusCode();
                
               return true;
           }

           public async Task<AttendeeResponse> GetAttendeeAsync(string name)
           {
               if (string.IsNullOrEmpty(name))
               {
                   return null;
               }

               var response = await _httpClient.GetAsync($"/api/attendees/{name}");

               if (response.StatusCode == HttpStatusCode.NotFound)
               {
                   return null;
               }

               response.EnsureSuccessStatusCode();

               return await response.Content.ReadAsAsync<AttendeeResponse>();
           }

           public async Task<SessionResponse> GetSessionAsync(int id)
           {
               var response = await _httpClient.GetAsync($"/api/sessions/{id}");

               if (response.StatusCode == HttpStatusCode.NotFound)
               {
                   return null;
               }

               response.EnsureSuccessStatusCode();

               return await response.Content.ReadAsAsync<SessionResponse>();
           }

           public async Task<List<SessionResponse>> GetSessionsAsync()
           {
               var response = await _httpClient.GetAsync("/api/sessions");

               response.EnsureSuccessStatusCode();

               return await response.Content.ReadAsAsync<List<SessionResponse>>();
           }

           public async Task DeleteSessionAsync(int id)
           {
               var response = await _httpClient.DeleteAsync($"/api/sessions/{id}");

               if (response.StatusCode == HttpStatusCode.NotFound)
               {
                   return;
               }

               response.EnsureSuccessStatusCode();
           }

           public async Task<SpeakerResponse> GetSpeakerAsync(int id)
           {
               var response = await _httpClient.GetAsync($"/api/speakers/{id}");

               if (response.StatusCode == HttpStatusCode.NotFound)
               {
                   return null;
               }

               response.EnsureSuccessStatusCode();

               return await response.Content.ReadAsAsync<SpeakerResponse>();
           }

           public async Task<List<SpeakerResponse>> GetSpeakersAsync()
           {
               var response = await _httpClient.GetAsync("/api/speakers");

               response.EnsureSuccessStatusCode();

               return await response.Content.ReadAsAsync<List<SpeakerResponse>>();
           }

           public async Task PutSessionAsync(Session session)
           {
               var response = await _httpClient.PutAsJsonAsync($"/api/sessions/{session.Id}", session);

               response.EnsureSuccessStatusCode();
           }
       }
   }
   ```

### Configure the API client

1. Open the *Startup.cs* file
1. Locate the `ConfigureServices` method and add the following code to the bottom of it:

   

``` csharp
   services.AddHttpClient<IApiClient, ApiClient>(client =>
   {
       client.BaseAddress = new Uri(Configuration["serviceUrl"]);
   });
   ```

![](/img/aspdotnetcore/confplanner/4/49.png)

1. This adds an instance of `HttpClientFactory` with its base URL pulled from the application configuration, which will point to our BackEnd API application
1. Open the *appsettings.json* file and add the configuration key for `serviceUrl` pointing to the URL your specific BackEnd API application is configured to run in (check your *launchSettings.json* file for the specific port your BackEnd API application uses):

   

``` json
   "ServiceUrl": "https://localhost:44386/"
   ```

![](/img/aspdotnetcore/confplanner/4/50.png)

## List the sessions on the home page

> Now that we have an API client we can use to talk to our BackEnd API application, we'll update the home page to show a basic list of all sessions for the conference to ensure the FrontEnd can talk to the BackEnd correctly.

### Load the data into the PageModel

1. Open the */Pages/Index.cshtml.cs* file
1. Edit the constructor to accept the `IApiClient` interface and store it in a local field:

   

``` csharp
   protected readonly IApiClient _apiClient;

   public IndexModel(IApiClient apiClient)
   {
       _apiClient = apiClient;
   }
   ```

1. Add some properties to store sessions and other data we'll need when rendering the page:

   

``` csharp
   public IEnumerable<IGrouping<DateTimeOffset?, SessionResponse>> Sessions { get; set; }

   public IEnumerable<(int Offset, DayOfWeek? DayofWeek)> DayOffsets { get; set; }

   public int CurrentDayOffset { get; set; }
   ```

1. Add a page handler method to handle GET requests to the page, that loads the session data and calculates the data required to build the day navigation UI:

   

``` csharp
   public async Task OnGet(int day = 0)
   {
       CurrentDayOffset = day;

       var sessions = await _apiClient.GetSessionsAsync();

       var startDate = sessions.Min(s => s.StartTime?.Date);

       var offset = 0;
       DayOffsets = sessions.Select(s => s.StartTime?.Date)
                            .Distinct()
                            .OrderBy(d => d)
                            .Select(day => (offset++, day?.DayOfWeek));

       var filterDate = startDate?.AddDays(day);

       Sessions = sessions.Where(s => s.StartTime?.Date == filterDate)
                          .OrderBy(s => s.TrackId)
                          .GroupBy(s => s.StartTime)
                          .OrderBy(g => g.Key);
   }
   ```

### Render the sessions list on the home page

1. Open the */Pages/Index.cshtml* Razor Page file
1. Add some Razor markup to show the sessions as a simple list, grouped by time-slot:

   

``` html
   <div class="agenda">
       <h1>My Conference @System.DateTime.Now.Year</h1>

       @foreach (var timeSlot in Model.Sessions)
       {
       <h4>@timeSlot.Key?.ToString("HH:mm")</h4>
       <ul>
           @foreach (var session in timeSlot)
           {
           <li>@session.Title</li>
           }
       </ul>
       }
   </div>
```

1. Right-click the solution, select Properties and set both BackEnd and FrontEnd as startup projects

![](/img/aspdotnetcore/confplanner/4/52.png)

1. Run the FrontEnd application at this stage and we should see the sessions listed on the home page

![](/img/aspdotnetcore/confplanner/4/53.png)

### Add buttons to allow showing sessions for different days

1. In */Pages/Index.cshtml*, add some markup to allow the user to show sessions for the different days of the conference, below the `<h1>` we added previously:

   

``` html
   <ul class="nav nav-pills mb-3">
       @foreach (var day in Model.DayOffsets)
       {
       <li role="presentation" class="nav-item">
           <a class="nav-link @(Model.CurrentDayOffset == day.Offset ? " active" : null)" asp-route-day="@day.Offset">@day.DayofWeek?.ToString()</a>
       </li>
       }
   </ul>
```

1. Run the application again and try clicking the buttons to show sessions for the different days

![](/img/aspdotnetcore/confplanner/4/54.png)

## Update the sessions list UI

1. Make the list of sessions better looking by updating the markup to use [Bootstrap cards](https://getbootstrap.com/docs/4.0/components/card/):

   

``` html
   <h4>@timeSlot.Key?.ToString("HH:mm")</h4>
   <div class="row">
       @foreach (var session in timeSlot)
       {
       <div class="col-md-3 mb-4">
           <div class="card shadow session h-100">
               <div class="card-header">@session.Track?.Name</div>
               <div class="card-body">
                   <h5 class="card-title"><a asp-page="Session" asp-route-id="@session.Id">@session.Title</a></h5>
               </div>
               <div class="card-footer">
                   <ul class="list-inline mb-0">
                       @foreach (var speaker in session.Speakers)
                       {
                       <li class="list-inline-item">
                           <a asp-page="Speaker" asp-route-id="@speaker.Id">@speaker.Name</a>
                       </li>
                       }
                   </ul>
               </div>
           </div>
       </div>
       }
   </div>
```

1. Run the page again and see the updated sessions list UI. Click the buttons again to show sessions for the different days.

![](/img/aspdotnetcore/confplanner/4/55.png)

## Add a session details page

> Now that we have a home page showing all the sessions, we'll create a page to show all the details of a specific session

### Add a Session Razor Page

1. Add a new Razor Page using the *Razor Page* template. Call the page *Session.cshtml* and save it in the */Pages* directory.
1. Open *Session.cshtml.cs* and change the page model class to `SessionModel` .
1. Accept the `IApiClient` in the constructor and add supporting members to the Page model `SessionModel` :

   

``` csharp
   public class SessionModel : PageModel
   {
       private readonly IApiClient _apiClient;

       public SessionModel(IApiClient apiClient)
       {
           _apiClient = apiClient;
       }

       public SessionResponse Session { get; set; }

       public int? DayOffset { get; set; }
   }
   ```

1. Add a page handler method to retrieve the Session details and set them on the model:

   

``` csharp
   public async Task<IActionResult> OnGetAsync(int id)
   {
       Session = await _apiClient.GetSessionAsync(id);

       if (Session == null)
       {
           return RedirectToPage("/Index");
       }

       var allSessions = await _apiClient.GetSessionsAsync();

       var startDate = allSessions.Min(s => s.StartTime?.Date);

       DayOffset = Session.StartTime?.Subtract(startDate ?? DateTimeOffset.MinValue).Days;

        return Page();
    }
   ```

1. Open the *Session.cshtml* file and add markup to display the details and navigation UI:

   

``` html
   @page "{id}"
   @model SessionModel

   <ol class="breadcrumb">
       <li class="breadcrumb-item"><a asp-page="/Index">Agenda</a></li>
       <li class="breadcrumb-item"><a asp-page="/Index" asp-route-day="@Model.DayOffset">Day @(Model.DayOffset + 1)</a></li>
       <li class="breadcrumb-item active">@Model.Session.Title</li>
   </ol>

   <h1>@Model.Session.Title</h1>
   <span class="label label-default">@Model.Session.Track?.Name</span>

   @foreach (var speaker in Model.Session.Speakers)
   {
   <em><a asp-page="Speaker" asp-route-id="@speaker.Id">@speaker.Name</a></em>
   }

   @foreach (var para in Model.Session.Abstract.Split("\r\n", StringSplitOptions.RemoveEmptyEntries))
   {
   <p>@para</p>
   }
```

![](/img/aspdotnetcore/confplanner/4/56.png)

## Add a page to show speaker details

> We'll add a page to show details for a given speaker

1. Add a new Razor Page using the *Razor Page* template. Call the page *Speaker.cshtml* and save it in the */Pages* directory.
1. Accept the `IApiClient` in the constructor and add supporting members to the Page model `SpeakerModel` :

   

``` csharp
   public class SpeakerModel : PageModel
   {
       private readonly IApiClient _apiClient;

       public SpeakerModel(IApiClient apiClient)
       {
           _apiClient = apiClient;
       }

       public SpeakerResponse Speaker { get; set; }
   }
   ```

1. Add a page handler method to retrieve the Speaker details and set them on the model:

   

``` csharp
   public async Task<IActionResult> OnGet(int id)
   {
       Speaker = await _apiClient.GetSpeakerAsync(id);

       if (Speaker == null)
       {
           return NotFound();
       }

       return Page();
   }
   ```

1. Open the *Speaker.cshtml* file and add markup to display the details and navigation UI:

   

``` html
   @page "{id}"
   @model SpeakerModel

   <ol class="breadcrumb">
       <li class="breadcrumb-item"><a asp-page="/Speakers">Speakers</a></li>
       <li class="breadcrumb-item active">@Model.Speaker.Name</li>
   </ol>

   <h2>@Model.Speaker.Name</h2>

   <p>@Model.Speaker.Bio</p>

   <h3>Sessions</h3>
   <div class="row">
       <div class="col-md-5">
           <ul class="list-group">
               @foreach (var session in Model.Speaker.Sessions)
               {
               <li class="list-group-item"><a asp-page="Session" asp-route-id="@session.Id">@session.Title</a></li>
               }
           </ul>
       </div>
   </div>
```

![](/img/aspdotnetcore/confplanner/4/57.png)

## Add search functionality

> We'll add a page to allow users to search the conference agenda, finding sessions and speakers that match the supplied search term.

### Add DTOs for search

1. Add a new DTO class `SearchTerm` in the DTO project:

![](/img/aspdotnetcore/confplanner/4/58.png)

    

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Text;

    namespace ConferenceDTO
    {
        public class SearchTerm
        {
            public string Query { get; set; }
        }
    }
    ```

1. Add a new DTO class `SearchResult` in the DTO project:

   

``` csharp
   using System;
   using System.Collections.Generic;
   using System.Text;

   namespace ConferenceDTO
   {
       public class SearchResult
       {
            public SearchResultType Type { get; set; }

            public SessionResponse Session { get; set; }

            public SpeakerResponse Speaker { get; set; }
        }

        public enum SearchResultType
        {
            Session,
            Speaker
        }
   }
   ```

### Add a search controller

1. Add a `SearchController` with an action method that accepts a `SearchTerm` and searchs for sessions and speakers with matching titles or names, and concatenates the results as `SearchResult` s:

   

``` csharp
   using System;
   using System.Collections.Generic;
   using System.Linq;
   using System.Threading.Tasks;
   using BackEnd.Data;
   using ConferenceDTO;
   using Microsoft.AspNetCore.Mvc;
   using Microsoft.EntityFrameworkCore;

   namespace BackEnd.Controllers
   {
       [Route("api/[controller]")]
       [ApiController]
       public class SearchController : ControllerBase
       {
           private readonly ApplicationDbContext _context;

           public SearchController(ApplicationDbContext context)
           {
               _context = context;
           }

           [HttpPost]
           public async Task<ActionResult<List<SearchResult>>> Search(SearchTerm term)
           {
               var query = term.Query;
               var sessionResults = await _context.Sessions.Include(s => s.Track)
                                                   .Include(s => s.SessionSpeakers)
                                                       .ThenInclude(ss => ss.Speaker)
                                                   .Where(s =>
                                                       s.Title.Contains(query) ||
                                                       s.Track.Name.Contains(query)
                                                   )
                                                   .ToListAsync();

               var speakerResults = await _context.Speakers.Include(s => s.SessionSpeakers)
                                                       .ThenInclude(ss => ss.Session)
                                                   .Where(s =>
                                                       s.Name.Contains(query) ||
                                                       s.Bio.Contains(query) ||
                                                       s.WebSite.Contains(query)
                                                   )
                                                   .ToListAsync();

               var results = sessionResults.Select(s => new SearchResult
               {
                   Type = SearchResultType.Session,
                   Session = s.MapSessionResponse()
               })
               .Concat(speakerResults.Select(s => new SearchResult
               {
                   Type = SearchResultType.Speaker,
                   Speaker = s.MapSpeakerResponse()
               }));

               return results.ToList();
           }
       }
   }
   ```

### Add search methods to the IApiClient

1. Add the `SearchAsync` method to IApiClient:

   

``` csharp
   Task<List<SearchResult>> SearchAsync(string query);
   ```

1. Add the implementation to ApiClient:

   

``` csharp
   public async Task<List<SearchResult>> SearchAsync(string query)
   {
       var term = new SearchTerm
       {
           Query = query
       };

       var response = await _httpClient.PostAsJsonAsync($"/api/search", term);

       response.EnsureSuccessStatusCode();

       return await response.Content.ReadAsAsync<List<SearchResult>>();
   }
   ```

### Add a search page to the Front End

1. Add a new Razor Page using the *Razor Page* template. Call the page *Search.cshtml* and save it in the */Pages* directory.
1. Accept the `IApiClient` in the constructor and add supporting members to the Page model `SearchModel` :

   

``` csharp
   public class SearchModel : PageModel
   {
       private readonly IApiClient _apiClient;

       public SearchModel(IApiClient apiClient)
       {
           _apiClient = apiClient;
       }

       public string Term { get; set; }

       public List<SearchResult> SearchResults { get; set; }
   }
   ```

1. Add a page handler method to retrieve the search results and set them on the model, deserializing the individual search items to the relevant model type:

   

``` csharp
   public async Task OnGetAsync(string term)
   {
       Term = term;
       SearchResults = await _apiClient.SearchAsync(term);
   }
   ```

1. Open the *Search.cshtml* file and add markup to allow users to enter a search term and display the results, casting each result to the relevant display model type:

   

``` html
   @page
   @using ConferenceDTO
   @model SearchModel

   <div class="search">
       <h1>Search</h1>
       <form method="get">
           <div class="input-group mb-3">
               <input asp-for="Term" placeholder="Search for sessions or speakers..." class="form-control" />
               <div class="input-group-append">
                   <button class="btn btn-outline-secondary" type="submit">Go!</button>
               </div>
           </div>
           @if (Model.SearchResults?.Count > 0)
           {
           <p>
               @Model.SearchResults.Count result(s)
           </p>
           }
       </form>
   </div>

   <div class="row">
       @foreach (var result in Model.SearchResults)
       {
       <div class="col-md-12">
           @switch (result.Type)
           {
           case SearchResultType.Speaker:
           <div class="card shadow mb-3">
               <div class="card-header">
                   <h3 class="card-title">Speaker: <a asp-page="Speaker" asp-route-id="@result.Speaker.Id">@result.Speaker.Name</a></h3>
               </div>
               <div class="card-body">
                   <p>
                       @foreach (var session in result.Speaker.Sessions)
                       {
                       <a asp-page="/Session" asp-route-id="@session.Id"><em>@session.Title</em></a>
                       }
                   </p>
                   <p>
                       @result.Speaker.Bio
                   </p>
               </div>
           </div>
           break;

           case SearchResultType.Session:
           <div class="card shadow mb-3">
               <div class="card-header">
                   <h3 class="card-title">Session: <a asp-page="Session" asp-route-id="@result.Session.Id">@result.Session.Title</a></h3>
                   @foreach (var speaker in result.Session.Speakers)
                   {
                   <a asp-page="/Speaker" asp-route-id="@speaker.Id"><em>@speaker.Name</em></a>
                   }
               </div>
               <div class="card-body">
                   <p>
                       @result.Session.Abstract
                   </p>
               </div>
           </div>
           break;
           }
       </div>
       }
   </div>
```

1. Add the search link to the navigation pane in `_Layout.cshtml` :

    

``` html
    <li class="nav-item">
        <a class="nav-link text-dark" asp-page="/Search">Search</a>
    </li>
```

1. Click on the `Search` link to test the new search feature.

![](/img/aspdotnetcore/confplanner/4/59.png)

---
{% include conf_sessions.md %}

{% include blogslide.html %}

