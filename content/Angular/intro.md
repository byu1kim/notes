+++
title = "Intro"
weight = 3
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## What is Angular

Angular is an open-source front end development framework maintained by Google.

Built as a side project by Miko Hevery at Google, it was originally launched in 2010 as AngularJS. This original, AngularJS, was a JavaScript based framework with the aim of simplifying the process of building dynamic, single-page web applications via a reusable component architecture, two-way data binding, and a dependency injection system that made it easier to manage complex applications with components that rely on each other.

AngularJS became very popular, very quickly, and was adopted by many large organizations including Google, HBO, Nike, Paypal, and Microsoft.

With the rapid adoption of AngularJS, as well as the breakneck pace at which web development needs evolved in the mobile-explosion years, several performance and scalability limitations became apparent.

To address those limitations, the Angular team rewrote it from the ground up with a focus on providing a more modular, scalable, and performant framework using TypeScript as the primary language. It was released in 2016 simply as Angular and remains one of the most widely used front-end frameworks with a huge and active developer community.

https://angular.io/

## Advantabes and Disadvantages of Angular

#### Advantages

- Excellent separation of presentation and logic for data driven applications
- Great support for single page application development
- Convenient two-way data binding and validation
- Excellent browser compatibility
- Support by Google
- Huge global developer community.

#### Disadvantages

- Steep learning curve
- Incompatibilities between versions
- Overly sophisticated for smaller applications
- Limited SEO capabilities
- Decreasing popularity in recent years

https://insights.stackoverflow.com/trends?tags=reactjs%2Cangular

![image](https://learn.bcit.ca/content/enforced/868291-32660.202230/File_cf86e09855134664a7a20b6d22950a37_image.png?_&d2lSessionVal=mjk0WZvC3Kn5gY2H5nCjiYUGQ&ou=868291)

## What is TypeScript

TypeScript is a strict, syntactic superset of JavaScript developed and maintained by Microsoft.

Essentially, it is a language extension for JavaScript that provides static typing through type annotations which allows us to do type checking at compile time.

What that means is we can be specific with the data types that our code uses, including variables as well as the parameters and values returned from functions, and we can catch data type errors BEFORE they happen.

We develop in TypeScript and then when deploying or launching an app in dev mode it gets compiled down to regular JavaScript which is what the browser works with.

https://www.typescriptlang.org/

Though I will help you work through any TypeScript issues while we're working through our Angular / Vue module, I strongly urge you to get familiar with Typescript on your own time.

https://www.typescriptlang.org/docs/

## Angular Setup

The only software prerequisites for getting started with local development of Angular applications are having an LTS version of Node.js and NPM installed.

#### Installing the Angular CLI

For Windows users we will need to set the Execution Policy to RemoteSigned in order to install the Angular CLI globally.

Open a PowerShell terminal and run the following:

```
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned
```

Then, run the following to install the CLI

```
npm install -g @angular/cli
```

#### Angular Dev Tools

https://angular.io/guide/devtools

That's it. We're ready to create our first Angular app.

## Hello Angular

In VSCode, in a regular (non-WSL) terminal, navigate to the folder where you want to build your Hello Angular app.

Run the following CLI command:

```
ng new hello-angular
```

Accept the defaults for the prompts by pressing enter.

It may take a while as the packages are installed and the application is initialized.

The utility will also initialize a local Git repository for us.

Running Hello Angular
Navigate into the newly created folder.

```
cd hello-angular
```

Launch the application.

```
ng serve --open
```

This too may take a while the first time it runs, but upon completion it should open a browser to http://localhost:4200

## Key Angular Files

Here are some key points in the directory and file structure of an Angular application.

![img](https://learn.bcit.ca/content/enforced/868291-32660.202230/File_642c660651924668808bf7132bb9d0e2_image.png?_&d2lSessionVal=mjk0WZvC3Kn5gY2H5nCjiYUGQ&ou=868291)

##### src/main.ts

This is the main entry point to the application. It compiles the application and loads AppModule to run in the browser. You generally do not need to edit this.

![img](https://learn.bcit.ca/content/enforced/868291-32660.202230/File_28ee047347c14e0dbcf2b684a6ded70d_image.png?_&d2lSessionVal=mjk0WZvC3Kn5gY2H5nCjiYUGQ&ou=868291)

##### src/app/app.module.ts

Angular uses modules to group components and directives into libraries. Every application must have at least one module.

The root module tells the application what to load and how to load it.

In this case our root module is named AppModule and it is telling Angular to load and bootstrap AppComponent.

![img](https://learn.bcit.ca/content/enforced/868291-32660.202230/File_ae5c1b72d93844d1a8223fab8244d301_image.png?_&d2lSessionVal=mjk0WZvC3Kn5gY2H5nCjiYUGQ&ou=868291)

##### src/app/app.component.ts

This is the root component for our boilerplate application.

In the @Component decorator area we have:

- selector
  - This defines a custom element which is what we use to include the component in any HTML view
- templateUrl
  - This defines the path to the HTML view
- styleUrls
  - This is an array of stylesheets that the component uses

Below that is the class definition which is where the data and functions to support our component will be defined. In this case the class stores a property (called a model, in Angular) named title.

![im](https://learn.bcit.ca/content/enforced/868291-32660.202230/File_545119beab9b40e8a4fca32990aa1dcf_image.png?_&d2lSessionVal=mjk0WZvC3Kn5gY2H5nCjiYUGQ&ou=868291)

##### src/app/app.component.html

This contains the HTML template for the view.

As the exciting comment suggests, let's delete the placeholder content and just output an h1 tag containing the title.

```
<h1>Welcome to {{ title }}!</h1>
```

As soon as the file is saved the browser will reload. The angular server is watching for changes and handling hot reloading.

![img](https://learn.bcit.ca/content/enforced/868291-32660.202230/File_bc7a58086df74536bb04d0c8af2e343e_image.png?_&d2lSessionVal=mjk0WZvC3Kn5gY2H5nCjiYUGQ&ou=868291)

##### src/index.html

This contains the parent view for our single page application.

A couple of key things to note:

It contains the <app-root></app-root> selector that was defined in the component.

There is an absence of JavaScript references in this file. All of the JavaScript references are inserted into the application when it is transpiled, just before it runs in the browser.

![img](https://learn.bcit.ca/content/enforced/868291-32660.202230/File_4c926c8a1db24caf8895f51b257479a2_image.png?_&d2lSessionVal=mjk0WZvC3Kn5gY2H5nCjiYUGQ&ou=868291)

## Key Angular Concepts and Terminology

#### Components

Components are the basic building blocks of an Angular application.

Components define views which are the sets of screen elements that Angular displays according to our program logic and data.

A component's job is to enable the user experience by presenting properties and methods for data binding that mediate between the view and the application logic.

#### Modules

Modules collect blocks of code into single purpose units that are exportable and can be imported into other modules.

An Angular application is defined by a set of NgModules classes that group components, services, pipes, and other functionality into organized units.

#### Services

Services are injectable classes that don't directly involve the view or application logic.

Services are good for tasks such as fetching data, validating user input, logging and error handling.

#### Decorators

Decorators are used to attach metadata to classes and properties so Angular can know what the given class or property is as well as how it should work. Modules, components, and services are all classes that use decorators to mark their type and provide the needed information to allow Angular to use them.

For instance, the metadata for a component class defines the selector, template, and stylesheets used by the component.

#### Templates

Templates are where we combine HTML with Angular markup to control what is displayed to the user.

In templates we have directives which provide program logic and binding markup which allows us to both interpolate property values into our HTML (Property Binding) and also to respond to user input and interaction by updating the application data (Event Binding).

#### Pipes

Pipes are used to format, filter, and manipulate data before it is displayed to the user.

#### Directives

Directives are special HTML attributes that modify the behavior of elements in the template.

#### Dependency Injection

Dependency Injection is a design pattern that facilitates the interaction between parts of a application by defining what other parts of the application any given part needs access to in order to function. It is made up of two main roles: dependency consumers and dependency providers.

For more information on terms and concepts, refer to:

The Angular glossary: https://angular.io/guide/glossary

The Angular guide: https://angular.io/guide/understanding-angular-overview

## Adding a Class to our Component

Let's add a TimeOfDay class to our component to explore things a bit further.

Near the top of app.component.ts, after the import but before the @Component decorator, add the following code:

```js
export class TimeOfDay {
  hour: number = 10;
  meridiem: string = "AM";

  constructor() {
    const date = new Date();
    let hours = date.getHours();
    const ampm = hours >= 12 ? "PM" : "AM";
    hours = hours % 12;
    hours = hours ? hours : 12;
    return { hour: hours, meridiem: ampm };
  }
}
```

Next, within the AppComponent class definition let's add in a name property and also use our TimeOfDay class to add a public property named timeOfDay.

```js
export class AppComponent {
  title = 'hello-angular';
  name = 'Josh';
  public timeOfDay = new TimeOfDay();
}
```

Update our template to use the name and timeOfDay properties:

#### app.component.html

```js
<h1>Welcome to {{ title }}!</h1>
<h2>Hello, {{ name }}.</h2>
<h3>It is currently {{ timeOfDay.hour }} {{ timeOfDay.meridiem }}</h3>
```

#### Conditional Output with \*ngIf

With Angular we can use the \*ngIf directive to conditionally output an element in our template.

In its simplest form we just add \*ngIf="expression" and if the expression returns a truthy value the element is output.

##### app.component.html

```
<h1 *ngIf="title">Welcome to {{ title }}!</h1>
```

Test it out.

Try changing the title property in your appComponent to an empty string.

#### Complex Expressions

We can also use more complex expressions with \*ngIf, such as checking if a property has a specific value as well as instructing Angular to output something else if the expression returns a falsy value.

Here's an updated template that conditionally outputs a morning or afternoon greeting.

```js
<h1 *ngIf="title">Welcome to {{ title }}!</h1>
<h2>
  <ng-container *ngIf="timeOfDay.meridiem === 'AM'; else afternoonGreeting">
    Good morning</ng-container
  >, {{ name }}
</h2>

<h3>It is currently {{ timeOfDay.hour }} {{ timeOfDay.meridiem }}</h3>

<ng-template #afternoonGreeting>
  <ng-container>Good afternoon</ng-container>
</ng-template>
```

In this example I am outputting a fallback ng-template named afternoonGreeting if the meridiem is not AM.

I am also utilizing <ng-container> which allows us to use structural directives (such as \*ngIf) without adding any extra elements to the DOM.

In the AppComponent class, test out specific property values for hour and meridiem.

```js
export class AppComponent {
  title = 'hello-angular';
  name = 'Josh';
  // public timeOfDay = new TimeOfDay();
  public timeOfDay: TimeOfDay = {
    hour: 10,
    meridiem: 'AM',
  };
}
```

## Two Way Data Binding - [(ngModel)] Directive

So far we've output the properties values of a component into the DOM, and we've also added in some conditional logic, but with two way data binding we can also allow the property values to be modified by user interactions via the ngModel directive and HTML inputs.

#### Angular Core Modules

Angular includes an assortment of core modules that provide predefined directives, services, classes, and pipes to facilitate building our applications.

We'll explore other core modules later on, but for now we'll need to import the FormsModule so that we can set up our two-way data binding between form inputs and and our component properties

#### Importing the Angular FormsModule

Revise app.module.ts as follows:

```js
import { NgModule } from "@angular/core";
import { BrowserModule } from "@angular/platform-browser";
import { FormsModule } from "@angular/forms";
import { AppComponent } from "./app.component";

@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule, FormsModule],
  providers: [],
  bootstrap: [AppComponent],
})
export class AppModule {}
```

On line 3 we're importing the the FormsModule from the @angular/ namespace (which is typically where we'll find core modules), and then on line 8 we're adding FormsModule to the imports array in the @NgModule decorator which is how we define this modules dependencies.

#### Binding an Input to a Model

Add the following to our template in app.component.html

```js
<label for="name">Name: </label
><input type="text" name="name" id="name" [(ngModel)]="name" />
```

Edit the value in the name input and you'll see the greeting text in the DOM update instantly. Also note that the initial value of the input was set equal to the name property set in our component.

Side note: There isn't actually a value attribute on the input. Take a look in DevTools and you'll see what I mean.

![img](https://learn.bcit.ca/content/enforced/868291-32660.202230/File_29e4ee9ab58a4d13bb5c3b2027193679_image.png?_&d2lSessionVal=mjk0WZvC3Kn5gY2H5nCjiYUGQ&ou=868291)

#### Practice Exercise

Add two more inputs into the template and bind them to the hour and meridiem properties of our timeOfDay object.

Tips:

- The hour input should be type number
  - Set min and max attributes (go with 1 and 24, for now)
  - Bind it to timeofDay.hours
- AM / PM should be a select with options of AM and PM
  - The binding should be on the select element, not the options

## Event Handling with (ngModelChange)

We've bound our inputs to their corresponding models (properties), but at this point the hour and meridiem handling via the inputs are are independent of each other and when we reach the max value for hour we'd need to manually change the value back to 1 rather than have it loop back automatically.

To handle this, it would be useful to have a function that is run whenever the hour changes such that:

- If the hour input is set to a value greater than 12 the hour property is set back to 1 (which, due to the two-way binding will also set the input to 1)
- Meridiem automatically changes from AM to PM accordingly.

I'll let you work out the body of the function yourself, but the event handling setup will look like this:

- Add a function within the AppComponent class

```
export class AppComponent {
  title = 'hello-angular';
  name = 'Josh';
  // public timeOfDay = new TimeOfDay();
  public timeOfDay: TimeOfDay = {
    hour: 10,
    meridiem: 'AM',
  };
  handleHourChange() {
    console.log(this.timeOfDay.hour);
    // fill in the rest of this to simulate clock behaviour
  }
}
```

- On the "hour" input, bind the ngModelChange event to the function above:

```
<input
  type="number"
  name="hour"
  id="hour"
  size="2"
  min="1"
  max="24"
  style="width: 3em"
  [(ngModel)]="timeOfDay.hour"
  (ngModelChange)="handleHourChange()"
/>
```

Note: In the example above we are listening for the ngModelChange event. This works because we have already bound the input to timeOfDay.hour via [(ngModel)]. If we wanted to listen for change events on an input that isn't bound to a model, we could do so by listening for (change) instead of (ngModelChange). You'll see the equivalent of that when we set up click event listening in a couple of sllides.

My version of the handleHourChange is below, in white:

handleHourChange() {
if (this.timeOfDay.hour > 12) {
this.timeOfDay.hour = 1;
}
if (this.timeOfDay.hour === 12) {
this.timeOfDay.meridiem === 'AM'
? (this.timeOfDay.meridiem = 'PM')
: (this.timeOfDay.meridiem = 'AM');
}
}

## Loops with \*ngFor

We can use the \*ngFor directive to loop through a collection of items.

Let's put together a set of colour swatches.

#### Define a ColourSwatch Class

Above our TimeOfDay class in app.component.ts , define a ColourSwatch class with two string properties hexCode and name.

```
export class ColourSwatch {
  hexCode: string;
  name: string;
}
```

#### TypeScript Micro-Tangent

Uh oh. You should be seeing some red squiggles under the property names!

![g](https://learn.bcit.ca/content/enforced/868291-32660.202230/File_5636fbc2ebf143ba935138fce2bf9970_image.png?_&d2lSessionVal=mjk0WZvC3Kn5gY2H5nCjiYUGQ&ou=868291)

That's because even though we've specified the data types for the properties we don't have a constructor that for this class that initialized the values to the expected type.

There are a few ways we could handle this, and I don't want to go on too far a tangent, so let me just introduce you to the definite assignment assertion operator, !.

By appending ! to each of those properties we are telling TypeScript that we will definitely initialize their values appropriately.

```
export class ColourSwatch {
  hexCode!: string;
  name!: string;
}
```

Error be gone. Let's carry on.

#### Initialize an array of ColourSwatch

Just below our class definition add:

```
const SWATCHES: ColourSwatch[] = [
  { hexCode: '#000000', name: 'Black' },
  { hexCode: '#FFFFFF', name: 'White' },
  { hexCode: '#BE3455', name: 'Viva Magenta' },
  { hexCode: '#6667AB', name: 'Very Peri' },
];
```

#### Set them as a property of the AppComponent

Above the handleHourChange method, add:

```
public swatches = SWATCHES;
```

#### Output a div containing the name for each Swatch in the Template

In app.component.html

```
<div *ngFor="let swatch of swatches">{{swatch.name}}</div>
```

#### Accessing the Index While Looping

Purely for demonstration purposes, let's also number our colour swatches based on its index in the array.

We can do so by adding referencing the index as follows:

```
<div *ngFor="let swatch of swatches; let i = index">
  {{ i + 1 }}. {{ swatch.name }}
</div>
```

Notes:
*ngFor works with collections. With it, you can't directly loop between a range of numbers as you might elsewhere in JavaScript. If you do want to use *ngFor loop over a range of numbers, create an array of the numbers first then iterate through that.

There also isn't an ngWhile directive. If you really want to repeatedly output a block you could use a combination of *ngIf and *ngFor directives, but... you also need to keep in mind that only one structural directive is allowed per element.

There is, of course, far more to \*ngFor.

https://angular.io/api/common/NgFor

## Inline Styling of Elements

We can apply inline styles to elements with the usual style="" attributes, and we can directly echo out or use expressions to set style values as well.

#### Colouring our Swatches

```
<div
  *ngFor="let swatch of swatches; let i = index"
  style="background-color: {{ swatch.hexCode }};
  color: {{ swatch.name === 'Black' ? 'white' : 'black' }}"
>
  {{ i + 1 }}. {{ swatch.name }}
</div>
```

The above will work, but is likely to cause warnings in your editor. The values are also only indirectly bound to the properties (via the \*ngFor).

To bind our styles and clean up the syntax, we could rewrite the above as:

```
<div
  *ngFor="let swatch of swatches; let i = index"
  [style.backgroundColor]="swatch.hexCode"
  [style.color]="swatch.name === 'Black' ? 'white' : 'black'"
>
  {{ i + 1 }}. {{ swatch.name }}
</div>
```

The thing to understand here is that the value in double quotes to the right of the equals is an expression.

Let's add a couple more styles to control the padding and width of our swatches before moving on.

#### Style Binding with String Expressions

```
[style.width]="'200px'"
```

#### Style Binding with Units

```
[style.padding.rem]="1"
```

## Event Handling - (click) Directive

Let's make our colour swatches clickable.

The process is very much the same as how we handled change events.

First, let's add a selectedColourSwatch property to our component.

The catch is, I don't want to specify a selected colour swatch initially. To handle this without running afoul of TypeScript rules, I can use the Union datatype to specify that the property will either be a ColourSwatch or undefined.

##### app.component.ts

```
selectedColourSwatch: ColourSwatch | undefined;
```

#### Add an Event Handler Method

Still in our AppComponent, add:

```
  handleColourChange(swatch: ColourSwatch) {
    this.selectedColourSwatch = swatch;
  }
```

#### Registering the Event Listener

While we're looping through the swatches in out template, we can add the event listener with:

```
  (click)="handleColourChange(swatch)"
```

To verify that our click handling is working, open the Angular Dev Tools in your browser and select the app-root component.

Initially, you won't see a selectedColourSwatch property.

![img](https://learn.bcit.ca/content/enforced/868291-32660.202230/File_b05d0db0b84145908289e543f64168e4_image.png?_&d2lSessionVal=mjk0WZvC3Kn5gY2H5nCjiYUGQ&ou=868291)

After clicking on a swatch, however, you should see it in the property list.

![a](https://learn.bcit.ca/content/enforced/868291-32660.202230/File_ebc2d470e2c941919dc57d2323c9e2b6_image.png?_&d2lSessionVal=mjk0WZvC3Kn5gY2H5nCjiYUGQ&ou=868291)

## Styling Elements with [ngStyle]

Let's conditionally output a div showing the currently selected colour swatch and explore one more way of binding styles to it: [ngStyle]

##### app.component.html

```
<div
  *ngIf="selectedColourSwatch"
  [ngStyle]="{
    'background-color': selectedColourSwatch.hexCode,
    padding: '1rem'
  }"
>
  <h2 [style.color]="selectedColourSwatch.name === 'Black' ? 'white' : 'black'">
    You Selected {{ selectedColourSwatch.name }}.
  </h2>
</div>
```

As you can see, there are many ways to apply styles to an element in Angular.

The biggest difference between [style.property] and [ngStyle] is that with the former we set a single property at a time whereas with [ngStyle] we pass in an object that can contain multiple properties.

Though there are slight differences in how they work behind the scenes, which you use will depend on the situation and your personal preference.

## CSS and SASS with Angular

If you look at the @Component decorator in app.component.ts you will see that it defines three things:

- selector
- templateUrl
- styleUrls

```
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css'],
})
```

We can write plain-old CSS in here and it will both work as expected AND be scoped to this specific component.

Let's add a very simple hover effect for our colour swatches.

##### app.component.css

```
*,
*:before,
*:after {
  box-sizing: border-box;
}

div:hover {
  margin-left: 0.3rem;
}
```

Since we're working with the Angular CLI we can also use Sass with no additional setup.

#### Angular Sass

For demonstration purposes let's create a app.swatches.scss file, move our hover styling into it, and add the app.swatches.scss file to the styleUrls array.

##### app.swatches.scss

```
div {
  margin: 0;
  &:hover {
    margin-left: 0.3rem;
  }
}
```

##### app.component.ts

```
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css', './app.swatches.scss'],
})
```

## Dynamically Setting Classes

Now that we have CSS in play, let's start adding classes to our template elements.

The simplest form of this is with class binding, which allows us to conditionally add a single class to an element based on the result of an expression.

#### Class Binding

Let's add a selected class to the selected colour swatch.

First, in app.swatches.scss add a rule for div.selected:

```
div {
  margin: 0;
  &:hover {
    margin-left: 0.3rem;
  }
  &.selected {
    font-size: 1.2rem;
    font-weight: bold;
    &::before {
      content: "\00BB";
    }
  }
}
```

Now, in app.component.html, in our \*ngFor loop, add the following:

```
[class.selected]="swatch == selectedColourSwatch"
```

#### Setting Multiple Classes with [ngClass]

First, let's add three classes to app.component.css (or .scss)

```
.rounded {
  border-radius: 2rem;
}

.bordered {
  border: 2px solid
black;
}

.text-centered {
  text-align: center;
}
```

There are several ways that we can set multiple classes on an element using the [ngClass] directive.

Our expression can return a space separated list of one or more classes as a string

```
[ngClass]="'rounded bordered text-centered'"
```

It can return an array of strings

```
[ngClass]="['rounded', 'bordered', 'text-centered']"
```

It can return an array, but we can conditionally control the items

```
[ngClass]="[
  'rounded',
  'text-centered',
  selectedColourSwatch.name === 'White' ? 'bordered' : ''
]"
```

Or, we can return an object where each property whose value is truthy is added

```
[ngClass]="{
  rounded: true,
  centered: true,
  bordered: selectedColourSwatch.name === 'White'
}"
```

## Assignment

```
export class Category {
  id!: number;
  name!: string;
  slug!: string;
}

export class Tag {
  id!: number;
  name!: string;
  slug!: string;
  pivot?: any;
}

export class Project {
  'id': number;
  'title': string;
  'slug': string;
  'excerpt': string;
  'body': string;
  'url': string | null;
  'published_date': string | null;
  'image': string | null;
  'thumb': string | null;
  'category_id': number | null;
  'created_at': string;
  'updated_at': string;
  'category': Category | null;
  'tags': Tag[] | undefined;
}
```
