---
title: Build out BackEnd and Refactor
date: 2019-10-24 05:54:00 Z
permalink: "/blog/BuildoutBackEndandRefactor"
categories:
- AspDotnetCore
tags:
- learning
summary: In this tutorial, we'll add the rest of our models and controllers that expose
  them. We'll also refactor our application, moving our DTOs to a shared project so
  they can be used by our front-end application later..
image: "/img/aspnetcore.png"
author: Girish Godage
layout: posts
prevurl: "/blog/CreateBackEndAPIproject"
nexturl: "/blog/Addfront-endrenderagendasetupfront-endmodels"
discussion_id: 2019-10-24-confplannertut3
---

## Building out the Back End
In this session, we'll add the rest of our models and controllers that expose them. We'll also refactor our application, moving our DTOs to a shared project so they can be used by our front-end application later.

## Add a ConferenceDTO project
> We'll start by creating the new shared project to hold our data transfer objects.
### Adding the ConferenceDTO Project using Visual Studio
1. If using Visual Studio, right-click on the Solution and select *Add* / *New Project...*.
1. Select *.NET Standard* from the project types on the left and select the *Class Library (.NET Standard)* template. Name the project ConferenceDTO and press OK.
![](/img/aspdotnetcore/confplanner/3/vs2019-create-dto-project.png)
1. Delete the generated `Class1.cs` file from this new project.
![](/img/aspdotnetcore/confplanner/3/31.png)
1. Right-click the 'Dependencies' node under the BackENd project, select "Add Reference..." and put a checkmark near ConferenceDTO.
![](/img/aspdotnetcore/confplanner/3/32.png)
### Adding the ConferenceDTO project via the Command Line
1. Open a command prompt and navigate to the root `ConferencePlanner` directory.
1. Run the following command: 
   ```console
   dotnet new classlib -o ConferenceDTO -f netstandard2.0
   ```
1. Next we'll need to add a reference to the ConferenceDTO project from the BackEnd project. From the command line, navigate to the BackEnd project directory and execute the following command:
   ```console
   dotnet add reference ../ConferenceDTO
   ```
1. Add the ConferenceDTO project to the solution:
   ```
   dotnet sln add ConferenceDTO/ConferenceDTO.csproj
   ```

## Refactoring the Speaker model into the ConferenceDTO project
1. Copy the `Speaker.cs` class from the *BackEnd* application into the root of the new ConferenceDTO project, and change the namespace to `ConferenceDTO`.
1. The data annotations references should be broken at this point, to resovle it, we need to add a nuget the missing NuGet package into the `ConferenceDTO` project.
1. Add a reference to the NuGet package `System.ComponentModel.Annotations` version `4.6.0`.
    ![](/img/aspdotnetcore/confplanner/3/33.png)
    > This can be done from the command line using `dotnet add package System.ComponentModel.Annotations --version 4.6.0`

1. When the package restore completes, you should see that your data annotations are now resolved.
1. Go back to the *BackEnd* application and modify the code in `Speaker.cs` as shown:
   ```csharp
   public class Speaker : ConferenceDTO.Speaker
   {
   }
   ```
   ![](/img/aspdotnetcore/confplanner/3/34.png)
1. Run the application and view the Speakers data using the Swagger UI to verify everything still works.

## Adding the remaining models to ConferenceDTO

We've got several more models to add, and unfortunately it's a little mechanical. You can copy the following classes manually, or open the completed solution which is shown at the end.

1. Create an `Attendee.cs` class in the *ConferenceDTO* project with the following code:
   ```csharp
   using System;
   using System.Collections.Generic;
   using System.ComponentModel.DataAnnotations;
   
   namespace ConferenceDTO
   {
       public class Attendee
       {
           public int Id { get; set; }
   
           [Required]
           [StringLength(200)]
           public virtual string FirstName { get; set; }
   
           [Required]
           [StringLength(200)]
           public virtual string LastName { get; set; }
   
           [Required]
           [StringLength(200)]
           public string UserName { get; set; }
           
           [StringLength(256)]
           public virtual string EmailAddress { get; set; }
       }
   }
   ```
1. Create a `Session.cs` class with the following code:
   ```csharp
   using System;
   using System.Collections;
   using System.Collections.Generic;
   using System.ComponentModel.DataAnnotations;
   
   namespace ConferenceDTO
   {
       public class Session
       {
           public int Id { get; set; }
   
           [Required]
           [StringLength(200)]
           public string Title { get; set; }
   
           [StringLength(4000)]
           public virtual string Abstract { get; set; }
   
           public virtual DateTimeOffset? StartTime { get; set; }
   
           public virtual DateTimeOffset? EndTime { get; set; }
   
           // Bonus points to those who can figure out why this is written this way
           public TimeSpan Duration => EndTime?.Subtract(StartTime ?? EndTime ?? DateTimeOffset.MinValue) ?? TimeSpan.Zero;
   
           public int? TrackId { get; set; }
       }
   }
   ```
1. Create a new `Track.cs` class with the following code:
   ```csharp
   using System;
   using System.Collections.Generic;
   using System.ComponentModel.DataAnnotations;
   
   namespace ConferenceDTO
   {
       public class Track
       {
           public int Id { get; set; }
   
           [Required]
           [StringLength(200)]
           public string Name { get; set; }
       }
   }
   ```

## Creating Derived Models in the BackEnd project
We're not going to create our EF models directly from the `ConferenceDTO` classes. Instead, we'll create some composite classes such as `SessionSpeaker`, since these will map more closely to what our application will be working with.

We're also going to take this opportunity to rename the `Models` directory in the *BackEnd* project to `Data` since it no longer just contains models.
1. Right-click the `Models` directory and select `Rename`, changing the name to `Data`.
    > Note: If you are using Visual Studio, you can use refactoring to rename the namespace.
1. Add a `SessionSpeaker.cs` class to the `Data` directory with the following code:
   ```csharp
   using System;
   using System.Collections.Generic;
   using System.Linq;
   using System.Threading.Tasks;
   
   namespace BackEnd.Data
   {
       public class SessionSpeaker
       {
           public int SessionId { get; set; }
   
           public Session Session { get; set; }
   
           public int SpeakerId { get; set; }
   
           public Speaker Speaker { get; set; }
       }
   }
   ```
1. Add an `SessionAttendee.cs` class with the following code:
 
   ```csharp
   using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Threading.Tasks;

    namespace BackEnd.Data
    {
        public class SessionAttendee
        {
            public int SessionId { get; set; }

            public Session Session { get; set; }

            public int AttendeeId { get; set; }

            public Attendee Attendee { get; set; }
       }
   }
   ```
1. Add an `Attendee.cs` class with the following code:
   ```csharp
   using System;
   using System.Collections.Generic;
   
   namespace BackEnd.Data
   {
       public class Attendee : ConferenceDTO.Attendee
       {
           public virtual ICollection<SessionAttendee> SessionsAttendees { get; set; }
       }
   }
   ```
1. Add a `Session.cs` class with the following code:
   ```csharp
   using System;
   using System.Collections;
   using System.Collections.Generic;
   
   namespace BackEnd.Data
   {
       public class Session : ConferenceDTO.Session
       {
           public virtual ICollection<SessionSpeaker> SessionSpeakers { get; set; }

           public virtual ICollection<SessionAttendee> SessionAttendees { get; set; }
   
           public Track Track { get; set; }
       }
   }
   ```
1. Add a `Track.cs` class with the following code:
   ```csharp
   using System;
   using System.Collections.Generic;
   using System.ComponentModel.DataAnnotations;

   namespace BackEnd.Data
   {
       public class Track : ConferenceDTO.Track
       {
           public virtual ICollection<Session> Sessions { get; set; }
       }
   }
   ```
1. Modify the `Speaker.cs` class we wrote previously to make the following two changes: update to the namespace to match our directory rename, and add a referece to the `SessionSpeaker` composite class:
   ```csharp
   using System;
   using System.Collections.Generic;
   
   namespace BackEnd.Data
   {
       public class Speaker : ConferenceDTO.Speaker
       {
           public virtual ICollection<SessionSpeaker> SessionSpeakers { get; set; } = new List<SessionSpeaker>();
       }
   }
   ```

## Update the ApplicationDbContext
Okay, now we need to update our `ApplicationDbContext` so Entity Framework knows about our new models.

1. Update `ApplicationDbContext.cs` to use the following code:
   ```csharp
   using Microsoft.EntityFrameworkCore;

   namespace BackEnd.Data
   {
       public class ApplicationDbContext : DbContext
       {
           public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
               : base(options)
           {

           }

           protected override void OnModelCreating(ModelBuilder modelBuilder)
           {
               modelBuilder.Entity<Attendee>()
               .HasIndex(a => a.UserName)
               .IsUnique();

                // Many-to-many: Session <-> Attendee
                modelBuilder.Entity<SessionAttendee>()
                    .HasKey(ca => new { ca.SessionId, ca.AttendeeId });

                // Many-to-many: Speaker <-> Session
                modelBuilder.Entity<SessionSpeaker>()
                    .HasKey(ss => new { ss.SessionId, ss.SpeakerId });
           }

           public DbSet<Session> Sessions { get; set; }

           public DbSet<Track> Tracks { get; set; }

           public DbSet<Speaker> Speakers { get; set; }

           public DbSet<Attendee> Attendees { get; set; }
       }
   }
   ```
1. Fix errors due to the rename from `BackEnd.Models` to `BackEnd.Data`. You can either do this using a find / replace (replacing "BackEnd.Models" with "BackEnd.Data") or you can do a build and fix errors.
1. Ensure that the application builds now.

## Add a new database migration

### Visual Studio: Package Manager Console 
1. Run the following commands in the Package Manager Console (specify the `BackEnd` project)
   ```console
   Add-Migration Refactor
   Update-Database
   ```
![](/img/aspdotnetcore/confplanner/3/37.png)
### Command line
1. Run the following commands in the command prompt in the `BackEnd` project directory:
   ```console
   dotnet ef migrations add Refactor
   dotnet ef database update
   ```
1. Now take a deep breath and run the application and navigate to `/swagger`. You should see the Swagger UI.

## Updating the Speakers API controller

1. Modify the query for the `GetSpeakers()` method as shown below:
   ```csharp
   var speakers = await _context.Speakers.AsNoTracking()
                           .Include(s => s.SessionSpeakers)
                               .ThenInclude(ss => ss.Session)
                           .ToListAsync();
   return speakers;
   ```
   ![](/img/aspdotnetcore/confplanner/3/39.png)
1. While the above will work, this is directly returning our model class. A better practice is to return an output model class. Create a `SpeakerResponse.cs` class in the `ConferenceDTO` project with the following code:
   ```csharp
   using System;
   using System.Collections.Generic;
   using System.Text;

   namespace ConferenceDTO
   {
       public class SpeakerResponse : Speaker
       {
           public ICollection<Session> Sessions { get; set; } = new List<Session>();
       }
   }
   ```
1. Now we'll add a utility method to map between these classes. In the *BackEnd* project, create an `Infrastructure` directory. Add a class named `EntityExtensions.cs` with the following mapping code:
   ```csharp
   using BackEnd.Data;
   using System;
   using System.Collections.Generic;
   using System.Linq;
   using System.Threading.Tasks;

   namespace BackEnd.Data
   {
       public static class EntityExtensions
       {
           public static ConferenceDTO.SpeakerResponse MapSpeakerResponse(this Speaker speaker) =>
               new ConferenceDTO.SpeakerResponse
               {
                   Id = speaker.Id,
                   Name = speaker.Name,
                   Bio = speaker.Bio,
                   WebSite = speaker.WebSite,
                   Sessions = speaker.SessionSpeakers?
                       .Select(ss =>
                           new ConferenceDTO.Session
                           {
                               Id = ss.SessionId,
                               Title = ss.Session.Title
                           })
                       .ToList()
               };
       }
   }
   ```
1. Now we can update the `GetSpeakers()` method of the *SpeakersController* so that it returns our response model. Update the last few lines so that the method reads as follows:
   ```csharp
   [HttpGet]
   public async Task<ActionResult<List<ConferenceDTO.SpeakerResponse>>> GetSpeakers()
   {
       var speakers = await _context.Speakers.AsNoTracking()
                                       .Include(s => s.SessionSpeakers)
                                           .ThenInclude(ss => ss.Session)
                                       .Select(s => s.MapSpeakerResponse())
                                       .ToListAsync();
       return speakers;
   }
   ```
1. Update the `GetSpeaker()` method to use our mapped response models as follows:
   ```csharp
   [HttpGet("{id}")]
   public async Task<ActionResult<ConferenceDTO.SpeakerResponse>> GetSpeaker(int id)
   {
       var speaker = await _context.Speakers.AsNoTracking()
                                       .Include(s => s.SessionSpeakers)
                                           .ThenInclude(ss => ss.Session)
                                       .SingleOrDefaultAsync(s => s.Id == id);
       if (speaker == null)
       {
           return NotFound();
       }
       return speaker.MapSpeakerResponse();
   }
   ```
   ![](/img/aspdotnetcore/confplanner/3/40.png)
1. Remove the other actions (`PutSpeaker`, `PostSpeaker`, `DeleteSpeaker`), on the `SpeakersController`.

## Adding the remaining API Controllers and DTOs

1. Add the following response DTO classes from [the save point folder](https://github.com/girishgodage/confplanerapp/tree/master/save-points/2-BackEnd-completed/ConferencePlanner/ConferenceDTO)
   - `AttendeeResponse`
   - `SessionResponse`
   - `ConferenceResponse`
   - `TrackResponse`
   - `TagResponse`
1. Update the `EntityExtensions` class with the `MapSessionResponse` and `MapAttendeeResponse` methods from [the save point folder](https://github.com/girishgodage/confplanerapp/tree/master/save-points/2b-BackEnd-completed/ConferencePlanner/BackEnd/Infrastructure)
1. Copy the following controllers from [the save point folder](https://github.com/girishgodage/confplanerapp/tree/master/save-points/2b-BackEnd-completed/ConferencePlanner/BackEnd/Controllers) into the current project's `BackEnd/Controllers` directory:
   - `SessionsController`
   - `AttendeesController`

## Adding Conference Upload support
1. Copy the `DataLoader.cs` class from [here](https://github.com/girishgodage/confplanerapp/tree/master/ConferencePlanner/BackEnd/Data/DataLoader.cs) into the `Data` directory of the `BackEnd` project.
1. Copy the `SessionizeLoader.cs` and `DevIntersectionLoader.cs` classes from [here](https://github.com/girishgodage/confplanerapp/tree/master/ConferencePlanner/BackEnd/Data/) into the current project's `/src/BackEnd/Data/` directory. 
    > Note: We have data loaders from the two conference series where this workshop has been presented most; you can update this to plug in your own conference file format.
1. To improve the UI for upload, turn on the option to display enums as strings by changing `AddSwaggerGen` in `Startup.cs` to the following:
    ```c#
    services.AddSwaggerGen(options =>
    {
        options.SwaggerDoc("v1", new OpenApiInfo { Title = "Conference Planner API", Version = "v1" });
        options.DescribeAllEnumsAsStrings();
    });
    ```
1. Run the application to see the updated data via Swagger UI.
![](/img/aspdotnetcore/confplanner/3/41.png)
1. Use the Swagger UI to upload [NDC_Sydney_2019.json](https://github.com/girishgodage/confplanerapp/tree/master/ConferencePlanner/BackEnd/Data/Import/NDC_Sydney_2019.json) to the `/api/Sessions/upload` API.

![](/img/aspdotnetcore/confplanner/3/42.png)
![](/img/aspdotnetcore/confplanner/3/43.png)
---
{% include conf_sessions.md %}

{% include blogslide.html %}

