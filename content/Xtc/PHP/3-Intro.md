+++
title = "Laravel Intro"
weight = 3
pre = "- "
+++

## Laravel

Laravel is a full stack web application managing a PHP framework

#### Laravel Sail

Sail was created by the Laravel team as a tool to expedite and Dockerize Laravel development with an emphasis on modern development practices using the latest tools and technologies

#### Launching Laravel App

- Window : Open Ubuntu terminal in VSC
- Make sure Docker is running : empty containers, images

In your working directory,

```
curl -s "https://laravel.build/[name]" | bash
cd [name]
./vendor/bin/sail up -d   //start the container
./vendor/bin/sail stop   //stop the container
```

- attached mode : output is being directed into the terminal
- detached mode : add -d after sail up (faster), after initial run with attached mode

#### Setting up a Shell Alias (Window)

- Check which shell is running : `$ ps -p $$`
- Switch to home directory : `$ cd ~`
- Verify .bashrc exists : `$ ls -alh`
- Open .bashrc with vi : `$ vi .bashrc`
- Insert mode : type i
- Paste in the following Alias definition (around line 100)

```
alias sail='[ -f sail ] && sh sail || sh vendor/bin/sail'
```

- Save and close : `esc` > `:wq` > `enter`
- Restart the shell to use the alias : ./vender/bin/sail up -> `sail up`

#### Exploring the App

- docker-compose.yml : configuration for the group of images/containers
- composer.json : like package.json
- artisan : Artisan is the CLI included with Laravel.

## Blade

Blade is the templating engine that is included with Laravel.

Blade is lightweight, relatively straightforward to use, compiles down into plain PHP (so you don't have to write the PHP yourself) and is cached until the template is modified which makes it fast.

Blade templates are stored in the resources/views directory and have the extension .blade.php

#### Display data from Routes

```html
<?php echo $name; ?>
```

or

```
{{ $name }}
```

#### Default Variables using ?? (Null coalescing operator)

```
{{ $name ?? "Default" }}
```

## Routes

#### Routes configure

app/Providers/RouteServiceProvider.php

[ routes/web.php ]

```
<?php
use Illuminate\Support\Facades\Route;
Route::get('/', function () { return view('name-of-blade'); }
```

#### Passing the data

Passing Single Data

```
Route::get('/', function () { return view('name-of-blade', ['name' => 'Byul']); }
```

Passing Multiple Data

```
view('welcome, ['name'=>'aa', 'sal'=>'aa']) or
view('welcome')->with('name','aa')->with('sal','aa')
```

Route Parameters

```
Route::get('/greetings/{name}', function ($name) {
    return view('welcome')->with('name', $name) });
```

Optional Route Parameters

```
Route::get('/greetings/{name?}', function ($name='aa') {
    return view('welcome')->with('name', $name) });
```

- name is an optional route parameter
- aa is a default value

Fallback Routes : Unhandled requests (404 errors)

```
Route::fallback(function() {
    return view('404page-view') });
```

## Controller

Note:
If for some reason you are working locally instead of via Sail, you would run:

```
php artisan list
```

You will encounter php artisan ... in many online tutorials. Just replace php with sail if you're sailing along with containers.

```
sail artisan make:controller GreetingController
```

**app/Http/Conrollers/GreetingController.php**

```php
class GreetingController extends Controller
{
    public function greet($name = 'Earthling') {
        return view('welcome')
        ->with('name', $name)
        ->with('salutation', 'Greetings');
    }
}
```

**routes/web.php**

Let's change our /greetings/{name?} route to call the greet method in our controller:

```php
Route::get('/greetings/{name?}', [GreetingController::class, 'greet']);
```

Note: We don't need to explicitly pass the route parameter to controller. It is passed automatically.

We do, however, need to make our controller available for use.

We can import the class by adding the following near the top of our file

```php
use App\Http\Controllers\GreetingController;
```

Or, to make things easier and avoid typos, we can use the PHP Namespace Resolver extension and simply place our cursor within GreetingController and press Ctrl-Alt-i to have the class imported for us.

https://marketplace.visualstudio.com/items?itemName=MehediDracula.php-namespace-resolver

## Blade Layout Options

In order to set up our layouts and have views within other views, there are several ways we can go.

We can either:

- Template Inheritance
  - Use the @section directive to define sections of content
  - Use the @yield directive to display the contents of a given section
  - Use the @extends directive to specify which layout a child view should "inherit".
- Include views directly within other views using the @include directive
  - There are also conditional variants @includeIf, @includeWhen, @includeUnless, and @includeFirst
- Use Blade components and slots.

For brevity's sake and to minimize confusion, we'll focus mainly on components and slots, but first let's take a quick look at the older way of doing things.

## Layouts through Inheritance

#### Layouts the Legacy Way

Let's begin by deconstructing welcome.blade.php.

Create a new folder named layouts under resources/views/, and within that also create a folder named partials.

In the layouts folder, create a file named app.blade.php

**resources/views/layouts/app.blade.php**

Copy the entire contents of welcome.blade.php into app.blade.php.

Then, cut out the div that exists inside the body tag and replace it with:

```
@yield('content')
```

Go back to welcome.blade.php and replace the all of the code with just the div you cut out from the previous step.

The @yield directive above is telling the Blade engine that we will be defining a section with the indicated name in the view file that extends this layout.

At the top of the file add in @extends('layouts.app') and wrap the div with @section('content') and @endsection.

```php
@extends('layouts.app')
@section('content')

        <div class="relative flex items-top justify-center min-h-screen bg-gray-100 dark:bg-gray-900 sm:items-center py-4 sm:pt-0">
            {{ $salutation ?? 'Hello' }}, {{ $name ?? 'Laravel'}}!
        </div>

@endsection
```

The @extends directive tells Blade which layout file to use and @section('content') and @endsection tells Blade what should be inserted when we @yield('content') in the layout.

#### Partials with @include

Within the partials folder let's create a new file named \_head.blade.php and extract the `<head>` element out of app.blade.php into \_head.blade.php

Note: By convention partial templates should begin with an underscore.

The @include directive allows us to directly include a partial view within another view.

Add the following code to app.blade.php where the HTML head should be.

```
@include('layouts.partials._head')
```

Some additional things to know about the @include directive:

- All variables available to the parent view are inherited by @included sub-views.
- We can also make additional data available to sub-views.
  - @include('view.name', ['status' => 'complete'])
- We can conditionally include a sub-view.
  - @includeWhen($boolean, 'view.name', ['status' => 'complete'])
  - @includeUnless($boolean, 'view.name', ['status' => 'complete'])

## Set up a Laravel Portfolio App

Let's start a new Laravel Project for your Portfolio Application

- Open your Ubuntu WSL terminal
- Stop any running sail containers
  - sail stop
- Optionally, you can delete the containers from Docker Desktop
- Initialize a new project
  - curl -s https://laravel.build/jsolomon-showcase | bash
  - Provide your Ubuntu password when prompted
- Launch the app in via Sail (Docker), detached
  - sail up -d
- Verify in browser
  - http://localhost/
- Initialize a local git repo for version control
  - git init
  - add files and make your initial commit
- Create and push to a github remote

## Blade Components

The alternative to using @include and @yield with @extends and @section/@endsection is to use Blade components.

First, let's create a components folder directly under the views folder and within that, create layout.blade.php

Note: Similar to how any file in the views folder is available as a view, anything in views/components is instantly available as a Blade component.

Next, copy over the contents of welcome.blade.php and strip out the body html

```
<html lang="{{ str_replace('_', '-', app()->getLocale())}}">
<head></head>
<body class="antialiased"></body>
</html>
```

Inside the body tag, rather than using @yield, let's echo out a content variable. Insert:

```
{{ $content }}
```

#### Projects Views

For our portfolio application we will be managing and displaying a collection of projects, so let's organize our views accordingly.

Create a folder directly under resources/views/ named projects, and within that create index.blade.php and project.blade.php

To reference a Blade component we use what looks like an HTML tag, but prefixed with x-

In our case we want to use the layout component, so in index.blade.php let's add:

```
<x-layout>

</x-layout>
```

Since our layout component is expecting and echoing out a variable named $content, we need to provide it in one of two ways.

We can either:

- Add it as an attribute in the component tag
  `<x-layout content="Build a Portfolio">`
- Create a slot and set its name attribute
  `<x-slot name="content">Build a Portfolio</x-slot>`

```
<x-layout>
    <x-slot name="content">
        <div class="relative flex items-top justify-center min-h-screen bg-gray-100 dark:bg-gray-900 sm:items-center py-4 sm:pt-0">
            Display a list of projects.
        </div>
    </x-slot>
</x-layout>
```

Also copy the contents of index.blade.php to project.blade.php and change the text.

To test this out, let's create new routes for our project index and detail views.

#### Routing Intro

In routes/web.php add the following:

```php
Route::get('/projects', function () {
    return view('projects.index');
});

Route::get('/projects/project', function () {
    return view('projects.project');
});
```

Navigate to http://localhost/projects and http://localhost/projects/project

Note: Our layout Blade component is what is referred to as an anonymous component, which is to say it is just a view template without an associated class file. Later we'll be building components that are made up of both a class and a template.

We also could have used artisan to make our component.

For our anonymous component the artisan command would have been:

```
sail artisan make:component layout --view
```

## Project Controller, Routes, & Views

Let's start out by creating our ProjectController

In your Ubuntu terminal, run:

```
sail artisan make:controller ProjectController
```

That will create our controller file in app/Http/Controllers/

In ProjectController.php, inside the class definition, add the following methods to return our views.

```php
    public function index()
    {
        return view('projects.index');
    }

    public function show($project)
    {
        return view('projects.project', ['project' => $project]);
    }

```

#### Update our Routes

In routes/web.php let's set up the get routes to call the appropriate controller methods.

First, add the following near the top to make our new controller available:

```php
use App\Http\Controllers\ProjectController;
```

Then, refactor our projects routes, as follows:

```php
Route::get('/projects', [ProjectController::class, 'index']);
Route::get('/projects/{project}', [ProjectController::class, 'show']);
```

Note:

If you have the PHP Namespace Resolver extension installed you can place your cursor within ProjectController, right-click, and choose Import Class. This can be helpful in avoiding typos.

#### Route Parameter Intro

For the individual project view we're using a route parameter.

Eventually we'll be extracting either a slug or an id and loading our data from the database accordingly, but for now just make sure that the show() function in ProductController receives $product and passes it to the view.

To see the value, echo it out in the view with `<h2>Project: {{ $project }}</h2>`

## Dummy Data

In order to take the next steps with our projects index and detail views, let's hard-code some dummy data in our ProjectController.

Inside the index() method, before the return statement, add the following:

```
 $projects = [
            [
                'id' => 0,
                'title' => 'Node.js Yearbook',
                'description' => 'Details coming soon...',
                'is_published' => false
            ],
            [
                'id' => 1,
                'title' => 'React Movie App',
                'description' => '...',
                'is_published' => false
            ],
            [
                'id' => 3,
                'title' => 'Laravel Portfolio Back-End',
                'description' => 'In progress.  Stay tuned.',
                'is_published' => false
            ]
        ];
```

Then, pass the dummy data to the index view:

```php
        return view('projects.index')
            ->with('projects', $projects);
```

While we're here, let's also simulate passing a single todo to our singular todo view.

```php
         $project= [
            'id' => 4,
            'title' => 'Vue.js Portfolio Front-End',
            'description' => '...',
            'is_published' => false
        ];
        return view('projects.project', ['project' => $project]);
```

## Blade Loops and Conditionals

In order to output our projects list in our index view, we're going to need some conditional logic as well as iteration. Fortunately, Blade provides simple directives to handle these.

#### If Statements

We can create if statements using @if, @elseif, @else, and @endif directives.

In index.blade.php add the following, above the div with our placeholder text, but within the x-slot named content:

```php
@if (count($projects))
    <div>{{ count($projects) }} projects to peep.</div>
@else
    <div>Nothing to showcase, yet.</div>
@endif
```

#### Loops

Blade also offers several looping directives.

For our purposes we'll use a @foreach loop.

Replace the placeholder content with:

```php
@foreach ($projects as $project)
    <div>
        <div>
            <a href="/projects/{{ $project['id'] }}">{{ $project['title'] }}</a>
        </div>
        <div>{{ $project['description'] }}</div>
    </div>
@endforeach
```

Note: Since we are currently passing in the projects as an array of arrays we need to use square bracket syntax to access the values by their key.

Once we are have a Project model we'll be working with objects and will be able to access their properties using $project->id.

For more info on Blade directives, see the docs:

https://laravel.com/docs/9.x/blade#blade-directives

## TailwindCSS Setup

Installation

```
sail npm install -D tailwindcss postcss autoprefixer
```

Initialization

```
sail npx tailwindcss init -p
```

#### Set the Template Paths

In tailwind.config.js

```php
/** @type {import('tailwindcss').Config} */
module.exports = {
    content: ["./resources/**.*.blade.php", "./resources/**/*.js"],
    theme: {
        extend: {},
    },
    plugins: [],
};
```

#### Add the Tailwind Directives to CSS

In resources/css/app.css

```
@tailwind base;
@tailwind components;
@tailwind utilities;
```

To fix the "unknown @ rules warning in VSCode.

Open Settings in VSCode (Ctrl + ,) then type "files assoc" in the search bar.

Add an item to associate \*.css with tailwindcss

#### Add the Tailwind CSS Intellisense Extension for VSCode

https://marketplace.visualstudio.com/items?itemName=bradlc.vscode-tailwindcss

user > tailwind CSS: Emmet Completions (check)

#### Load Tailwind from CDN into main layout file

In layout.blade.php, replace the first `<style>...</style> ` element with:

` <script src="https://cdn.tailwindcss.com"></script>`

#### Start Styling with TailwindCSS!

As an initial test, let's add some tailwind classes to the project count div at the top of index.blade.php

```
<div class="text-xl text-orange-700 bg-gray-100">{{ count($projects) }} projects to peep.</div>
```

## Tailwind Styling our Projects

```
<div
    class="relative flex justify-center min-h-screen bg-gray-100 dark:bg-gray-900 sm:items-center py-4 sm:pt-0">
    <div class="mt-6">
        <section class="grid grid-cols-1 md:grid-cols-2 gap-2">
            @foreach ($projects as $project)
            <div class="p-6  bg-white overflow-hidden shadow sm:rounded-lg">
                <div>
                    <a href="/projects/{{ $project['id'] }}">{{ $project['title'] }}</a>
                </div>
                <div>{{ $project['description'] }}</div>
            </div>
            @endforeach
        </section>
        @if (count($projects))
        <div class="text-xs w-full text-right">{{ count($projects) }} projects to peep.</div>
        @else
        <div>Nothing to showcase, yet.</div>
        @endif
    </div>
</div>
```

## Extracting a Project Card Component

Create a projects folder under components and a project-card.blade.php inside that.

Next, cut out the div that we had inside the projects loop in index.blade.php and paste it into the new project-card.blade.php component.

```
<div class="p-6  bg-white overflow-hidden shadow sm:rounded-lg">
    <div>
        <a href="/projects/{{ $project['id'] }}">{{ $project['title'] }}</a>
    </div>
    <div>{{ $project['description'] }}</div>
</div>
```

#### Passing Project Data to our Component

Back in index.blade.php we could include our component from inside the loop with:

```
<x.projects.project-card />
```

But our component will complain that $project is undefined.

If we wanted to pass a simple string to our component we could do so by adding an attribute i.e. <x.projects.project-card title="Hello"/>, but to pass objects or arrays to a component we need to use props.

```
<x-projects.project-card :project="$project" />
```

Within our project-card component we can receive the prop by adding the following at the top of the file.

```
@props(['project'])
```

## Create a Simple Header Nav

In resources/views/components/layout.blade.php

```
<header class="px-6 py-8">
    <nav class="md:flex md:justify-between md:items-center">
        <div><a href="/" class="text-s font-bold uppercase">Home</a></div>
        <div class="mt-8 md:mt-0">
            <a href="/register" class="ml-3 text-xs font-bold uppercase">Register</a>
            <a href="/login" class="ml-3 text-xs font-bold uppercase">Log In</a>
        </div>
    </nav>
</header>
```
