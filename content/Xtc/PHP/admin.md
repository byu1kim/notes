+++
title = "Admin"
weight = 6
pre = "- "
+++

## Create Project Form

Now that we have a secured admin area, let's build our project creation UI.

#### Add Routes

routes/web.php

```php
    Route::get('/admin/projects/create', [ProjectController::class, 'create']);
    Route::post('/admin/projects/create', [ProjectController::class, 'store']);
```

#### Add A Create Method to ProjectController

Since we will need a category dropdown in our form, make sure to import the Category model and send all categories to the view.

```php
    public function create() {
        return view('admin.projects.create')
        ->with('categories', Category::all());
    }
```

#### Build the Projects Create View

resources/views/admin/projects/create.blade.php

Use the same x-layout and x-slot that we've used for our other views.

#### Construct the Form

Remember to add the @csrf directive

#### Add the Inputs

Refer to the create_projects_table migration for a reference to the needed inputs.

```php
$table->string('title');
$table->text('excerpt');
$table->text('body');
$table->string('url')->nullable(true);
$table->date('published_date')->nullable(true);
$table->foreignId('category_id')->nullable(true);
```

Add inputs and labels accordingly, including conditional display of errors and display of old values, i.e.:

```
<div class="mb-6">
    <label for="title" class="block mb-2 uppercase font-bold text-xs text-gray-700">Title</label>
    <input type="text" name="title" id="title" value="{{ old('title') }}" required
    class="border border-gray-400 rounded p2 w-full">
    @error('title')
    <p class="text-red-500 text-xs mt-1">{{ $message }}</p>
    @enderror
</div>
```

#### Adding the Category Dropdown

For the category_id field we'll want to iterate through the available categories and create options for each as well as offer an option for None

```
<select name="category_id" id="category_id">
    <option value="" selected disabled>Select a Category</option>
    <option value="">None</option>
    @foreach ($categories as $category)
        <option value="{{ $category->id }}" @if ($category->id == old('category_id')) selected @endif>
            {{ $category->name }}</option>
    @endforeach
</select>
```

## Store the Project

#### Handle Form Request Data

Create the store method in ProjectController

Let's start by dumping out the submitted data with:

```php
    public function store() {
        ddd(request()->all());
    }
```

Note: ddd is Die, Dump, Debug.

#### Validate the Request

```php
$attributes = request()->validate([
    'title' => 'required',
    'excerpt' => 'required',
    'body' => 'required',
    'url' => ['nullable','sometimes','url'],
    'published_date' => ['nullable','sometimes','date'],
    'category_id' => ['nullable','sometimes','exists:categories,id'],
]);
```

#### If We Pass Validation, Generate the Slug

Import the Str class at the top of the file so we can use its slug method.

```php
use Illuminate\Support\Str;
```

Then after the validation:

```php
        // Generate the slug from the title
        $attributes['slug'] = Str::slug($attributes['title']);
```

#### Create the Project

```php
Project::create($attributes);
```

#### Mass Assignment

Laravel Models have a set of properties which control which fields should, or should not, be eligible for mass assignment. In our case, we want title, excerpt, body, url, pubished_date, and category_id to be fillable, so, in our Project model we need to add:

```php
    /**
     * The attributes that are mass assignable.
     *
     * @var array<int, string>
     */
    protected $fillable = [
        'title',
        'slug',
        'excerpt',
        'body',
        'url',
        'published_date',
        'category_id'
    ];
```

#### Redirect to the Dashboard

```php
// Set a flash message
session()->flash('success','Project Created Successfully');

// Redirect to the Admin Dashboard
return redirect('/admin');
```

## Editing Projects

There are very few differences between creation and editing of records, particularly when it comes to the form, so let's see if we can reuse as much as possible.

The first difference is that we'll need to send the original project data to the form view from our controller.

**ProjectController.php**

```php
    public function edit(Project $project) {
        return view('admin.projects.create')
        ->with('project', $project)
        ->with('categories', Category::all());
    }
```

#### Updating the Form

**create.blade.php**

Next, in our form we'll need to branch the form action.

Let's also change the request type to "PATCH" and clearly display on screen whether this is a create or edit.

Note: HTML form elements only allow GET or POST, but we can use Laravel @method directives to override this. When updating part or all of a resource the appropriate method is PATCH.

```
@if ($project)
<h1 class="text-center font-bold text-xl mb-3">Edit Project: {{ $project->title }}</h1>
<form method="POST" action="/admin/projects/{{ $project->id }}/edit" enctype="multipart/form-data">
    @method('PATCH')
@else
    <h1 class="text-center font-bold text-xl mb-3">Create Project</h1>
    <form method="POST" action="/admin/projects/create" enctype="multipart/form-data">
@endif
```

#### Handling Both Old and Original Values

If validation fails we still want to re-display the last user-entered value, but when initially editing a project we want to display the original value.

We can accomplish that with the nullish coalescing operator (??) to show either the old value or the original value, but also have to remember that if we are creating a new project there is no original project from which to echo out a value.

All together, that will look like:

`<input type="text" name="title" id="title" value="{{ old('title') ?? $project?->title }}" />`

You'll need to do that for each of the inputs, including determining which option in the category dropdown is selected.

#### Edit Routes

```php
Route::get('/admin/projects/{project}/edit', [ProjectController::class, 'edit']);
Route::patch('/admin/projects/{project}/edit', [ProjectController::class, 'update']);
```

#### Validation

Most of our validation can remain the same, with one key exception - the uniqueness of the title (and indirectly, the slug).

Lucky for us, the unique validator takes a this argument which is the id to ignore.

    public function update(Project $project, Request $request) {

```php
$attributes = request()->validate([
    'title' => ['required','unique:projects,title,'.$project->id],
    'excerpt' => 'required',
    'body' => 'required',
    'url' => ['nullable','sometimes','url'],
    'published_date' => ['nullable','sometimes','date'],
    'category_id' => ['nullable','sometimes','exists:categories,id'],
]);
```

After passing validation we call the update() method on the project instance.

```php
// Save updates to the DB
$project->update($attributes);
```

## Deleting Projects

Last stop on our CRUD tour is deletion.

Both the routing and controller method are quite straightforward.

**routes/Web.php**

```php
Route::delete('/admin/projects/{project}/delete', [ProjectController::class, 'destroy']);
```

**ProjectController.php**

```php
    public function destroy(Project $project) {
        $project->delete();

        // Set a flash message
        session()->flash('success','Project Deleted Successfully');

        // Redirect to the Admin Dashboard
        return redirect('/admin');
    }
```

The only thing that may be slightly different than expected is that we want to use a form around the delete button in the in the dashboard so that we can set the HTTP method to delete.

**views/admin/index.blade.php**

```
            <form method="POST" action="/admin/projects/{{$project->id}}/delete" class="inline">
                @csrf
                @method('delete')
                <button type="submit" class="text-red-600">Delete
                </button>
            </form>

```

## Adding Project Thumbnails

Let's add support for each project to, optionally, have a featured image and a thumbnail.

#### Migration

First we'll need a add a couple of columns our projects table.

Rather than redefining the create_projects_table migration, let write a migration that just alters the table definition.

```
sail artisan make:migration add_image_and_thumb_to_projects_table --table=projects
```

Then define the up and down methods in the migration

```php
    public function up()
    {
        Schema::table('projects', function (Blueprint $table) {
            $table->string('image')->nullable()->default(null)->after('published_date');
            $table->string('thumb')->nullable()->default(null)->after('image');
        });
    }
    public function down()
    {
        Schema::table('projects', function (Blueprint $table) {
            $table->dropColumn('image');
            $table->dropColumn('thumb');
        });
    }
```

Run the migration

```
sail artisan migrate
```

#### Update the Model

Add the new columns as fillable fields in the Project model

```php
    protected $fillable = [
        'title',
        'excerpt',
        'body',
        'url',
        'published_date',
        'image',
        'thumb',
        'category_id'
    ];

```

#### Update the Form

In order to send files along with the form data we need to set the enctype for the create/edit form to "multipart/form-data".

Add File Inputs

```php
<div class="mb-6">
<label for="thumb" class="block mb-2 uppercase font-bold text-xs text-gray-700">Thumbnail</label>
<input type="file" name="thumb" id="thumb"
    value='{{ old('thumb') ?? $project?->thumb }}'
    class="border border-gray-400 rounded p2 w-full" / >
@error('thumb')
    <p class="text-red-500 text-xs mt-1">{{ $message }}</p>
@enderror
</div>
<div class="mb-6">
<label for="image" class="block mb-2 uppercase font-bold text-xs text-gray-700">Image</label>
<input type="file" name="image" id="image"
    value="{{ old('image') ?? $project?->image }}"
    class="border border-gray-400 rounded p2 w-full">
@error('image')
    <p class="text-red-500 text-xs mt-1">{{ $message }}</p>
@enderror
</div>
```

#### Handle file uploads in ProjectController

Add validation rules to both the store and update methods :

```php
            'image' => ['nullable','sometimes','image','mimes:jpg,png,jpeg,gif,svg','max:2048','dimensions:max_width=1200'],
            'thumb' => ['nullable','sometimes','image','mimes:jpg,png,jpeg,gif,svg','max:1024','dimensions:max_width=600'],
```

Save the Upload Files into Public Storage

```php
// Save upload in public storage and set path attributes
$image_path = $request->file('image')->storeAs('images',$request->image->getClientOriginalName(), 'public');
$attributes['image'] = $image_path;
$thumb_path = $request->file('thumb')->storeAs('images', $request->thumb->getClientOriginalName(), 'public');
$attributes['thumb'] = $thumb_path;
```

## Pagination

Paginating records can be the stuff of nightmares, but with Laravel we can implement it for our index page by editing just 2 lines of code.

Instead of

**ProjectController.php**

```php
    public function index()
    {
        return view('projects.index')
            ->with('projects', Project::latest('published_date')->paginate(6)->withQueryString())
            ->with('categoryName', null);
    }

                @if (count($projects))
                    <div class="text-xs mt-4 w-full text-right">{{ $projects->links() }}</div>
                @else
                    <div>Nothing to showcase, yet.</div>
                @endif
```

## Additional Pagination Notes

As most of you encountered when adding pagination to the /projects/index.blade.php, Laravel throws an error when rendering the category specific list of projects because $projects->links() is undefined.

The "easy" solution that I presented in class was to create a duplicate that index.blade.php and have a non-paginated version. While that does work and is easy, it means we've added a significant amount of repetition in our codebase.

A did a little further digging this evening and found a better solution:

```php
  @if (count($projects))
      <div class="text-xs mt-4 w-full text-right">
          @if($projects instanceof \Illuminate\Pagination\AbstractPaginator)
              {{ $projects->links() }}
          @else
              Found {{ count($projects) }} Projects in {{ $categoryName }}
          @endif
      </div>
  @else
      <div>Nothing to showcase, yet.</div>
  @endif
```

Long story moderately short - a solid strategy when something is throwing errors is to find and implement a (reasonable) quick fix first, jot down a "there must be a better solution to this..." note somewhere, and then focus on the other things that still need to be completed. Our minds are remarkably good at divergent thinking when fed puzzles to work on in the background - especially when there's no "it's broken" anxiety attached to the task.

## Many to Many Relationships

To implement our one-to-many relationship between categories and projects we:

- Created a categories table
- Added a category_id column to our projects table with a foreign key constraint
- In our Project model we added a category() method that used Eloquent's belongsTo(Category::class) to retrieve the category for a project
- In our Category model we added a projects() method that called hasMany(Project::class)

Many-to-many relationships are a tiny bit more complicated, but Eloquent still makes them relatively easy to work with.

What we'll need to set up in order to support many-to-many relationships between our projects and tags are:

- A tags table
- A projects_tags pivot table
  - this will have projects_id and tags_id columns and there will be one record for each association between project and tag
- In each model (Project and Tag) we will then add a belongsToMany() method

## Project Tags

#### Create the Tags Model and Migration

```
sail artisan make:model Tag -m
```

In the migration:

Add unique string columns for name and slug.

Also, to follow convention make sure the table name is pluralized: tags.

If removing the timestamps() from the migration, make sure to set the following in the model too:

```
public $timestamps = false;
```

#### Create the Pivot Table Migration

To create our pivot table we still use artisan's make:migration with the create\_\*\_table syntax, but it's important that we list out the connected tables in alphabetical order.

```
sail artisan make:migration create_projects_tags_table
```

For this table we'll need an id() as well as the foreign key columns pointing at the id column in both the projects and tags tables and we'll also want the FK constraints to the cascade on delete.

```php
    public function up()
    {
        Schema::create('projects_tags', function (Blueprint $table) {
            $table->id();

            $table->unsignedBiginteger('projects_id')->unsigned();
            $table->unsignedBiginteger('tags_id')->unsigned();

            $table->foreign('projects_id')->references('id')
                    ->on('projects')->onDelete('cascade');
            $table->foreign('tags_id')->references('id')
                    ->on('tags')->onDelete('cascade');
        });
    }
```

#### Update the Models

**Project.php**

```php
    // Load the Tags for this Project
    public function tags()
    {
        return $this->belongsToMany(Tag::class, 'projects_tags', 'projects_id', 'tags_id');
    }
```

**Tag.php**

```php
    public function projects()
    {
        return $this->belongsToMany(Project::class, 'projects_tags', 'tags_id','projects_id');
    }
```

Be aware that the order of the 'tags_id' and 'projects_id' argument is important here and reverses depending on whether we want the tags related to a project or the projects related to a tag.

Most of the time you won't need to dig into the API documentation directly, but when there are multiple arguments involved and you're not getting expected results at first... dive into the details:

https://laravel.com/api/9.x/Illuminate/Database/Eloquent/Concerns/HasRelationships.html#method_belongsToMany

#### Run the Migrations and Verify

```
sail artisan migrate
```

#### Add a Tag Seeder

```
sail artisan make:seeder TagSeeder
```

- Import the Tag model
- Follow the same structure as the Category seeder, and prepopulate the DB with some tags (PHP, Laravel, Node.js... no need to be exhaustive, but give yourself a few to start working with).
- Add the TagSeeder to the DatabaseSeeder
- Run a fresh migration that includes the seeders
  - sail artisan migrate:fresh --seed

#### Prepopulate Project Tags via a ProjectsTagsSeeder

- Import the Project Model
- Assuming that you have added some projects and tags via the preceding seeders, you can now explicitly attach some tags to one or more projects.

```php
    public function run()
    {
        // Explicitly attach tags to projects
        $portfolioProject = Project::find(1);
        $portfolioProject->tags()->attach([1,2,3]);
    }
```

## Managing Many-To-Many Relationships

There are three helper functions that we can use to manage our many-to-many relationship.

Let's walk through them briefly, from the Project perspective, assuming that we have our Project instance in $project and tags with ids 1 through 5 in our DB.

#### attach()

We can pass in an array of tag ids and one row will be created in projects_tags for each.

```php
$project->tags()->attach([1,2,5]);
```

This would most often be used in ProjectController store() method.

#### sync()

We can pass in an array of tag ids and projects_tags will be updated to reflect only these pairings.

```php
$project->tags()->sync([2,4]);
```

This would most often be used in ProjectController update() method.

#### detach()

This will remove all rows for this project from the projects_tags table.

```php
$project->tags()->detach();
```

This would most often be used in ProjectController destroy() method, although the cascading delete would remove the rows from the pivot table anyway.

## Adding Tags via Project Create/Edit Form

Both the attach() and sync() helpers take arrays of ids.

There are several ways that we could approach sending this from our form, the two most common being:

- A select with the multiple attribute
- A set of checkboxes, all with the same name attribute

In either case, you'll want make sure that the name attribute has square brackets as a suffix or else you'll have to construct the array yourself from the posted data.

Here's how I've set this up, as a multi-select, pre-selecting the current set of tags attached to a project when the form first loads and then persisting the old() options if there were any validation errors on submit.

```php
<label for="tags" class="block mb-2 uppercase font-bold text-xs text-gray-700">Tags</label>
<select name="tags[]" id="tags" multiple="multiple">
    @foreach ($tags as $tag)
    <option value="{{ $tag->id }}"
        @if (old('tags') && in_array($tag->id, old('tags')))
                selected
        @elseif ($project && $project->tags)
            @foreach ($project->tags as $projectTag)
                @if ($tag->id == $projectTag->id)
                    selected
                @endif
            @endforeach
        @endif
        >
        {{ $tag->name }}</option>
    @endforeach
</select>
@error('tags')
    <p class="text-red-500 text-xs mt-1">{{ $message }}</p>
@enderror
```

I'll leave it to you to implement the attach and synch in appropriate ProjectController methods, with just the hint that you should do it after creating or updating the project record itself.

## Read-only JSON API

This is a quick and dirty approach to setting up read-only endpoints to return Project, Category, and Tags data as JSON, but it works.

Rather than rendering views, we'll use the json() method of the Response class to return what we need.

**ProjectController.php**

```php
    public function getProjectsJSON()
    {
        $projects = Project::with(['category','tags'])->get();
        return response()->json($projects);
    }
```

**CategoryController.php**

```php
use App\Models\Category;

class CategoryController extends Controller
{
    public function getCategoriesJSON()
    {
        $categories = Category::all();
        return response()->json($categories);
    }
}
```

**TagController.php**

```php
use App\Models\Tag;

class TagController extends Controller
{
    public function getTagsJSON()
    {
        $tags = Tag::all();
        return response()->json($tags);
    }
}
```

**Add Routes**

```php
Route::get('/api/projects', [ProjectController::class, 'getProjectsJSON']);
Route::get('/api/categories', [CategoryController::class, 'getCategoriesJSON']);
Route::get('/api/tags', [TagController::class, 'getTagsJSON']);
```

https://xqsit.github.io/laravel-coding-guidelines/docs/naming-conventions/
