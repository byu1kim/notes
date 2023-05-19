## Angular Pipes

Pipes allow us to easily and consistently format values and/or transform data.

Angular provides a set of built-in pipes that we can use, as needed, right away.

For instance, if we want to display the title for each of our projects in uppercase, we can use the UpperCasePipe to do so right in our template.

##### app.component.html

```
<h2>{{ project.title | uppercase }}</h2>
```

To see a full list of the built in pipes, check the API docs:

https://angular.io/api/common#pipes

For a full guide on using pipes:

https://angular.io/guide/pipes

## Creating a Custom Pipe

In addition to the built-in pipes that Angular provides, we can write our own custom pipes which opens up MUCH greater possibilities for transforming data.

Let's use the Angular CLI to generate a custom pipe so we can filter our projects by tag.

In your terminal, run:

```
ng generate pipe project-filter
```

By doing this via the CLI not only is project-filter.pipe.ts created for us (with boilerplate), but the pipe is also imported into app.module.ts and added to the declarations in the @NgModule decorator.

##### app.module.ts

```
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';

import { AppComponent } from './app.component';
import { ProjectFilterPipe } from './project-filter.pipe';

@NgModule({
  declarations: [AppComponent, ProjectFilterPipe],
  imports: [BrowserModule],
  providers: [],
  bootstrap: [AppComponent],
})
export class AppModule {}
```

#### Filtering Projects by Tag

To filter our projects by tag we will need to:

- Import the Project and Tag classes from app.component.ts
- Pass our array of Projects and optionally a Tag into the pipe.
- Check if the project.tags of each Project includes the provided Tag
  - In this case I use JSON.stringify on both in order to do a simple string indexOf check for the presence of the tag in the project tags.
- Return the filtered array of Projects.

##### project-filter.pipe.ts

```
import { Pipe, PipeTransform } from '@angular/core';
import { Project, Tag } from './app.component';

@Pipe({
  name: 'projectFilter',
})
export class ProjectFilterPipe implements PipeTransform {
  // The transform method will receive our array of Projects and return only those that contain our Tag if defined
  transform(projects: Project[], tag: Tag | undefined): Project[] {
    let filteredProjects = [];
    if (tag) {
      filteredProjects = projects.filter((project) => {
        // Convert the project tags as well as the tag to strings
        // Since indexOf returns -1 if not found, add 1 to make not found a falsy value
        return JSON.stringify(project.tags).indexOf(JSON.stringify(tag)) + 1;
      });
    } else {
      filteredProjects = projects;
    }
    return filteredProjects;
  }
}
```

#### Using our projectFilter Pipe

Since our custom pipe was imported and registered in app.module.ts, we can use it anywhere in our app template files.

The @Pipe decorator set the name of our pipe to projectFilter, so when we're going to iterate through our projects in app.component.html, we just send the projects and tagFilter into the pipe as follows:

```
<article
      *ngFor="let project of projects | projectFilter : tagFilter"
      [class.hidden]="
        categoryFilter && project.category?.id != categoryFilter.id
      "
    >
```

## Refactoring our List Components

Right now we have absolutely everything happening in our AppComponent, which is far from optimal.

A well structured application is modular, so let's start moving things in that direction beginning by creating shells for a ProjectsComponent, TagsComponent, and CategoriesComponent.

#### Creating Components

Use the Angular CLI to create a new components named projects, tags, and categories respectively:

```
ng generate component projects
```

That will create a projects folder under src/app containing all of the files needed for our component:

- The component
  - projects.component.ts,
- The template
  - projects.component.html
- The stylesheet
  - projects.component.scss
- A testing file
  - projects.somponent.spec.ts

It also updates app.module.ts to import and declare the component so we can use it.

Open projects.component.ts and you'll find that it has created a shell for our component and populated the @Component decorator with the selector, templateUrl, and styleUrls.

```
import { Component } from '@angular/core';

@Component({
  selector: 'app-projects',
  templateUrl: './projects.component.html',
  styleUrls: ['./projects.component.scss']
})
export class ProjectsComponent {

}
```

Repeat for categories and tags.

Leave these in their skeleton form, for now. We're going to get back to this shortly, but before doing so let's improve how we're handling the associated data.

## Creating Models for our Data

#### Generating Models

The next step in cleaning up our application is to move our Project, Category, and Tag class definitions out of app.component.ts and redefine them as interfaces

ng generate interface model/project

ng generate interface model/category

ng generate interface model/tag

##### category.ts

```
export interface Category {
  id: number;
  name: string;
  slug: string;
}
```

##### tag.ts

```
export interface Tag {
  id: number;
  name: string;
  slug: string;
}
```

Note: I've stripped the pivot property out. We'll remove it from our data too.

##### project.ts

```
import { Category } from './category';

export interface Project {
  id: number;
  title: string;
  slug: string;
  excerpt: string;
  body: string;
  url: string | null;
  published_date: string | null;
  image: string | null;
  thumb: string | null;
  category_id: number | null;
  created_at: string;
  updated_at: string;
  category: Category | null;
  tags: any;
}
```

## Extracting our Data

Currently our AppComponent is cluttered with dummy data for our projects, categories, and tags.

Let's clean that up.

#### Convert Our JSON files to TypeScript

- Create a data folder to store our files
- Create projects.ts
- Import the Project interface from ../model/project
- Cut and paste the PROJECTS const from app.component.ts
- Export the PROJECTS const.

##### src/app/data/projects.ts

```
import { Project } from '../model/project';

export const PROJECTS: Project[] = [
  {
    id: 1,
    title: 'Portfolio Showcase',
    slug: 'portfolio-showcase',

    ...
```

Rinse and repeat for categories and tags.

## Angular Services

Services allow us to modularize our code by separating out general processing tasks from the view-related functionality that optimally is the only thing that our components should be responsible for.

Services are injectable, so by creating narrowly focused, well defined, service classes we can re-use their functionality in any components that need them - both cleanly and efficiently.

#### Create a ProjectService

```
ng generate service project
```

That gives us our boilerplate service.

- Import the Project interface
- Import the PROJECTS data
- Add a method that returns the PROJECTS

##### project.service.ts

```
import { Injectable } from '@angular/core';
import { Project } from './model/project';
import { PROJECTS } from './data/projects';

@Injectable({
  providedIn: 'root',
})
export class ProjectService {
  constructor() {}
  getProjects(): Project[] {
    return PROJECTS;
  }
}
```

Do the same for categories and tags.

## Using Services in our Components

To use the ProjectService in ProjectsComponent we will need to:

- Import the ProjectService
- Add a constructor with a private parameter of type ProjectService
- Add a projects property to hold our array of Projects
  - Initialize it to an empty array
- Add a getProjects method that sets the projects property to the array returned from projectService.getProjects()
- Add an ngOnInit() method that calls the getProjects() method.

```
import { ProjectService } from '../project.service';

export class ProjectsComponent {
  constructor(private projectService: ProjectService) {}

  projects: Project[] = [];
  getProjects(): void {
    this.projects = this.projectService.getProjects();
  }

  ngOnInit(): void {
    this.getProjects();
  }

...
```

Same same for categories and tags.

## Angular Data Flow

We're almost back to having our app back up and running, but now that we've created a split out individual components we need to be able to pass our categoryFilter and tagFilter down from the AppComponent to the ProjectsComponent and also be able to set them from the ProjectsComponent as well as from the CategoriesComponent or TagsComponent.

In general, what this looks like is:

#### Passing Data to Child Components

In the child component we need to:

- Import Input from @angular/core
- Decorate any property that will be provided by the parent with @Input()

In the parent component we need to:

- Set a property to the value we want to pass down
  - This happens in the class file - app.component.ts
- Use property binding to actually send the value to the input
  - This happens in the template - app.component.html

For example, we want to send categoryFilter and tagFilter from our AppComponent to the ProjectsComponent, so:

##### projects.component.ts

```
import { Component, Input } from '@angular/core';

...

@Input() categoryFilter: Category | undefined;
@Input() tagFilter: Tag | undefined;
```

##### app.component.ts

```
...

  tagFilter: Tag | undefined;

  setTagFilter(tag: Tag) {
    this.tagFilter = tag;
  }

  categoryFilter: Category | undefined;
  setCategoryFilter(category: Category) {
    this.categoryFilter = category;
  }
  clearFilters() {
    this.categoryFilter = undefined;
    this.tagFilter = undefined;
  }
```

##### app.component.html

```
...

    <app-projects
      [categoryFilter]="categoryFilter"
      [tagFilter]="tagFilter"
    ></app-projects>

...
```

#### Passing Data from Child to Parent

In the child component.ts we need to:

- Import Output and EventEmitter
- Decorate a property in the child with @Output
- Set that @Output property to a new EventEmitter
- Add a method that, when called emits the appropriate event.

In the child component.html we need to:

- Bind an event handler to the event emitting method in the component

In the parent component.ts we need to:

- Define a method that will take the appropriate action when the emitted event is received

In the parent component.html we need to:

- bind a custom event listener to the handler above.

Putting that all together...

##### projects.component.ts

```
import { Component, Input, Output, EventEmitter } from '@angular/core';

...

  @Input() categoryFilter: Category | undefined;
  @Output() newCategoryFilterEvent = new EventEmitter<Category>();
  @Input() tagFilter: Tag | undefined;
  @Output() newTagFilterEvent = new EventEmitter<Tag>();

  setCategoryFilter(category: Category) {
    this.categoryFilter = category;
    this.newCategoryFilterEvent.emit(category);
  }

  setTagFilter(tag: Tag) {
    this.tagFilter = tag;
  }

  clearFilters() {
    this.categoryFilter = undefined;
    this.tagFilter = undefined;
  }

```

##### projects.component.html

```
<article
  *ngFor="let project of projects | projectFilter : tagFilter"
  [class.hidden]="categoryFilter && project.category?.id != categoryFilter.id"
>
  <section>
    <h2>{{ project.title | uppercase }}</h2>
    <div>{{ project.excerpt }}</div>
  </section>
  <footer>
    <div *ngIf="project.category" (click)="setCategoryFilter(project.category)">
      Category: <span>{{ project.category.name }}</span>
    </div>
    <div *ngIf="project.tags?.length">
      Tags:
      <span *ngFor="let tag of project.tags" (click)="setTagFilter(tag)">
        {{ tag.name }}
      </span>

    </div>
  </footer>
</article>
```

##### app.component.ts

```
...

export class AppComponent {
  title = 'Solomon Showcase';
  date = new Date();
  author = 'Josh Solomon';

  tagFilter: Tag | undefined;
  setTagFilter(tag: Tag) {
    this.tagFilter = tag;
  }

  categoryFilter: Category | undefined;
  setCategoryFilter(category: Category) {
    this.categoryFilter = category;
  }

...
```

##### app.component.html

```
..

    <app-projects
      [categoryFilter]="categoryFilter"
      (newCategoryFilterEvent)="setCategoryFilter($event)"
      [tagFilter]="tagFilter"
      (newTagFilterEvent)="setTagFilter($event)"
    ></app-projects>

...
```

## Cleaning up the AppComponent

Once we have implemented all of that, we can finally clean up our AppComponent and its template.

##### app.component.ts

```
import { Component } from '@angular/core';
import { ProjectsComponent } from './projects/projects.component';
import { CategoriesComponent } from './categories/categories.component';
import { TagsComponent } from './tags/tags.component';

import { Project } from './model/project';
import { Category } from './model/category';
import { Tag } from './model/tag';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss'],
})
export class AppComponent {
  title = 'Solomon Showcase';
  date = new Date();
  author = 'Josh Solomon';

  tagFilter: Tag | undefined;

  setTagFilter(tag: Tag) {
    this.tagFilter = tag;
  }

  categoryFilter: Category | undefined;
  setCategoryFilter(category: Category) {
    this.categoryFilter = category;
  }
  clearFilters() {
    this.categoryFilter = undefined;
    this.tagFilter = undefined;
  }
}
```

##### app.component.html

```
<header>
  <h1>{{ title }}</h1>
</header>
<main>
  <section id="side-nav">
    <article *ngIf="categoryFilter || tagFilter">
      <h2>Filter:</h2>
      <div class="current-filter" (click)="clearFilters()">
        {{ categoryFilter?.name || tagFilter?.name }}
        <i class="bi bi-trash"></i>
      </div>
    </article>

    <h3>Categories</h3>
    <article id="categories">
      <app-categories
        [categoryFilter]="categoryFilter"
        (newCategoryFilterEvent)="setCategoryFilter($event)"
      ></app-categories>
    </article>
    <h3>Tags</h3>
    <article id="tags">
      <app-tags
        [tagFilter]="tagFilter"
        (newTagFilterEvent)="setTagFilter($event)"
      ></app-tags>
    </article>
  </section>
  <section id="projects">
    <app-projects
      [categoryFilter]="categoryFilter"
      (newCategoryFilterEvent)="setCategoryFilter($event)"
      [tagFilter]="tagFilter"
      (newTagFilterEvent)="setTagFilter($event)"
    ></app-projects>
  </section>
</main>
<footer>&copy; {{ date.getFullYear() }} {{ author }}</footer>
```

#### Refactoring Stylesheets

You'll still likely need to refactor your stylesheets because they are scoped to the component that they're imported by. Styles in app.component.scss will NOT be applied to output from projects.component.html etc., but if you've made it this far moving your styles around should be easy as pi.
