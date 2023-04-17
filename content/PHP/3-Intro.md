+++
title = "Laravel Intro"
weight = 3
pre = "<i class='fas fa-pen'></i> &nbsp"
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
