+++
title = "Day1"
weight = 1
pre = "- "
+++

## What is Vue.js

Vue.js is also an open-source front-end JavaScript framework.

Vue.js was built by Evan You as a side project after working for Google and encountering the limitations of working with AngularJS. He built it with the aim of being as powerful as Angular, but lighter and easier to learn and use.

According to Evan You:

Vue.js is a more flexible, less opinionated solution. It’s only an interface layer so you can use it as a light feature in pages instead of a full-blown SPA.”

## Advantages and Disadvantages of Vue

#### Advantages

- Simplicity and ease of use
  - It's an ideal choice for small to medium sized projects
- Learning curve
  - Vue syntax has a more flexible architecture and intuitive templating system than either React and Angular.
- Size
  - The Vue.js downloadable only around 18 KB.
- Performance
  - Due to its lightweight nature and streamlined architecture, Vue outperforms React and Angular.
  - Like React, Vue makes use of the Virtual DOM which makes UI updates blazing fast.
- Readability
  - Vue.js uses a single-file-component architecture. Everything is a component and components contain all of the HTML, CSS, and JavaScript in one file.
- Growth
  - Though still smaller than the React or Angular, the Vue.js community is growing.

#### Disadvantages

- Lack of scalability
- Smaller overall community
  - Fewer learning resources
  - Fewer plugins and libraries available
- Excessive flexibility
  - The unopinionated nature of Vue.js can lead to poorly structured code.
- Less mature tooling
- Less enterprise adoption
- Learning curve for advanced features
  - Despite the simplicity and ease of use that makes Vue.js easy to get started with, more advanced features can be challenging to implement, particularly since the overall ecosystem has less to offer by way of ready-to-use plugins and libraries.

https://survey.stackoverflow.co/2022/#section-most-popular-technologies-web-frameworks-and-technologies

https://2022.stateofjs.com/en-US/libraries/front-end-frameworks/

## Vue Extensions and DevTools

Browser DevTools

https://devtools.vuejs.org/guide/installation.html

VSCode Extension : Volar

https://marketplace.visualstudio.com/items?itemName=MisterJ.vue-volar-extention-pack

## Vue Setup

#### Loading Vue.js Via CDN

Given how lightweight Vue.js is, if you are working on a relatively small project and/or wanting to quickly prototype components, it is entirely possible and reasonable to simply load Vue.js from a CDN, add a bit of script to our HTML, mount the app and go!

**index.html**

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Hello Vue from CDN</title>
  </head>
  <body>
    <h1 id="app">{greeting}</h1>

    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
    <script>
      const { createApp } = Vue;
      createApp({
        data() {
          return {
            greeting: "Hello Vue World!",
          };
        },
      }).mount("#app");
    </script>
  </body>
</html>
```

#### Creating a Vue Application via CLI (with build step)

This is the way to go for more complex applications.

The process is very much like initializing a React or Angular app.

The only requirement is Node.js version 16 or higher.

In your terminal, navigate to the parent folder of where you want your Vue app to live, then run:

```
npm init vue@latest
```

If it isn't already installed, this will install create-vue (think create-react-app) for you, and then it will show a set of prompts walking you through the project config.

Hint: When you get the Project name: prompt, just start typing your project name.

Go ahead and accept the defaults for the rest of the prompts:

## Hello Vue App

In the last step we initialized a Vue project.

The (now familiar) next steps are:

- cd hello-vue-app
- npm install
- npm run dev
- open your browser to http://localhost:5173/

#### Quick Tour of the Boilerplate Project

- index.html
  - Has a div with id="app" which is where our app will mount
  - Loads /src/main.js as a module
- main.js
  - imports { createApp } from vue
  - Imports the App component from ./App.vue
  - imports ./assets/main.css
  - mounts the App component in the #app div
- App.vue
  - This is our root Vue component
  - It is a Single-File Component (SFC) and encapsulates all of the component's logic, template, and styles, neatly split out into `<script>, <template>, and <style> tags.`

There are two ways that Vue components can be authored:

- Options API
- Composition API

This boilerplate code is using the Composition API. We'll compare the two in the next slide.

## Options and Composition APIs

There are two styles for authoring Vue components: the Options API and the Composition API. They both achieve the same result, and are largely interchangeable, but they differ in how we write our code.

#### Options API

Using the Options API we explicitly export an object that contains a set of options properties such as data, methods, and mounted. These options properties are then available by referencing this on the component instance.

```
<script>
export default {
  // Properties returned from data() become reactive state
  // and will be exposed on `this`.
  data() {
    return {
      count: 0
    }
  },

  // Methods are functions that mutate state and trigger updates.
  // They can be bound as event listeners in templates.
  methods: {
    increment() {
      this.count++
    }
  },

  // Lifecycle hooks are called at different stages
  // of a component's lifecycle.
  // This function will be called when the component is mounted.
  mounted() {
    console.log(`The initial count is ${this.count}.`)
  }
}
</script>

<template>
  <button @click="increment">Count is: {{ count }}</button>
</template>
```

#### Composition API

With the Composition API, instead of having an options object we define the component's logic using imported API functions.

There's more going on behind the scenes and as such this approach requires a broader knowledge of the Vue.js architecture, but allows us to write less boilerplate code since more things are handled at compile time.

Here's the same component as above, but using the Composition API.

```
<script setup>
import { ref, onMounted } from 'vue'

// reactive state
const count = ref(0)

// functions that mutate state and trigger updates
function increment() {
  count.value++
}

// lifecycle hooks
onMounted(() => {
  console.log(`The initial count is ${count.value}.`)
})
</script>

<template>
  <button @click="increment">Count is: {{ count }}</button>
</template>
```

For the remainder of today's lesson I'll work with the Options API since it keeps things closer to the surface.

## Template Interpolation

We can reactively output any data that we've set on a component using the mustaches syntax in our template.

Continuing with the counter example example we have a single property for count declared as part of the data option.

```
<script>
export default {
  // Properties returned from data() become reactive state
  // and will be exposed on `this`.
  data() {
    return {
      count: 0,
    };
  },

```

We're dynamically outputting the value of count in the template here:

```
<template>
  <button @click="increment">Count is: {{ count }}</button>
</template>
```

#### Practice

Add greeting and name properties to the component data and output them in an h1 above the button.

Note: The content inside the mustaches can be any valid JavaScript expression, but cannot be statements. As in React, this means you'll find extensive use of ternary and logical operators in those interpolation blocks.

#### More Practice

Change the h1 content such that if the count is 0 it outputs the greeting, but otherwise it outputs an encouraging message to the user.

## Attribute binding with v-bind

We can use mustache interpolation to dynamically output content within elements, but mustaches cannot be used to set HTML attributes.

Instead, when we want to set an element's attribute to a property of the component we use the v-bind directive.

The basic syntax for v-bind is:

```
v-bind:attribute="property"
```

Let's explore this by:

Adding a style classes for .winning and .losing that set text color to green and red respectively.

```
<style scoped>
h1 {
  width: 100%;
}
.winning {
  color:
green;
}
.losing {
  color:
red;
}
</style>
```

Adding a gameStatus property to our component data

```
data() {
    return {
      count: 0,
      greeting: "Hello again",
      name: "Josh",
      gameStatus: null,
    };
  },
```

Add a setGameStatus method that sets this.gameStatus to winning if count is greater than 0, losing if less than 0, or or null.

Add a decrement method that decrements the count

Have both increment() and decrement() call this.setgameStatus

Bind the class attribute of the h1 element to the value of gameStatus

```
  <h1 v-bind:class="gameStatus">
    {{ count === 0 ? greeting : "Keep going" }}, {{ name }}
  </h1>
```

With that in place, we are dynamically setting the class attribute on our h1 to the value of gameStatus.

#### Shorthand for v-bind

Since v-bind is so ubiquitous, there is a special shorthand syntax where we just start our binding with a :

```
  <h1 :class="gameStatus">
    {{ count === 0 ? greeting : "Keep going" }}, {{ name }}
  </h1>
```

It looks weird at first, but you'll get used to it quickly.

## Event Handling with v-on

To listen for events we can either use the v-on directive or the shorthand @ symbol.

Regardless of which syntax is used, we can either call a method defined on the component or provide an inline handler.

#### Shorthand Syntax w/ Method Handler

This will listen for clicks and call either the increment or decrement method, both of which are defined in the methods property of our options object.

```
  <button @click="increment">Increment</button>
  <button @click="decrement">Decrement</button>
```

#### v-on Syntax w/ Method Handler

```
  <button v-on:click="jackpot">Jackpot!</button>
```

#### Inline Handler

```
  <button
    @click="() =>
      {
        this.count = 0;
        this.setGameStatus();
      }
    "
  >
    Reset
  </button>
```

#### Receiving the Event Object

When an event is thrown we can pass the native event object into our handler method (or an inline function) using the automatically populated $event variable or using the usual event syntax with an inline arrow function.

```
  <button @click="scorePoints($event)">5</button>
  <button @click="(event) => scorePoints(event)">10</button>
```

```
    scorePoints(e) {
      const points = Number(e.target.textContent);
      this.count += points;
      this.setGameStatus();
    },
```

The above works for all of the usual native DOM events, such as:

- keyup, keydown
- focus, blur
- change, submit
- etc.

Shortly, we'll be setting up custom events as well, and the syntax/pattern for handling those will be the same.

## Form Input Two-Way Binding

To implement two-way binding between properties and form inputs we can use a combination of v-bind and v-on, or we can use the more concise v-model directive to accomplish both at once.

#### Using v-bind (:) and v-on (@)

```
  <div>
    <label for="greeting">Greeting:</label>
    <input
      type="text"
      id="greeting"
      :value="greeting"
      @input="
        (e) => {
          this.greeting = e.target.value;
        }
      "
    />
  </div>
```

#### Using v-model

```
  <div>
    <label for="name">Name:</label
    ><input type="text" id="name" v-model="name" />
  </div>
```

These work for more than just text inputs.

For more info see the Form Input Bindings guide: https://vuejs.org/guide/essentials/forms.html#text

## Conditional Rendering

Vue.js provides a set of directives for handling conditional rendering of elements.

#### v-if

If we only want to render an element when an expression is true, we can use the v-if directive.

```
<h1 v-if="count >= 50">You Win!</h1>
```

#### v-else-if

If we want to have a secondary conditional we can use v-else-if.

```
  <h1 v-if="count >= 50">You Won!</h1>
  <h1 v-else-if="count <= -5">You Lost!</h1>
```

#### v-else

Lastly, we have v-else.

```
  <h1 v-if="count >= 50">You Won!</h1>
  <h1 v-else-if="count <= -5">You Lost!</h1>
  <div v-else>Keep on clickin'...</div>
```

Once again, it may take some getting used to the syntax for these since there is no clear connection between the v-if, v-else-if, and v-else, ESPECIALLY if you have complex templating, but...

- a) With practice it becomes natural
- b) If you find yourself with overly complex templates that may be a clue that you should break things up into smaller components.

#### v-show

Vue provides another directive for conditionally displaying an element, v-show .

The syntax is the same as v-if, but it always renders the element to the DOM and then toggles the CSS display property to control visibility.

```
  <div v-show="count >= 50 || count <= -5">
    <button @click="reset">Play Again?</button>
  </div>
```

#### Containers for Grouped Conditional Rendering

If (and when) you have a multiple elements that you want to conditionally render based on the same expression, you can wrap them in a container element OR if you don't want to clutter up your DOM you can wrap them in a `<template>` tag.

```
  <template v-if="count < 50 && count > -5">
    <button @click="increment">Increment</button>
    <button @click="decrement">Decrement</button>
    <button @click="scorePoints($event)">5</button>
    <button @click="scorePoints($event)">10</button>
    <button v-on:click="jackpot">Jackpot!</button>
    <button @click="reset">Reset</button>
  </template>
```

## Loops with v-for

Where conditionals go, loops tend to follow.

For most of our looping needs in Vue we use variations of the v-for directive.

I've added an array of task objects to the component data, and outside the component I declared a let variable so that we can easily increment ids:

```
<script>
let id = 0;

export default {
  // Properties returned from data() become reactive state
  // and will be exposed on `this`.
  data() {
    return {
      count: 0,
      greeting: "Hello again",
      name: "Josh",
      gameStatus: null,
      tasks: [
        { id: id++, text: "Learn React", done: true },
        { id: id++, text: "Learn Angular", done: true },
        { id: id++, text: "Learn Vue", done: false },
      ],
```

#### v-for with an Array

```
  <h2>Task List</h2>
  <ul>
    <li v-for="task in tasks" :key="task.id">
      <input type="checkbox" v-model="task.done" />
      <span :class="{ done: task.done }">{{ task.text }}</span>
      <button @click="deleteTask(task)">X</button>
    </li>
  </ul>
```

Although Vue is more flexible than React about each item in a list needing a unique key attribute, it is still a good idea to bind one - particularly if you'll potentially be mutating the array.

If necessary you we can also access the index of each item in an array with:

```
  <h2>Task List</h2>
  <ul>
    <li v-for="(task, index) in tasks" :key="index">
      <input type="checkbox" v-model="task.done" />
      <span :class="{ done: task.done }">{{ task.text }}</span>
      <button @click="deleteTask(task)">X</button>
    </li>
  </ul>
```

#### v-for with a Range

v-for can also be used to loop through integers, though this is truly an odd duck.

```
<span v-for="n in 10">{{ n }}</span>
```

The mind-boggling quirk here is that n has an initial value of 1.  
I do not know why!

## Computed Properties

Computed properties are a mechanism for tracking, updating, and caching state when dependencies change.

They are declared as another property of the options object (distinct from data, methods, lifecycle hooks etc.) but are then used in templates as though they were any other data property.

This example declared filteredTasks as a computed property that depends on tasks and hideCompleted.

```
export default {
  // Properties returned from data() become reactive state
  // and will be exposed on `this`.
  data() {
    return {
...
      tasks: [
        { id: id++, text: "Learn React", done: true },
        { id: id++, text: "Learn Angular", done: true },
        { id: id++, text: "Learn Vue", done: false },
      ],
      hideCompleted: false,
    };
  },

  // Computed Properties are a mechanism for automatically updated state when dependencies change
  computed: {
    filteredTasks() {
      return this.hideCompleted
        ? this.tasks.filter((t) => !t.done)
        : this.tasks;
    },
  },

  ...
```

The template, including a button for toggling the value of hideCompleted, would then be:

```
  <h2>Task List</h2>
  <ul>
    <li v-for="task in filteredTasks" :key="task.id">
      <input type="checkbox" v-model="task.done" />
      <span :class="{ done: task.done }">{{ task.text }}</span>
      <button @click="deleteTask(task)">X</button>
    </li>
  </ul>

  <button @click="hideCompleted = !hideCompleted">
    {{ hideCompleted ? "Show All" : "Hide Completed" }}
  </button>
```

## Child Components

Naturally, Vue applications are usually composed of a hierarchy of nested components and provide mechanisms for passing and manipulating data through relationships.

Let's handle individual tasks in the task list as child components.

#### Passing Props to a Child Component

First, let's create a new SFC named Task.vue in the components directory and set it to receive an Object to be stored in props as task. We can also move the .done class styling.

**src/components/Task.vue**

```
<script>
export default {
  props: {
    task: Object,
  },
};
</script>

<template>
  <input type="checkbox" v-model="task.done" />
  <span :class="{ done: task.done }">{{ task.text }}</span>
  <button @click="deleteTask(task)">X</button>
</template>

<style scoped>
.done {
  text-decoration: line-through;
}
</style>
```

Then, in App.vue we need to:

- Import the Task component
- Declare it in the components property of our the App options
- Render the Task component in our template, using v-bind to bind the individual task object to the corresponding prop in the child component.

```
<script>
import Task from "./components/Task.vue";

let id = 0;

export default {
  components: {
    Task,
  },

  ...

</script>
```

```
  <h2>Task List</h2>
  <ul>
    <li v-for="task in filteredTasks" :key="task.id">
      <Task :task="task" />
    </li>
  </ul>
```

## Emitting Events from Child Components

To pass data from a child component to a parent we can emit custom events, including a payload, from the child and then use a v-on listener in the parent to respond accordingly.

To accomplish this, in the Task component we'll:

- Add an emits property with the name of the custom event we will emit
- Add a methods object containing the local deleteTask which then
  - emits the custom event
  - attached the task itself as a second argument

```
<script>
export default {
  props: {
    task: Object,
  },
  emits: ["taskDelete"],
  methods: {
    deleteTask(task) {
      this.$emit("taskDelete", task);
    },
  },
};
</script>

<template>
  <input type="checkbox" v-model="task.done" />
  <span :class="{ done: task.done }">{{ task.text }}</span>
  <button @click="deleteTask(task)">X</button>
</template>

<style scoped>
.done {
  text-decoration: line-through;
}
</style>
```

In the parent component we then just listen for the taskDelete event and pass the payload on to the deleteTask method.

```
  <h2>Task List</h2>
  <ul>
    <li v-for="task in filteredTasks" :key="task.id">
      <Task :task="task" @taskDelete="(task) => deleteTask(task)" />
    </li>
  </ul>
```

## Mutating State!

We can now mark tasks as done and delete them, so let's add support for adding tasks to our list.

To accomplish this we will:

- Add a newTask property, initialized to an empty string
- Add an addTask() method that pushes a new task onto the tasks array, using the value of newTask for the task text.
- Add a form to our template that listens for submit events, prevents default form behaviour, and then calls addTask
- Within the form, add an input that is bound to the newTask property

```
  data() {
    return {
      count: 0,
      greeting: "Hello again",
      name: "Josh",
      gameStatus: null,
      newTask: "",
      tasks: [
        { id: id++, text: "Learn React", done: true },
        { id: id++, text: "Learn Angular", done: true },
        { id: id++, text: "Learn Vue", done: false },
      ],
```

```
    addTask() {
      this.tasks.push({ id: id++, text: this.newTask, done: false });
      this.newTask = "";
    },
```

```
  <h2>Task List</h2>
  <form @submit.prevent="addTask">
    <input v-model="newTask" />
    <button>Add Task</button>
  </form>
```
