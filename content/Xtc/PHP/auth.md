+++
title = "Auth"
weight = 5
pre = "- "
+++

## User Registration Form

Let's start by taking a look at the User Model and Table to get a sense of what our form will need to handle.

From the database we can see that name, email, and password columns are not nullable.

From the model we can see that these fields are also mass assignable aka fillable.

#### Form Setup

To build out our registration functionality we're going to need:

- A Controller with a create method
  - RegisterUserController
  - create method
- A view to display the create form
  - resources/views/register_user/create.blade.php
- A route to the view via the controller

#### Set up the Controller

In WSL terminal, run:

```
sail artisan make:controller RegisterUserController
```

Add the create action to the controller.

```php
class RegisterUserController extends Controller
{
    public function create() {
        return view('register_user.create');
    }
}
```

#### Set up the Form in a View

Create resources/views/register_user/create.blade.php

```
<x-layout>
  <x-slot name="content">
    <main class="max-w-lg mx-auto">
      <h1 class="text-center font-bold text-xl mb-3">Register User</h1>
      <form method="POST" action="/register">
        <div class="mb-6">
          <label for="name" class="block mb-2 uppercase font-bold text-xs text-gray-700">Name</label>
          <input type="text" name="name" id="name" required class="border border-gray-400 rounded p2 w-full">
        </div>
        <div class="mb-6">
          <label for="email" class="block mb-2 uppercase font-bold text-xs text-gray-700">Email</label>
          <input type="text" name="email" id="email" required class="border border-gray-400 rounded p2 w-full">
        </div>
        <div class="mb-6">
          <label for="password" class="block mb-2 uppercase font-bold text-xs text-gray-700">Password</label>
          <input type="text" name="password" id="password" required
            class="border border-gray-400 rounded p2 w-full">
        </div>
        <div class="mb-6">
          <label for="confirm-password" class="block mb-2 uppercase font-bold text-xs text-gray-700">Confirm
            Password</label>
          <input type="text" name="confirm-password" id="confirm-password" required
            class="border border-gray-400 rounded p2 w-full">
        </div>
        <div class="mb-6">
          <button type="submit" class="bg-green-700 text-white rounded py-2 px-4 hover:bg-green-600">Submit</button>
        </div>
      </form>
    </main>
  </x-slot>
</x-layout>
```

#### Set up a Route to the Registration Form via the Controller.

In routes/web.php

```php
Route::get('/register', [RegisterUserController::class, 'create']);
```

And import RegisterUserController:

```php
use App\Http\Controllers\RegisterUserController;
```

http://localhost/register

## Registration Form Post Handling

Add a Method to the Controller with some Request Validation

```php
    public function store() {
        request()->validate([
            'name' => 'required',
            'email' => ['required','email'],
            'password' => ['required','min:8'],
        ]);
    }
```

Add a Route

```php
Route::post('/register', [RegisterUserController::class, 'store']);
```

error..

Laravel has built in Cross-Site-Request-Forgery protection.

Even though Docker is set up to map port 80 on the host machine to port 80 in the container, we are technically working cross domain here.

Let's configure things so we can post back to Laravel from the browser.

## CSRF Handling

Before Laravel will actually accept data posted from our form we need to add a directive to our form to protect against CSRF attacks.

Thankfully, Laravel makes this ridiculously easy.

Inside the form in create.blade.php add the following:

```
@csrf
```

When Laravel renders the form it will replace that directive with a hidden input containing a token.

When the form is posted, Laravel will validate the token to ensure that it is valid before any processing takes place.

## When Request Validation Fails

Any time our request validation fails Laravel automatically redirects back to the same page that the request came from.

It also populates an error variable containing the name of the validation rule that failed as well as a message variable which we can use to output the details of that error.

Laravel provides an @error directive with we can use to check for errors against a specified rule.

Let's update our registration create form template to give useful information back to the user if their password is invalid (we specified a minimum length of 8 characters).

Just beneath the password input, add:

```
@error('password')
<p class="text-red-500 text-xs mt-1">{{ $message }}</p>
@enderror
```

Test it out with a password less than 8 characters long.

We get a useful message back, but all of the fields have been emptied out. That's a UX foul, to be sure.

Hold off on adding in @error directives for the other inputs. We have much left to cover and @error handling will be part of today's assignment.

## Retrieving and Displaying Old Values

Once again, Laravel makes this quite easy to handle.

There is a helper method, old(), that we can use to retrieve old values by name.

Since these will be empty unless we were redirected back to the form due to a validation error we can use them as the value attributes for our form inputs.

That looks like:

```
<input type="text" name="name" id="name" value="{{ old('name') }}" ...
```

Go ahead and add these in for both the name and email inputs.

Note: It's best practice NOT to repopulate password fields, so leave those without a value attribute to ensure they are empty each time the form is displayed.

## When Database Constraints Fail

If we try submitting the registration form with an email address that already exists in the database we are in for an ugly surprise... in the form of an unhandled MySQL integrity constraint violation.

In our Controller we're validating that the email is required, but we also need to handle the uniqueness constraint BEFORE it hits the database and barfs at the user.

Laravel FTW.

All we need to do is add another validation rule in the form

unique:`<table_name>,<column_name>`

```php
public function store() {
        $attributes = request()->validate([
            'name' => 'required',
            'email' => ['required','email', 'unique:users,email'],
            'password' => ['required','min:8'],
        ]);
   ...
}
```

Since our users table is currently empty, let's tinker in a record and then test the validation rule.

Do you remember how to tinker?

Details below, in white...

```php
sail artisan tinker
$user = new User;
$user->name = "Josh Solomon";
$user->email = "jsolomon11@bcit.ca";
$user->password = bcrypt("CantGuessThis!DoDoDoDo");
$user->save();
```

## Password Confirmation Checking

Before we create our new user we should validate that the user entered the same password in both password fields in the registration form.

Thankfully, Laravel offers a validation rule to accomplish just that.

The only requirement is that the field we want to match against has the same name as the original, but with \_confirmation appended.

Previously (and intentionally), I set the name of the second password field to
"confirm-password".

Replace that (and the id and label for='' ) with "password_confirmation".

While we're in here, let's also change the input type of both fields to password.

**register_user/create.blade.php**

```
<label for="password_confirmation" class="block mb-2 uppercase font-bold text-xs text-gray-700">Confirm
Password</label>
<input type="password" name="password_confirmation" id="password_confirmation" required
class="border border-gray-400 rounded p2 w-full">
```

**RegisterUserController.php**

```php
$attributes = request()->validate([
    'name' => 'required',
    'email' => ['required','email', 'unique:users,email'],
    'password' => ['required','min:8','confirmed'],
]);
```

## Creating the User

Now that we're past both the client side and request validation - all we need to do is create a new User, from our User model, passing in the posted form attributes.

In RegisterUserController.php

Import the Model

```php
use App\Models\User;
```

Then, within the store() method:

- Capture the attributes in a variable
- Pass them to the create method of the User model
- Redirect to the home page (or where desired)

```php
    public function store() {
        $attributes = request()->validate([
            'name' => 'required',
            'email' => ['required','email','unique:users,email'],
            'password' => ['required','min:8','confirmed'],
        ]);

        User::create($attributes);

        return redirect('/');
    }
```

Success, but... the password is in clear text (not to mention a bad password)!

## Eloquent Mutators

We need a way to encrypt the password before saving it to the database.

If you recall waaaaay back on Monday when we tinkered with Users, we can use the bcrypt library to accomplish this.

We could handle this after validation but before creating our User, but there's a more eloquent way to handle this: Eloquent Mutators.

In Models/User.php let's add a mutator such that any time we are going to set the Password attribute of our User it will be run through bcrypt first.

```php
    protected function setPasswordAttribute($password)
    {
        $this->attributes['password'] = bcrypt($password);
    }
```

Though we could use a more verbose/explicit syntax, I'm using a shorthand by naming my mutator function using the pattern` set<AttributeName>Attribute.`

The same pattern works accessor functions: `get<AttributeName>Attribute. `

## Login

When we were validating the registration form data via the request we were indirectly working with Laravel Facades via Inversion-of-Control helpers.

Facades are classes that act as static proxies to underlying classes.

For example, when we ran request()->validate(), we were working with:

Illuminate\Http\Request;

Without going on too much of a tangent, I encourage you to get familiar with the Facades that Laravel ships with because, in many ways, they are part of what makes Laravel so easy to work with.

https://laravel.com/api/9.x/Illuminate/Http/Request.html

https://laravel.com/docs/9.x/facades#facade-class-reference

#### Auth

In order to log users in and out as well as control what resources they can access, we're going to work with the Auth facade via the auth() helper.

https://laravel.com/api/9.x/Illuminate/Auth.html

To automatically log in a new user after registration:

```php
$user = User::create($attributes);

auth()->login($user);
```

## Login Form

To set up our login form we're going to:

- Create and use a SessionController
- Add get and post routes to /login
- Build a login form view
- Authenticate the request data against the database

#### Make the SessionController

```
sail artisan make:controller SessionController
```

#### Create the login form view

**/resources/views/sessions/create.blade.php**

Pillage create.blade.php from register_user, but now we just need email and password.

Remember to change the form action to "/login" too.

#### Handle Login Form get requests

**SessionController.php**

```php
    public function create() {
        return view('sessions.create');
    }
```

#### Handle the Login Request

```php
public function store() {
    // validate the form data
    $attributes = request()->validate([
        'email' => ['required', 'exists:users,email'],
        'password' => ['required'],
    ]);

    // attempt login via the auth helper
    if( auth()->attempt($attributes) ) {
        return redirect('/');
    }

    // handle auth attempt failure
    return back() // send the user back to the form
    ->withInput()  // send the data back to the form too
    ->withErrors(['email' => 'Email or Password is invalid.']); // send error and message

}
```

#### Add Routes

Import the SessionController

```php
use App\Http\Controllers\SessionController;
```

Get requests will show the form, post will store the user auth in session.

```php
Route::get('/login', [SessionController::class, 'create']);
Route::post('/login', [SessionController::class, 'store']);
```

## Flash Messages

We've got our registration and login handling working now, but we aren't yet giving the user any feedback unless there is an error.

In order to improve the UX, let's add in a mechanism that will provide a temporary message to the user.

Laravel supports this through the session() helper with a flash() method that will persist a key/value pair for a single page load.

Let's start by adding a flash message when a login attempt is successful.

**SessionController.php**

```php
        // attempt login via the auth helper
        if( auth()->attempt($attributes) ) {
             // Set a flash message
             $message = 'Welcome back, ' . auth()->user()->name . '!';
             session()->flash('success',$message);
             // Redirect to home page
            return redirect('/');
        }
```

Now, lets add an area to our layout template that will conditionally display our flash messages.

**layout.blade.php**

Just below the nav element in our header, add:

```php
@if (session()->has('success'))
    <div class="md:flex md:justify-center md:items-center">
        <p class="text-xs font-bold uppercase border border-green-700 rounded px-4 py-2">
            {{ session()->get('success')}}
        </p>
    </div>
@endif
```

## Conditional Output by @auth

Rather than just showing a flash message when a user is logged in, let's conditionally change the links in the header.

I've moved the Projects and About links over to the left next to HOME.

To accomplish the conditional output we can use the @auth directive.

If the user is logged we'll output their name and a Logout link, otherwise we'll output a Login link.

Outputting the name can be accomplished via the auth() and user() helpers.

**layout.blade.php**

```
<div class="mt-4 md:mt-0">
            @auth
                <span class="text-xs font-bold uppercase"> {{ auth()->user()->name }} </span>
                <a href="/logout" class="ml-3 text-xs font-bold uppercase">Logout</a>
            @else
                <a href="/login" class="ml-3 text-xs font-bold uppercase">Log In</a>
            @endauth
        </div>
```

## Logout

To log the user out, all we need to do is:

- Set up a get route to /logout
- Add a destroy method to our SessionController
  - Call auth()->logout()
  - Redirect

routes/web.php

```php
Route::get('/logout', [SessionController::class, 'destroy']);
```

app/Http/Controllers/SessionController.php

```php
    public function destroy() {
        // Log them out
        auth()->logout();
        // Redirect to Home
        return redirect('/');
    }
```

## Guest and Auth Middleware

Although we're conditionally showing/hiding links depending on whether a user is authenticated or not, this is not implementing any control over who can access which routes. To actually protect routes we'll need to use middleware.

Recall from our lesson on Express.js, middleware are just sets of things that happen between a request being received and a response being sent.

In Laravel we get a set of middleware out-of-the-box.

You'll find them defined in the files under app/Http/Middleware as well as in app/Kernel.php

Towards the bottom of app/Kernel.php you will see a protected property called $routeMiddleware.

Let's use a couple of those to control access to specific routes.

#### Guest Only Routes

Login really only make sense to be used by unauthenticated users - aka guests.

In web.php, let's enforce that via the 'guest' middleware with:

```php
Route::get('/login', [SessionController::class, 'create'])->middleware('guest');
Route::post('/login', [SessionController::class, 'store'])->middleware('guest');
```

#### Authenticated Users Only Route

Let's make the logout route only accessible to authenticated users.

```php
Route::get('/logout', [SessionController::class, 'destroy'])->middleware('auth');
```

If you test these out, either as a guest trying to log out, or as an already authenticated user trying to log in you will either get an error or a 404 page respectively.

Let's deal with former first.

#### A Guest Trying to Access Auth Only Routes

When a guest tries to access a route that is protected with middleware('auth'), such as the logout page, you'll see the following error:

This is because the auth middleware is attempting to redirect the user to a route named "login". This is defined in app/Http/Middleware/Authenticate.php

Although we do have a route defined for the path /login, we need to map that to the name login in order for this redirect to work.

Note: An alternative would be edit the Authenticate middleware itself, but best not to meddle with pre-existing middleware unless you have a thorough sense of where else it may be in use or what else might be relying on it.

#### Naming a Route

Open up routes/web.php and edit as follows:

```php
Route::get('/login', [SessionController::class, 'create'])->name('login')->middleware('guest');
```

Note: We need to name() our route before middleware('guest').

Make sure that you are not logged in and then try to access http://localhost/logout

#### An Authenticated User Trying to Access a Guest Only Route

When an authenticated user attempts to access a guest route we don't get an error, but instead they are automatically redirected to the URL /home, which results in a 404 unless we have a get route defined at that URL.

We could handle this by adding a /home route, but instead let's add a generic fallback route that redirects any invalid requests to / and adds a flash message.

#### Fallback Routes

Beneath all other routes in web.php, add:

```php
Route::fallback(function() {

    // Set a flash message
    session()->flash('error','Requested page not found.  Home you go.');

    // Redirect to /
    return redirect('/');
});
```

#### Displaying Session Error Messages

We already outputting session success messages between the header and content in our layout, so that's the obvious place to add in an @elseif

```
   @if (session()->has('success'))
        <div class="flex justify-center items-center bg-gray-100 w-full py-3">
            <p class="text-xs font-bold bg-white uppercase border border-green-700 rounded px-4 py-2">
                {{ session()->get('success') }}
            </p>
        </div>
    @elseif (session()->has('error'))
        <div class="flex justify-center items-center bg-gray-100 w-full py-3">
            <p class="text-xs color-red-500 font-bold bg-white uppercase border border-red-700 rounded px-4 py-2">
                {{ session()->get('error') }}
            </p>
        </div>
    @endif
```

## Admin Only via Custom Middleware

There are numerous ways that we could handle role-based authorization, but given the nature of what we're building let's start with the very simplest: Custom middleware that only allows specific users to proceed, based solely on their email address.

#### Making Middleware

In your Ubuntu terminal, run:

```
sail artisan make:middleware AdminOnly
```

#### Restricting Access to Admins by Email Address

Open app/Http/Middleware/AdminOnly.php

At the top, import the Response class with:

```php
use Illuminate\Http\Response;
```

Next, replace the body of the handle method with:

```php
        if(auth()->user()?->email !== 'jsolomon11@bcit.ca') {
            abort(Response::HTTP_FORBIDDEN);
        }

        return $next($request);
```

#### Registering our Middleware

Open app/Kernel.php (one level up from the Middleware directory).

Towards the bottom you will find where $routeMiddleware is defined as an associative array. These, of course, are the middleware that can be applied to specific routes.

Let's add our to the top (keeping things alphabetical):

```php
'admin' => \App\Http\Middleware\AdminOnly::class,
```

#### Applying our middleware to a Route

For now, let's just create an admin route and apply our middleware to it.

In routes/web.php, add:

```php
Route::get('/admin/projects', [ProjectController::class, 'index'])->middleware('admin');
```

If the authenticated user is an admin, they'll be able to see the project index at http://localhost/admin/projects

Anyone else, including guests, should get a 403 "forbidden" response.

## Simple Roles

We've locked down certain routes via middleware, but it would also be useful to have a method in our user model that we can call in order to conditionally output elements in a page.

Add the following in app/Models/User.php

```php
    public function isAdmin() {
        if ($this->email === 'jsolomon11@bcit.ca') {
            return true;
        } else {
            return false;
        }
    }
```

#### Add an admin only link between the user name and logout

resources/views/components/layout.blade.php

```php
       @if (auth()->user()->isAdmin())
          <a href="/admin/projects" class="ml-4 text-s font-bold uppercase">Admin</a>
        @endif
```

Update the AdminOnly middleware

```php
use App\Models\User;

...
public function handle(Request $request, Closure $next)
{
    $user = $request->user();
    if(!$user?->isAdmin()) {
        abort(Response::HTTP_FORBIDDEN);
    }

    return $next($request);
}
```

## Middleware Groups

If we have a set of routes that we want to apply the same middleware to, we can do so more cleanly through the use of middleware groups.

#### Applying Middleware to Route Groups

In web.php remove the route that we set up in the last step and replace it with:

```php
Route::middleware(['auth', 'admin'])->group(function () {
    Route::get('/admin/projects', [ProjectController::class, 'index']);
    Route::get('/admin/projects/{project}', [ProjectController::class, 'show']);
});
```

Try navigating to :

http://localhost/admin/projects

Assuming you have a project with the slug 'portfolio-showcase', also try:

http://localhost/admin/projects/portfolio-showcase

Note: clicking the link from the admin version of the index take you out of the admin area... That's expected. We won't be setting up an admin index shortly.

## Public Storage

Next week we'll be adding in the ability to upload a thumbnail and image for each project.

In preparation for that, let's make an images directory available and drop in some placeholders so that we can start redesigning our project card.

#### Creating a Symlink to Public Storage Directory

```
sail artisan storage:link
```

Within that folder, create an images folder and add in some placeholder and thumbnail images.

#### Accessing Files in Public Storage

In order to access the files in public you will need to use the url() helper.

The path should begin with storage, but not include "app/public". That part is handled by the symlink we created above.

```php
src="{{url('storage/images/placeholder_thumb.svg')}}"
```
