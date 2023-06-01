+++
title = "Day3"
weight = 3
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## Creating a Project Detail Component

```
ng generate component project-detail
```

In the newly created component we need to:

- Import the Project model
- Add Input to the import from @angular/core
- Add a project property decorated with @Input

```js
import { Component, Input } from '@angular/core';
import { Project } from '../model/project';

@Component({
  selector: 'app-project-detail',
  templateUrl: './project-detail.component.html',
  styleUrls: ['./project-detail.component.scss'],
})
export class ProjectDetailComponent {
  @Input() project?: Project;
}
```

#### Show the Project Detail Component, Conditionally

For now we are going to set things up such that display of the project detail is contingent on a project having been passed a project from the projects component.

**project-detail.component.html**

```
<article *ngIf="project">
  <section>
    <h2>{{ project.title | uppercase }}</h2>
    <div>{{ project.body }}</div>
  </section>
  <footer>
    <div *ngIf="project.category">
      Category: <span>{{ project.category.name }}</span>
    </div>
    <div *ngIf="project.tags?.length">
      Tags:
      <span *ngFor="let tag of project.tags">
        {{ tag.name }}
      </span>
    </div>
  </footer>
</article>
```

**projects.component.ts**

```
  selectedProject?: Project;
  onSelect(project: Project): void {
    this.selectedProject = project;
  }

  clearSelectedProject(): void {
    this.selectedProject = undefined;
  }
```

**projects.component.html**

- Wrap the element that outputs the list of projects in a container that only shows if there is no selected project.
- Add a section that outputs <app-project-detail> and a Back button if it is set.

```
<ng-container *ngIf="!selectedProject">
  <article
    *ngFor="let project of projects | projectFilter : tagFilter"
    [class.hidden]="categoryFilter && project.category?.id != categoryFilter.id"
  >

  ...

  </article>
</ng-container>
```

```
<section *ngIf="selectedProject">
  <div (click)="clearSelectedProject()">Back</div>
  <app-project-detail [project]="selectedProject"></app-project-detail>
</section>
```

## Angular Routing

To set up routing in our Angular app we're going to generate a new top-level module and then import it into our AppModule.

Both can be accomplished in one step with the CLI by running:

```
ng generate module app-routing --flat --module=app
```

**src/app/app-routing.module.ts**

```js
import { NgModule } from "@angular/core";
import { CommonModule } from "@angular/common";

@NgModule({
  declarations: [],
  imports: [CommonModule],
})
export class AppRoutingModule {}
```

#### Boilerplate Cleanup

We don't need the CommonModule import, so you can remove it both from the top and from the imports array in the @NgModule decorator.

What we DO need to import are

- RouterModule and Routes from the @angular/routing
- Our ProjectsComponent and ProjectDetailComponent.

```js
import { NgModule } from "@angular/core";
import { RouterModule, Routes } from "@angular/router";
import { ProjectsComponent } from "./projects/projects.component";
import { ProjectDetailComponent } from "./project-detail/project-detail.component";
```

#### Setting up Routes

Next, we'll start setting up our custom routes as an array of Routes.

Let's route requests to 'projects' to our ProjectsComponent.

```js
const routes: Routes = [{ path: "projects", component: ProjectsComponent }];
```

Then, within the @NgModule decorator, we'll pass our routes array to the forRoot() method of the RouterModule in the imports array and also export the now configured RouterModule so we can use it throughout our app.

```js
@NgModule({
  declarations: [],
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule],
})
```

#### RouterOutlet

At a high level, the Router module provides us with a RouterOutlet component and when a route is matched it will display whichever component we have specified for that route.

To see this in action, replace the <app-projects></app-projects> component in app.component.html with <router-outlet></router-outlet>

**app.component.html**

```
<header>
  <h1>{{ title }}</h1>
</header>
<main>
  <router-outlet></router-outlet>
</main>
<footer>&copy; {{ date.getFullYear() }} {{ author }}</footer>
```

#### Set up Navigation with routerLinks

Router also provides us with a routerLink directive that we can use to turn any clickable element into an internal (client side) link by assigning it a path.

**app.component.html**

```
<header>
  <h1>{{ title }}</h1>
  <nav>
    <span routerLink="/">Home</span> |
    <span routerLink="/projects">Projects</span>
  </nav>
</header>
```

## Route Parameters

Next, let's add a parameterized route to display our project detail component.

To make this work we will:

- Add a parameterized route in the app-routing module
- Add a method to our project service that returns a specific project by id
- Set up our project-detail component to read the id parameter from the route and call the appropriate method in the project service
- For good measure, we'll also add a back button and use the Location directive to allow the user to navigate back via their client-side navigation history.

**app-routing.module.ts**

```js
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { ProjectsComponent } from './projects/projects.component';
import { ProjectDetailComponent } from './project-detail/project-detail.component';

const routes: Routes = [
  { path: 'projects', component: ProjectsComponent },
  { path: 'projects/:id', component: ProjectDetailComponent },
];

...
```

**project.service.ts**

```js
  getProject(id: number): Observable<Project> {
    const project = PROJECTS.find((p) => p.id === id)!;
    return of(project);
  }
```

**project-detail.component.ts**

```js
import { Component, Input } from '@angular/core';
import { ActivatedRoute } from '@angular/router';
import { Location } from '@angular/common';

import { ProjectService } from '../project.service';

import { Project } from '../model/project';

@Component({
  selector: 'app-project-detail',
  templateUrl: './project-detail.component.html',
  styleUrls: ['./project-detail.component.scss'],
})
export class ProjectDetailComponent {
  constructor(
    private route: ActivatedRoute,
    private projectService: ProjectService,
    private location: Location
  ) {}
  // @Input() project?: Project;
  project?: Project;
  ngOnInit(): void {
    this.getProject();
  }

  getProject(): void {
    const id = Number(this.route.snapshot.paramMap.get('id'));
    this.projectService
      .getProject(id)
      .subscribe((project) => (this.project = project));
  }

  goBack(): void {
    this.location.back();
  }
}
```

**project-detail.component.ts**

```js
import { Component, Input } from '@angular/core';
import { ActivatedRoute } from '@angular/router';
import { Location } from '@angular/common';

import { ProjectService } from '../project.service';

import { Project } from '../model/project';

@Component({
  selector: 'app-project-detail',
  templateUrl: './project-detail.component.html',
  styleUrls: ['./project-detail.component.scss'],
})
export class ProjectDetailComponent {
  constructor(
    private route: ActivatedRoute,
    private projectService: ProjectService,
    private location: Location
  ) {}
  // @Input() project?: Project;
  project?: Project;
  ngOnInit(): void {
    this.getProject();
  }

  getProject(): void {
    const id = Number(this.route.snapshot.paramMap.get('id'));
    this.projectService
      .getProject(id)
      .subscribe((project) => (this.project = project));
  }

  goBack(): void {
    this.location.back();
  }
}
```

**project-detail.component.html**

```
  <header>
    <button type="button" (click)="goBack()">Back</button>
  </header>
```

## Outputting HTML

For our project detail page when we output the project.body we're back to seeing our `<p>` tags.

All of the same cautions and warning about script injection are worth repeating, but since we have absolute control of the content let's output our HTML as HTML again.

The issue, in this case, is that when we use interpolation in our templates Angular is protecting us by escaping the data.

`<div>{{ project.body }}</div>`

The workaround is to bind our data to the innerHTML of our div.

**project-detail.component.html**

```
<div [innerHTML]="project.body"></div>
```

## Fetching Data Asynchronously

Currently our projects service is loading and returning the data synchronously.

It's not a big deal since our data is local and lightweight, but in most situations that would not be the case. As such, let's convert our services to run asynchronously.

We can do so quite easily using the RxJS which is already available in our Angular project.

What we're going to use from the RxJS library are the Observable class which allows us to subscribe and the of() function which gathers and emits the data asynchronously until it completes (or errors out).

Without getting too far off track, this is the Observer pattern in action.

You can learn more about RxJS here: https://rxjs.dev/

#### Implementing Async Data Loading

**project.service.ts**

```js
import { Injectable } from "@angular/core";
import { Observable, of } from "rxjs";

import { Project } from "./model/project";
import { PROJECTS } from "./data/projects";

@Injectable({
  providedIn: "root",
})
export class ProjectService {
  constructor() {}

  getProjects(): Observable<Project[]> {
    const projects = of(PROJECTS);
    return projects;
  }

  // getProjects(): Project[] {
  //   return PROJECTS;
  // }
}
```

**projects.component.ts**

First we need to add OnInit to our imports from @angular/core

```js
import { Component, OnInit, Input, Output, EventEmitter } from "@angular/core";
```

Then we need to let angular know that our component class implements OnInit

```js
export class ProjectsComponent implements OnInit {
```

Lastly, we chain a subscribe method onto our getProjects() call and use the callback to set our projects data.

```js
  projects: Project[] = [];
  getProjects(): void {
    // this.projects = this.projectService.getProjects();
    this.projectService
      .getProjects()
      .subscribe((projects) => (this.projects = projects));
  }
```

## Category and Tag Routing

Rather than passing data up and down our component tree and relying on condition output and pipes to filter our projects, let's handle this with route parameters.

Note: Performance wise this is a step backwards since we will be making unnecessary requests to fetch data that is highly unlikely to change within any given session, BUT it will be MUCH simpler to manage and we're fetching asynchronously so it doesn't overly degrade the UX...

The process here is nearly identical to using route params to load a single project.

- Add parameterized routes
- Add methods to our project service
- Update the projects component to use the new service methods

**app-routing.module.ts**

```js
import { NgModule } from "@angular/core";
import { RouterModule, Routes } from "@angular/router";
import { ProjectsComponent } from "./projects/projects.component";
import { ProjectDetailComponent } from "./project-detail/project-detail.component";

const routes: Routes = [
  { path: "projects", component: ProjectsComponent },
  { path: "projects/categories/:id", component: ProjectsComponent },
  { path: "projects/tags/:id", component: ProjectsComponent },
  { path: "projects/:id", component: ProjectDetailComponent },
];
```

**project.service.ts**

```js
  getProjectsByCategory(id: number): Observable<Project[]> {
    const projects = PROJECTS.filter((p) => p.category_id === id)!;
    return of(projects);
  }

  getProjectsByTag(id: number): Observable<Project[]> {
    const projects = PROJECTS.filter((p) => {
      if (p.tags.some((t) => t.id === id)) {
        return true;
      } else {
        return false;
      }
    })!;
    return of(projects);
  }
```

**projects.component.ts**

```js
import { ActivatedRoute } from '@angular/router';
import { Location } from '@angular/common';

...

export class ProjectsComponent implements OnInit {
  constructor(
    private projectService: ProjectService,
    private route: ActivatedRoute
  ) {}

...

  getProjectsByCategory(): void {
    const id = Number(this.route.snapshot.paramMap.get('id'));
    this.projectService
      .getProjectsByCategory(id)
      .subscribe((projects) => (this.projects = projects));
  }

  getProjectsByTag(): void {
    const id = Number(this.route.snapshot.paramMap.get('id'));
    this.projectService
      .getProjectsByTag(id)
      .subscribe((projects) => (this.projects = projects));
  }

  ngOnInit(): void {
    const segment: string = this.route.snapshot.url[1]?.path;

    if (segment === 'categories') {
      this.getProjectsByCategory();
    } else if (segment === 'tags') {
      this.getProjectsByTag();
    } else {
      this.getProjects();
    }
  }
```

Note: I'm taking a rather circuitous approach to determining whether we want all projects, projects by category, or projects by tag. That too could be parameterized instead of deconstructing the path, but for now this gets the job done.

http://localhost:4200/projects/categories/1

http://localhost:4200/projects/tags/1

If you're satisfied with how this works, you can go back through your projects, categories, and tags components and replace your event driven data-flow approach with simple routerLinks.

I'm not going to walk you through it, but you could also swap out the id based parameters with slugs if you choose.

#### Bug Fix:

Here's a fix so that changes in the route params triggers a re-fetch of the projects data.

**projects.component.ts**

```js
// replace the previous version of ngOnInit with this:

  ngOnInit(): void {
    this.route.params.subscribe((params) => {
      const segment: string = this.route.snapshot.url[1]?.path;
      if (segment === 'categories') {
        this.getProjectsByCategory();
      } else if (segment === 'tags') {
        this.getProjectsByTag();
      } else {
        this.getProjects();
      }
    });
  }
```

## Displaying Images

To display local images:

- Create an images folder under /src/assets.
- Access them by binding the to the src attribute of your img tag
  - Remember to wrap the string in single quotes when binding

```
<img [src]="'assets/images/JoshuaGiraffe.jpg'" alt="Josh Solomon" />
```

To set a background image in your CSS:

- The assets folder is available at the root of your app.

```js
body {
  background-image: url("/assets/images/JoshuaGiraffe.jpg");
}
```

Or... load your images externally from a CDN, S3 bucket, etc.
