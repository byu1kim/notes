+++
title = "MySQL Migration"
weight = 3
pre = "- "
+++

## Database Setup and Configuration

Great news!

When we created our project via Sail, Laravel set up and configured our database, running in its own container, ready for us to use.

Laravel stores all sensitive and/or environment specific configuration, including key info for our database, using dotenv.

Open up the .env file in the root of your Laravel project and take a gander.

The default DB_USERNAME and DB_PASSWORD make me cringe, but let's just work with it for now. I've included notes in a separate slide on how to go about changing these... but for now just know that the username is sail and the password is password.

Further details of the database configuration, including which database to use by default, are defined in config/database.php

The default is mysql, which is what we want to work with.

Note: The .env file is excluded from version control in the .gitignore that Laravel set up for us.

## Connecting MySQL Workbench to the Container

If you don't already have it up and running, make sure:

- You have Docker Desktop running
- You have your project containers running too
  - Open your Ubuntu terminal in VSCode
  - Make sure you are in the root of your project
  - Launch your project in detached mode
    - sail up -d

Open MySQL Workbench

- Click the + next to MySQL Connections
- Set an appropriate Connection Name
- Set the Username to sail (per the value found in .env)

Click Test Connection to confirm.

It will prompt you for the password, which (grrrrr) is set to password .

You should get a connection success message with some additional details.

Click OK to save the connection in the MySQL Workbench dashboard.

From here, when you click on the connection it will connect to your MySQL server, running in the container, any time you have your container running.

The next slide should hopefully be a recap of key MySQL commands.

## MySQL Fundamentals

Open the MySQL connection that we made in the last step.

#### Available Databases

We're connected to the MySQL server instance that is running in our Docker container, but are not yet set to work with the database that Laravel set up for our application itself.

In the Query 1 panel, enter the following, then click the lightning bolt icon at the top of the panel to run it:

```
SHOW databases;
```

You should see a results grid displaying 4 rows.

Three of these results are "system" databases:

- information_schema
- performance_schema
- testing

The fourth database should match the name of your application, in snake_case. In my case, that's solomon_showcase.

To specify which database we want to work with and list out all of the tables therein, type and run:

```
USE solomon_showcase;
SHOW tables;
```

#### Running our Initial Migration

```
sail artisan migrate
```

Back in MySQL Workbench, place your cursor on the line with SHOW tables; and then click the lightning bolt icon with a cursor superimposed.

```
SELECT * FROM users;
```

```
DESCRIBE users;
```

## Migrations

In the last step we ran sail artisan migrate and it created the following tables in the database for us:

- migrations
- users
- password_resets
- failed_jobs
- personal_access_tokens

It did so by running the code located in the database/migrations/ directory.

Open the file in that migrations folder that ends with \_create_users_table.php

There, you will see that it defines a new class which extends Migration, and within that class there are two key methods: up() and down().

The up() method is used to add tables, columns, or indexes in our database and the down() method is used to reverse what was done in up().

Deconstructing the \*\_create_users_table migration:

It's calling the create() method of the Schema class with two arguments: the name of the table to be created and a closure in which the columns are defined (via a Blueprint object).

Take a moment to familiarize yourself with (and bookmark) the documentation on creating tables as well as the column types available through migrations with Laravel's Schema Builder:

https://laravel.com/docs/9.x/migrations#creating-tables

https://laravel.com/docs/9.x/migrations#available-column-types

## Creating a Migration for Projects Table

In the Ubuntu WSL terminal, run the following:

```
sail artisan make:migration create_projects_table
```

Based on the migration name provided, Laravel determined that we wanted to create a table with the name projects.

Open up the newly created migration and you'll see:

Let's run that migration to see what it produces.

```
sail artisan migrate
```

$table->id(); created the id column as an auto-incrementing, unsigned big integer, not nullable, column with a primary key index on it.

$table->timestamps(); created two, nullable, timestamp columns for us.

Awesome, but we need more columns to manage our projects data.

## Re-defining our Projects Table

Looking back at the dummy data that we set up in our ProjectController, in addition to the columns above our projects table will need at least the following columns added:

- title
  - Let's assume we want these to be a max of 50 characters and required.
- description
  - This column should support long text
- is_published
  - This should be a boolean that is not nullable and defaults to false

We could set up a migration to alter the existing table, but since we don't have any data in there yet, let's roll back our previous migration, update our column definitions, then run things forward again.

To reverse the effects of our last sail artisan migrate command we can run:

```
sail artisan migrate:rollback
```

That ran the down() method in each of the migrations that were included in the last batch that were executed (just one migration in this case), and our todos table is no more.

To see which migrations were included in the last batch, you can query the migrations table:

```
SELECT * FROM migrations;
```

If you don't want to roll back all of the migrations from the last batch, you can also explicitly specify how many steps to roll back:

```
sail artisan migrate:rollback --step=1
```

## Practice - Edit the Projects Table

Now, I'd like you to try to figure out how to redefine the projects table so that we have the following columns:

- id
- title
- excerpt
- body
- url
- published_date
- created_at
- updated_at

Details:

- The url and published_date fields should be nullable.
- The excerpt and body fields should allow long text.

Get cozy with the docs:
https://laravel.com/docs/9.x/migrations#available-column-types
https://laravel.com/docs/9.x/migrations#column-modifiers

Roll back and migrate as needed.

- sail artisan migrate:rollback
- sail artisan migrate

When you're ready to confirm, the solution is in white text below:

```php
    public function up()
    {
        Schema::create('projects', function (Blueprint $table) {
            $table->id();
            $table->string('title');
            $table->text('excerpt');
            $table->text('body');
            $table->string('url')->nullable(true);
            $table->date('published_date')->nullable(true);
            $table->timestamps();
        });
    }
```

Next, we'll set up our Model.

## Eloquent Models and Tinker

Eloquent is Laravel's official ORM.

Eloquent uses the Active Record Pattern which essentially means that a database table or view is wrapped in a class and each row (record) is handled in code as an object instance and manipulated via getter and setter methods.

Models are stored in the app/Models/ directory.

Out of the box our Laravel app has a User model, so let's explore that a bit before setting up our Project model.

#### Tinkering with Models

Artisan provides a utility for working with models from the command line: tinker.

Since we already have a User model, let's tinker with it to create, update, and delete a user record in our database.

Tinker is a REPL for working with Laravel models.

In your Ubuntu shell, run:

```
sail artisan tinker
```

Checking either the table definition or looking at the User model, you'll see we need to provide a name, email, and password in order to save a user to the database, so our process will be:

- Create a new instance of User
- Set the name, email, and password
- Save

The password is a little bit more involved because we NEVER want to store passwords in clear text. Fortunately, Laravel comes with a package that will handle the encryption for us: bcrypt.

```
$user->save();
= true
```

#### Factories

Factories are essentially a definition of how to instantiate Models, often with fake data via the Faker PHP library.

If you open up the database folder (in the root of your application) you will see a factories folder with a UserFactory.php file inside.

Let's create 3 fake users:

User::factory()->count(3)->create();

#### More Tinkering:

Select a User by id

$user = User::find(2);

Select a User by name

$user = User::where('name','Josh')->first();

Select all Users

$users = User::all();

Select all Users with J in their name

$users = User::where('name', 'LIKE', '%J%')->get();

#### Collections

When the results may contain more than one record from the database, rather than returning a plain PHP array, we get an instance of the Collection class.

The Collection class provides a fluent interface which allows us to chain on additional methods, of which there are many available, to refine and manipulate our results.

**Select all Users, sorted by name, and return just their names**

`$users = User::all()->sortBy('name')->pluck('name');`

https://laravel.com/docs/9.x/collections#available-methods

Enough tinkering, let's build our Project model!

Ctrl-c to exit Tinker.

## Creating a Project Model

We're going to use Artisan again to make our Project model.

```
sail artisan make:model Project
```

Note: Model names should begin with an Uppercase letter and be singular.

```
sail artisan tinker
```

This time we need to be a bit more explicit in instructing tinker where to find our the model we want to work with:

```php
$project = new App\Models\Project;
$project->title = 'Laravel Project Showcase';
$project->excerpt = 'PHP Laravel back-end for portfolio website';
$project->body = 'Homebrew inheritance distributed systems fullstack lazy load protected behavior-driven CSV DAG module. Scalable OOP cache commit elixir minimum viable product configuration test-driven command-line Safari. Gradle ship it senior-engineer branch JSX presenter one-size-fits-all approach compression yarn module. Mutation observer scale Slack i website blog DAG. Emoji circle back Linux dynamic types document object model minification streams.';
$project->url = 'http://localhost';
$project->published_date = date('Y-m-d');
```

Note: If you make a mistake and want to remove a property from your object, you can do so with unset(). For example, if you typo'd excerpt as excert:

```
unset($project->excert);
```

When you're ready, persist your object to the database.

```
$project->save();
```

## Creating a Project Factory

Rather than manually adding in our Project records, let's use Artisan to whip up a ProjectFactory.

sail artisan make:factory ProjectFactory

Look in database/factories/ProjectFactory.php and compare with UserFactory.php

All we need to do is return an associative array of the data that we want generated.

Since only the title, excerpt and body columns are not nullable in our table, that's all we need to return from the definition.

We can use the FakerPHP library to generate the values for us:

```php
    public function definition()
    {
        $bodyArray = fake()->paragraphs(3);
        $body = '<p>' . join('</p></p>', $bodyArray ) . '</p>';
        return [
            'title' => fake()->company() . ' ' . fake()->companySuffix(),
            'excerpt' => fake()->catchPhrase(),
            'body' => $body
        ];
    }
```

Info on available formatters for FakerPHP are here:

https://fakerphp.github.io/formatters/

#### Using the ProjectFactory

Jump back into tinker and generate some fake projects

```
sail artisan tinker

use App\Models\Project;

$projects = Project::factory()->count(4)->create();
```

Verify in the DB.

## Updating Project Controller and Card Component

#### Refactor ProjectController to use the Project Model

Open up your ProjectController

**app/Http/Controllers/ProjectController.php**

Set it to use the Project model.

```
use App\Models\Project;
```

Modify the index() function to now use the Project data from our DB via the Project Model instead of the hard-coded array.

```php
    public function index()
    {
        return view('projects.index')
        ->with('projects', Project::all());
    }
```

Modify the show() method too.  
By specifying the argument being sent to show() as an instance of Project, we can leverage another Laravel feature called Route Model Binding. This will work automatically as long as we receive a valid id in the route parameter and are consistent with the naming.

```php
    public function show(Project $project)
    {
        return view('projects.project',['project' => $project]);
    }
```

#### Refactoring the Project Card Component

Modify the project-card component to use the properties of the Project instances (instead of the array values that it was receiving previously).

**resources/views/components/projects/project-card.blade.php**

```
<div class="p-6  bg-white overflow-hidden shadow sm:rounded-lg">
    <div class="text-xl font-bold">
        <a href="/projects/{{ $project->id }}">{{ $project->title }}</a>
    </div>
    <div>{{ $project->excerpt }}</div>
    <div>{{ $project->body }}</div>
</div>
```

View the projects index at http://localhost/projects

Next, click on any of the project titles (which we wrapped in anchor tags with the id interpolated above). That should take you to an individual project page.

## Conditionally Outputting the Body

It makes sense that on the index page we want to only show the title and excerpt of each Project.

The easiest way to facilitate that will be to add a prop in our project-card Blade component, default its value to false, and then only output the body when it is true.

**project-card.blade.php**

```php
@props(['project', 'showBody' => false])

<div class="p-6  bg-white overflow-hidden shadow sm:rounded-lg">
    <div class="text-xl font-bold">
        <a href="/projects/{{ $project->id }}">{{ $project->title }}</a>
    </div>
    <div>{{ $project->excerpt }}</div>
    @if ($showBody)
        <div>{{ $project->body }}</div>
    @endif
</div>
```

Setting the prop to true in /views/projects/project.blade.php

```
<x-projects.project-card :project="$project" :showBody="true"/>
```

## Displaying Unescaped HTML

When we're using the double curly braces to echo out content in our application Laravel is automatically sending those strings through PHP's htmlspecialchars function to prevent against script injection and XSS attacks.

That is why we see the HTML `<p>` tags in our output instead of having paragraph breaks.

Normally that is very, very good thing - particularly when working with any sort of CMS which allows user input data to be displayed.

In our case, however, since we'll be setting things up in such a way that ONLY trusted users (aka you) can enter data, it is relatively safe to circumvent this protection and display unescaped HTML.

#### The Workaround

Instead of using double curly braces, use single brace double exclamation marks.

Let's allow that for both excerpt and body.

```
{!! $project->excerpt!!}

{!! $project->body !!}
```

## Eloquent Relationships

Ultimately, for our portfolios it would be great if we could "tag" Projects with the various underlying technologies. For example, as a starting point we might tag this Project with:

- OOP
- PHP
- Laravel
- MySQL
- Docker
- TailwindCSS

Since Projects can have multiple Tags and any given Tag may be applied to multiple Projects, that will require implmenting a many-to-many relationship between Projects and Tags.

Of course, many-to-many relationships are (a bit) harder to manage than one-to-many, so let's start out by adding project categories.

#### Categorizing Projects

For now, we're going to set things up so that each Project can be assigned to one Category, though multiple Projects can be assigned to the any given Category.

To accomplish this we will need:

- A categories table
  - Migration
  - Seeder
    - Rather than generating a set of random records with a Factory, we'll populate our categories table with a pre-defined set of data
- A Category Model
- A CategoryController
- Methods to query for the Category that a Project belongs to as well as the projects that belong to a Category

Lucky for us, we can use Artisan got get a head start on almost all of the above!

#### Adding a Category Model

In your Ubuntu terminal, run the following:

```
sail artisan help make:model
```

This lists out the syntax as well as the various options available when using Artisan to make a Model.

We want a model, migration, seeder and controller, so our Artisan command will be:

```
sail artisan make:model Category -c -m -s
```

Note: This might take a few moments - don't panic.

#### Categories Migration

Let's start by editing the Migration.

For our categories table, let's say we want the following:

- id
- slug
  - string
  - unique
- name
  - string
  - unique

Give it a try on your own, then compare with solution, en blanc, below:

```php
        Schema::create('categories', function (Blueprint $table) {
            $table->id();
            $table->string('slug')->unique();
            $table->string('name')->unique();
        });
```

Run the migration and compare your table with the following:

```
sail artisan migrate
```

Of course, our table is not populated (yet).

#### Seeding our Categories Table

Take a look in the database/seeders directory.

Laravel's Seeder class defines a method, run(), which is our opportunity to insert records into our DB.

If you take a look in DatabaseSeeder.php you'll see some example usage inside the run() method.

Open CategorySeeder.php , and at the top import our Category model so we can use it.

```
use App\Models\Category;
```

Then, within the run() method add:

```php
        Category::create([
            'name' => 'Back End',
            'slug' => 'back-end',
        ]);
        Category::create([
            'name' => 'Front End',
            'slug' => 'front-end',
        ]);
        Category::create([
            'name' => 'Full Stack',
            'slug' => 'full-stack',
        ]);
```

#### Run the Seeder

In your Ubuntu terminal, run:

```
sail artisan db:seed --class=CategorySeeder
```

We have an error because we the seeder expects there to be created_at and updated_at columns in our categories table.

Easy fix: We just need to set $timestamps to false in our Category model.

```php
class Category extends Model
{
    use HasFactory;

    public $timestamps = false;
}
```

Run the seeder again.

## Adding a Category to Projects

To add a foreign key relationship between our Projects and Categories we'll need to add a foreignId column pointing at category_id in our projects table.

Open up the \*\_create_projects_table migration and edit the up() method as follows:

```php
    public function up()
    {
        Schema::create('projects', function (Blueprint $table) {
            $table->id();
            $table->string('title');
            $table->text('excerpt');
            $table->text('body');
            $table->string('url')->nullable(true);
            $table->date('published_date')->nullable(true);
            $table->foreignId('category_id');
            $table->timestamps();
        });
    }
```

Next, let's refresh the database from the ground up:

```
sail artisan migrate:fresh
```

Note: You DEFINITELY would NOT want to run the above on production. It drops all of the tables and then runs each of migrations rebuilding things from scratch. We're still early in dev mode though, so it makes more sense to wipe and rebuild.

Take a look at revised description of our projects table.

This is close to what we want, but perhaps we should make that category_id column nullable so that we can still showcase projects even if they don't fall into one of categories.

Edit the schema as follows and then run another migrate:fresh

```
$table->foreignId('category_id')->nullable(true);
```

Progress, but... we've lost all of our projects and categories data!

## Adding a ProjectSeeder

#### Building a Project Seeder

Rather than relying on our Project Factory to generate some random data for us, let's make a Project Seeder.

Artisan has a command for this.

```
sail artisan list

sail artisan help make:seeder

sail artisan make:seeder ProjectSeeder
```

Now, open ProjectSeeder.php and add:

```
use App\Models\Project
```

Let's also define a protected function to wrap our fake paragraphs in <p> tags.

```php
     protected function fakeHTMLParagraphs($count = 3) {
        $bodyArray = fake()->paragraphs($count);
        $body = '<p>' . join('</p></p>', $bodyArray ) . '</p>';
        return $body;
    }
```

Then, within the run() method, create some projects:

```php
public function run()
    {
        Project::create([
            'title' => 'Portfolio Showcase',
            'excerpt' => fake()->sentences(2, true),
            'body' => $this->fakeHTMLParagraphs(4),
            'category_id' => 3
        ]);
        Project::create([
            'title' => 'SSD Yearbook',
            'excerpt' => fake()->sentences(2, true),
            'body' => $this->fakeHTMLParagraphs(),
            'category_id' => 1
        ]);
        Project::create([
            'title' => 'Movie Mania',
            'excerpt' => fake()->sentences(2, true),
            'body' => $this->fakeHTMLParagraphs(5)
        ]);
        Project::create([
            'title' => 'News Site Homepage',
            'excerpt' => fake()->sentences(2, true),
            'body' => $this->fakeHTMLParagraphs(),
            'category_id' => 2
        ]);
        Project::create([
            'title' => 'JavaScript Game',
            'excerpt' => fake()->sentences(2, true),
            'body' => $this->fakeHTMLParagraphs(),
            'category_id' => 2
        ]);
        Project::create([
            'title' => 'iOS App',
            'excerpt' => fake()->sentences(2, true),
            'body' => $this->fakeHTMLParagraphs()
        ]);
        Project::create([
            'title' => 'Android App',
            'excerpt' => fake()->sentences(2, true),
            'body' => $this->fakeHTMLParagraphs()
        ]);
        Project::create([
            'title' => 'Industry Project',
            'excerpt' => fake()->sentences(2, true),
            'body' => $this->fakeHTMLParagraphs(6),
            'category_id' => 3
        ]);
    }
```

Now, run the CategorySeeder and then the ProjectSeeder:

```
sail artisan db:seed --class=CategorySeeder
sail artisan db:seed --class=ProjectSeeder
```

Verify, both in the DB and browser.

#### Running Seeders when we Migrate

Open up the DatabaseSeeder class then replace it's run() method with the following:

```php
  public function run()
    {
        // populate categories and projects table with seed data
        $this->call([
            CategorySeeder::class,
            ProjectSeeder::class
        ]);
    }
```

From here forward, any time we want to completely rebuild and repopulate our database tables we can do so in one easy artisan command:

```
sail artisan migrate:fresh --seed
```

Note: We don't need to import CategorySeeder or ProjectSeeder, but the order does matter.

## Eloquently Outputting Categories

#### Displaying the Category Name

We've got a category_id column in our projects table, but we still have some work to do if we want to show what category a project belongs to.

To handle this, let's add a getter method in our Project model.

Open app/Models/Project.php and insert the following inside the Project class definition:

```php
    public function category()
    {
        return $this->belongsTo(Category::class);
    }
```

And, finally, let's add some conditional output in our project-card component:

```
    <footer>
        @if ($project->category)
            <span>Category: {{ $project->category->name }}</span>
        @endif
    </footer>
```

## Listing Projects by Category

We can output which Category a Project belongs to, but we don't yet have a way to list Projects by Category.

Let's start by adding a method in our Category model for finding any Projects that have this Category:

```php
    public function projects()
    {
        return $this->hasMany(Project::class);
    }
```

Then, in ProjectController let's add method that receives a Category and returns the index view having passed in the results from above as the projects variable.

Import the Category Model:

```php
use App\Models\Category;
```

Add a listByCategory Method:

```php
    public function listByCategory(Category $category)
    {
        return view('projects.index')
        ->with('projects', $category->projects);
    }
```

Finally, let's add a Route with a category parameter that calls the appropriate method in our ProjectController

```php
Route::get('/categories/{category}', [ProjectController::class, 'listByCategory']);
```

Navigate to http://localhost/categories/2

## Category Slugs

#### Using Slugs

Currently we are using Route Model Binding via category->id to load our categories, but it would be better UX to use slugs and have links directly to the category list page.
This is relatively straightforward:

Update the Route parameter to use slug for the model binding

```php
Route::get('/categories/{category:slug}', [ProjectController::class, 'listByCategory']);
```

Note: This sort of Route-Model binding works with any unique field in the model.

Wrap the category name output slug-based links

```
<a href="/categories/{{ $project->category->slug }}">...</a>
```
