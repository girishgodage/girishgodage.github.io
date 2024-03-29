---
title: Building a SPA Front End
date: 2019-10-24 05:54:00 Z
permalink: "/blog/SPAFrontEnd"
categories:
- AspDotnetCore
tags:
- learning
summary: In this session, we'll add the code for the client application. Create a
  view for the conference sessions, details and speaker information.
image: "/img/aspnetcore.png"
author: Girish Godage
layout: posts
prevurl: "/blog/Challenges"
nexturl: "/blog/SPAFrontEnd"
discussion_id: 2019-10-24-confplannertut8
---

# Building a SPA Front End

In this session, we'll add the code for the client application. Create a view for the conference sessions, details and speaker information.

>This example uses the SpaServices templates which combines the FrontEnd and Web API by default. In this code sample/lab, the API (BackEnd) is a separate project therefore unneeded portions of the generated code can be removed and setting the url for the API will be handled in a way more apt to a production setup.

> *Note:* This uses the 2.0 templates for SPA which are not included in .NET Core 2.0.x SDK. If you are on one of those SDKs you will need to follow the instructions [here](https://docs.microsoft.com/en-us/aspnet/core/spa/index?view=aspnetcore-2.1#installation) to get the newer templates

## Creating the project

Create the new SPA application using the `dotnet` CLI in the **ConferencePlanner** folder.

```bash
dotnet new angular -n FrontEndSpa -o src/FrontEndSpa
```

Add the new project to the solution

```bash
dotnet sln add src/FrontEnd/FrontEnd.csproj
```

### Clean up

Since the API is a separate project, some initial setup and cleaning out the unnecessary files can be done.

#### Remove the Weather components

- delete the **Controllers** folder
- delete **ClientApp/src/app/fetch-data** folder
- remove all references (entire line) to **FetchDataComponent** in **ClientApp/src/app/app.module.ts**
- remove the link from the **nav-menu.component.html**

```html
<li [routerLinkActive]='["link-active"]' [routerLinkActiveOptions]='{ exact: true }'>
    <a [routerLink]='["/"]' (click)='collapse()'>
    <span class='glyphicon glyphicon-home'></span> Counter
  </a>
</li>
```

### Remove the Counter components

- delete **ClientApp/src/app/counter** folder
- remove all references (entire line) to **CounterComponent** in **ClientApp/src/app/app.module.ts**
- remove the link from the **nav-menu.component.html**

```html
<li [routerLinkActive]='["link-active"]'>
  <a [routerLink]='["/counter"]' (click)='collapse()'>
    <span class='glyphicon glyphicon-education'></span> Counter
  </a>
</li>
```

## Setup the URL for the Web API Service

The URL for the ASP.NET Core Web API service needs to be configured as a setting. Futhermore, makes sense to have the ability to use a different setting per environment. Angluar uses the **environments* files to do so.

Open **ClientApp/src/environments/environments.ts** and change it to the following.

```javascript
export const environment = {
  production: false,
  API_URL: 'http://localhost:50069'
};
```

For a production setting, add the `API_URL` value to the **environment.prod.ts** file as well.

Open **ClientApp/src/main.ts** , add the `getApiUrl()` function and add the new `API_URL` to the providers

```javascript
export function getApiUrl() {
  return environment.API_URL;
}

const providers = [
  { provide: 'BASE_URL', useFactory: getBaseUrl, deps: [] },
  { provide: 'API_URL', useFactory: getApiUrl, deps: [] }
];
```

## Create and wire-up the API using an Angular Service

> We'll create the models to map to the ConferenceDTO classes and a class to talk to the ASP.NET Core Web API service

### Create the model

Using the Angular CLI, create the class for the model to map the ConferenceDTO classes to TypeScript classes. Change directory to ClientApp, and create a **model.ts** using the following command.

```bash
ng g class shared/model
```

Add the following code to the file.

```javascript
  export class Track {
    trackID: number;
    conferenceID: number;
    name: string;
  }

  export class Speaker {
    id: number;
    name: string;
    bio?: any;
    webSite?: any;
    sessions?: Session[];
  }

  export class Session {
    track: Track;
    speakers: Speaker[];
    tags: any[];
    id: number;
    conferenceID: number;
    title: string;
    abstract: string;
    startTime: Date;
    endTime: Date;
    duration: string;
    trackId: number;
  }
```

### Create the data service

Create a data service to call the ASP.NET Core Web API.

```bash
ng g service shared/data
```

Update the code to the following.

```javascript
import { Injectable, Inject } from '@angular/core';
import { Headers, Http } from '@angular/http';

import 'rxjs/add/operator/toPromise';

import { Session, Speaker } from './model';

@Injectable()
export class DataService {

  private headers = new Headers({ 'Content-Type': 'application/json' });
  private sessionUrl = 'api/sessions';
  private speakerUrl = 'api/speakers';
  /**
   * init with Http
   */
  constructor(private http: Http, @Inject('API_URL') private baseUrl: string) { }

  getSessions(): Promise<Session[]> {
    return this.http.get(this.baseUrl + this.sessionUrl)
      .toPromise()
      .then(response => <Session[]>response.json())
      .catch(this.handleError);
  }

  getSession(id: number): Promise<Session> {
    const url = `${this.baseUrl + this.sessionUrl}/${id}`;
    return this.http.get(url)
      .toPromise()
      .then(response => <Session>response.json())
      .catch(this.handleError);
  }

  getSpeaker(id: number): Promise<Speaker> {
    const url = `${this.baseUrl + this.speakerUrl}/${id}`;
    return this.http.get(url)
      .toPromise()
      .then(response => <Speaker>response.json())
      .catch(this.handleError);
  }

  getSpeakers(): Promise<Speaker[]> {
    return this.http.get(this.baseUrl + this.speakerUrl)
      .toPromise()
      .then(response => <Speaker[]>response.json())
      .catch(this.handleError);
  }

  private getData(response: Response) { }
  private handleError(error: any): Promise<any> {
    console.error('An error occurred', error); // for demo purposes only
    return Promise.reject(error.message || error);
  }
}
```

## List the sessions

> Now that we have a service to talk to the API, we'll add the views to display a basic list of all sessions for the conference and validate the FrontEnd / API communication.

### Create and add the Sessions component

1. Create the sessions component

    ```bash
    ng g component sessions
    ```

1. Update the **sessions.component.ts** file to the following

    ```typescript
    import { Component, OnInit } from '@angular/core';
    import { Router } from '@angular/router';

    import { DataService } from '../shared/data.service';
    import { Session } from '../shared/model';

    @Component({
        selector: 'conf-sessions',
        templateUrl: './sessions.component.html'
    })
    export class SessionsComponent implements OnInit {
        sessions: Session[];

        constructor(private dataService: DataService) { }

        getSessions(): void {
            this.dataService
                .getSessions()
                .then(sessions => this.sessions = sessions);
        }

        ngOnInit() {
            this.getSessions();
        }
    }
    ```

1. Update the template **sessions.component.html**

    ```html
    <div class="agenda">
      <h1>My Conference 2017</h1>

      <p *ngIf="!sessions">
        <em>Loading...</em>
      </p>

      <div class="row">
        <div *ngFor="let session of sessions" class="col-md-3">
          <div class="panel panel-default session">
            <div class="panel-body">
              <p>{{session.track.name}}</p>
              <h3 class="panel-title">
                <a [routerLink]="['/sessiondetail', session.id]">{{session.title}}</a>
              </h3>
              <p *ngFor="let speaker of session.speakers">
                <em>
                  <a [routerLink]="['/speaker', speaker.id]">{{speaker.name}}</a>
                </em>
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>
    ```

1. Now that the components and data service is created, open **app.module.ts** to import the DataService component. Add the following to the top to import the module.

    ```typescript
    import { DataService } from './components/shared/data.service';
    ```

1. In the same file add the **SessionsComponent** in the `declarations` part of the **@NgModule**

1. Add the route for the sessions page to the **RouterModule**

    ```typescript
    { path: 'sessions', component: SessionsComponent },
    ```

1. Add the **DataService** as a provider after the imports.

    ```typescript
    providers: [DataService]
    ```

1. Finally, add the following in **nav-menu.component.html** to add a link to the sessions list from the left nav:

    ```html
    <li [routerLinkActive]="['link-active']">
        <a [routerLink]="['/sessions']">
            <span class='glyphicon glyphicon-th-list'></span> Sessions
        </a>
    </li>
    ```


#### Running the application

Start the Web API Application using Visual Studio or VS Code. It should start on http://localhost:50069

In another editor or process, start the FrontEndSpa application pressing F5 or running `dotnet run` and your SPA application will be available on http://localhost:5000. Since we are using Webpack with the hot module reload, you can continue to make changes to the Angular application and see the changes as you work through the rest of the code.

### Creating the Session Detail Page

> Now that we have a home page showing all the sessions, we'll create a page to show all the details of a specific session

1. Create the Session Detail component

    ```bash
    ng g component sessionDetail
    ```
1. Update the component code to the following

    ```typescript
    import 'rxjs/add/operator/switchMap';
    import { Component, OnInit } from '@angular/core';
    import { ActivatedRoute, ParamMap } from '@angular/router';
    import { Location }                 from '@angular/common';

    import { Session } from '../shared/model';
    import { DataService } from '../shared/data.service';

    @Component({
      selector: 'session-detail',
      templateUrl: './sessiondetail.component.html'
    })
    export class SessionDetailComponent implements OnInit {
      session: Session;

      constructor(
        private sessionService: DataService,
        private route: ActivatedRoute,
        private location: Location
      ) { }

      ngOnInit(): void {

        this.route.paramMap
          .switchMap((params: ParamMap) => this.sessionService.getSession(+params.get('id')!))
          .subscribe(session => this.session = session);
      }

      goBack() {
        this.location.back();
      }

    }
    ```

1. Update the template **session-detail.component.html**

    ```html
    <ol class="breadcrumb">
        <li><a (click)="goBack()" style="cursor: pointer">Back</a></li>
        <li><a [routerLink]="['/sessions']">Agenda</a></li>
        <li class="active">{{session.title}}</li>
    </ol>

    <h1>{{session.title}}</h1>
    <span class="label label-default">{{session.track.name}}</span>

    <p *ngFor="let speaker of session.speakers">
        <em>
          <a [routerLink]="['/speaker', speaker.id]">{{speaker.name}}</a>
        </em>
      </p>

    <p>{{session.abstract}}</p>
    ```

1. Add the route for the session detail page to the **RouterModule** also adding the `/:id` parameter to be passed along for the specific session.

    ```typescript
    { path: 'sessiondetail/:id', component: SessionDetailComponent }
    ```

### Create the Speaker Detail Page

> We'll add a page to show details for a given speaker

1. Create the Speaker Detail component

    ```typescript
    ng g component speakerDetail
    ```

1. Update the code for the component to the following

    ```typescript
    import 'rxjs/add/operator/switchMap';
    import { Component, OnInit } from '@angular/core';
    import { ActivatedRoute, ParamMap } from '@angular/router';
    import { Location }                 from '@angular/common';

    import { DataService } from '../shared/data.service';
    import { Speaker } from '../shared/model';

    @Component({
      selector: 'conf-speakerdetail',
      templateUrl: './speakerdetail.component.html'
    })
    export class SpeakerDetailComponent implements OnInit {
      speaker: Speaker;

      constructor(
        private dataService: DataService,
        private route: ActivatedRoute,
        private location: Location
      ) { }

      ngOnInit() {
        this.route.paramMap
        .switchMap((params: ParamMap) => this.dataService.getSpeaker(+params.get('id')!))
        .subscribe(speaker => this.speaker= speaker);
      }

      goBack() {
        this.location.back();
      }

    }
    ```

1. Update the template **speaker-detail.component.html** to the following

    ```html
    <ol class="breadcrumb">
      <li><a (click)="goBack()" style="cursor: pointer">Back</a></li>
      <li><a [routerLink]="['/speakers']">Speakers</a></li>
      <li class="active">{{speaker.name}}</li>
    </ol>

    <h2>{{speaker.name}}</h2>

    <p>{{speaker.bio}}</p>

    <h3>Sessions</h3>
    <div class="row">
      <div class="col-md-5">
          <ul class="list-group">
            <li *ngFor="let session of speaker.sessions" class="list-group-item">
              <a [routerLink]="['/sessiondetail', session.id]">{{session.title}}</a>
            </li>
        </ul>
      </div>
    </div>
    ```
1. Add the route for the speaker detail page to the **RouterModule** also adding the `/:id` parameter to be passed along for the specific session.

    ```typescript
    { path: 'speaker/:id', component: SpeakerDetailComponent }
    ```

### Challenge / Bonus

Add a Speakers Listing Page and link from it to the `/speakerdetail` route


---
{% include conf_sessions.md %}

{% include blogslide.html %}

