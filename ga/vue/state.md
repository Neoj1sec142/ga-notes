# Vue State and Methods

![](https://ghalex.com/wp-content/uploads/2020/12/state.png)

## Getting Started

- Fork and Clone
- `npm install`
- `npm run serve`

## Overview

In this lesson, we'll learn how to set up and utilize state with Vue. Vue takes a different approach when it comes to handling state and storing it, but it follows the same conventions as other frameworks.

## State/Data In VueJS

In Vue, `state` is referred to as `data`. Although the names are different, they both achieve the same result, storing the current view in an application. Vue takes an interesting approach with how state is managed. When we set it up initially, our `data` is initialized as a function that returns an `object`. This `data` variable or key will reference an object with key/value pairs that will retain the information about the current state of our application, was a button clicked? Is the hamburger menu open? Is their information available from an api? This all plays into how information get's displayed in an application.

## Data Flow

Just like most frameworks, Vue utilizes a `downward` flow to data. This means that data/state can only be passed in one direction... **down**.

Each child component depends on the parent telling them when something was updated in order to re-render the ui for the end user.

## Setting Up State

Open `App.vue` in your text editor. You'll find the following code:

```jsx
<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <div class="button-container">
    </div>
    <div class='container'>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  components: {},
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
button {
  padding: 0.6em 1em;
  margin: 0 1em;
  border: none;
  background-color: #41b883;
  font-size: 1.1rem;
  border-radius: 4px;
  cursor: pointer;
}

.card {
  border: 2px solid rgba(0, 0, 0, 0.2);
  border-radius: 6px;
  margin: 1em;
  min-width: 400px;
  background: #2c3e50;
  color: #fff;
}
</style>
```

Let's start by importing some provided data:

```js
import wallets from './wallets.json'
```

### Initializing State

In Vue, we learned that state is known as something different, `data`. The `data` attribute **must be a function that returns an object**. `data` will contain our `initial` state for the component.

Let's see how that works.
In `App.vue`, add a `data` key to the export statement which has a value of an anonymous function that returns an object:

```jsx
<script>
  import wallets from './wallets.json'
  export default {
    name: 'App',
    data: ()=>({})
  }
</script>
```

We're using the shorter ES6 syntax here, but you can also use the ES5 function syntax:

```jsx
<script>
  import wallets from './wallets.json'
  export default {
    name: 'App',
    data: function (){
      return {}
    }
  }
</script>
```

We'll be using a mix of ES5 and ES6 functions throughout our time with Vue.

Inside of the `data` function, add a key called `wallets`, set this to `null` for now:

```jsx
<script>
  import wallets from './wallets.json'
  export default {
    name: 'App',
    data: ()=>({
      wallets: null
    })
  }
</script>
```

Open your `vue` devtools and select the `app` component. You should see the following:

![app-state](https://i.imgur.com/YpdkOhy.png)

We've successfully set up some state in our Vue app!

### Managing State

Now that we've set up our initial state, let's learn how to update it.

In `App.vue`, add a new object inside of the export statement and below the data function:

```jsx
<script>
  import wallets from './wallets.json'
  export default {
    name: 'App',
    components: {},
    data: () => ({
      wallets: null
    }),
    methods: {}
  }
</script>
```

The `methods` object contains all of the functions that are accessible to our component. These functions can be passed to other components as props as well!

In the `methods` object create two functions, `showWallets` and `hideWallets`, write these using the ES5 syntax:

```jsx
<script>
  import wallets from './wallets.json'
  export default {
    name: 'App',
    components: {},
    data: () => ({
      wallets: null
    }),
    methods: {
      showWallets(){},
      hideWallets(){}
    }
  }
</script>
```

Our `showWallets` method will set some kind of data to our `wallets` state. The `hideWallets` method will clear the `wallets` state.

But why did we write these using the ES5 function syntax? The ES5 function synatx retains the context of `this`. What does this mean, whenever we want to work with any kind of data or method in the current component, we need to use the `this` keyword. The `this` keyword is used by Vue to keep track of what we want to work with, think of `this` as a pointer to some kind of information.

For example, let's say we want to reference the `wallets` state. We would refer to it as `this.wallets`, essentially instructing Vue that we want to work with `this` `wallets` data in the current component.

Let's see how that works.

In the `showWallets` method, set `this.wallets` to our imported `wallets` data:

```js
showWallets(){
  this.wallets = wallets
}
```

When this method gets triggered, it will update the `wallets` state with the data from our json file.

While we're here, let's reset the `wallets` state with `hideWallets`:

```js
hideWallets(){
  this.wallets = null
}
```

### Triggering Changes

Now that we have our methods written, we can see them perform some magic.

Add the following inside of the `button-container` div:

```jsx
<button @click="hideWallets">Hide Wallets</button>
<button @click="showWallets">Show Wallets</button>
```

These buttons will listen for a `click` event and trigger our methods when the time comes. There's a couple of ways we can use the `click` event, this pattern is known as a directive, but you can also use `v-on:click`. We'll learn more about events in a later lesson.

Open up your Vue devtools and click on each button. Observe the changes made to the `wallets` state:

`showWallets` clicked:

![show-wallets](https://i.imgur.com/iHUt5Lf.png)

`hideWallets` being clicked should return our state to `null`.

## Displaying State Variables

At this point, we've successfully been able to update our `wallets` state with either an `array` or `null`. But having our data in stored in state is doing us no favors if we can't display it.

Let's start with a simple example first.

In the `data` function, add a new key of `heading` with a value of `Money Bags Transactions`:

```jsx
  data: () => ({
    wallets: null,
    heading: 'Money Bags Transactions'
  }),
```

We want to take this heading and display it in our application, take a guess as to how we can do it. **Hint: We used this same syntax when displaying props**

<details>
<summary>Click To View The Solution</summary>
    <code>
        {{variable}}
    </code>
</details>

Let's see this in action. Above the `button-container` div, add a `h3` with the `heading` state:

```jsx
<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <h3>{{ heading }}</h3>
    <div class="button-container">
      <button @click="hideWallets">Hide Wallets</button>
      <button @click="showWallets">Show Wallets</button>
    </div>
    <div class="container">
    </div>
  </div>
</template>
```

You should now see a heading displayed in your app:

![heading-state](https://i.imgur.com/e0ZDML4.png)

Now your first thought might be:

- What happened to `this`?!
- Why don't we have to reference the heading with `this.heading`?

The reason for this behavior is because Vue passes the methods and data to our template as `variables` in a way. Because our `heading` and `methods` were declared in this component, when accessing those methods or data Vue know's where they came from and what component they belong to.

### Displaying Our Wallets

To wrap up our app, we'll display our `wallets` on the page. Your first thought might be to use a `.map` method to accomplish this. However, Vue has it's own built in way of displaying multiple components from an array.

Enter `directives`...

Short story, `directives` are built in `bindings`/`helpers` that help us accomplish everything from iterating through data to conditional rendering. We'll learn more about these in an upcoming lesson, but for now add in the following within the `container` div:

```jsx
<div class="container">
  <div class="card" :key="wallet.id" v-for="wallet in wallets">
    <div class="card-header">
      <h3>{{ wallet.address }}</h3>
      <p>{{ wallet.balance }} BTC</p>
    </div>
  </div>
</div>
```

Don't worry about what this bit of code is doing, we'll dive deeper into this topic later on.

Head back to your browser and click the `Show Wallets` button. You should now see a list of wallets appear on your screen:

![wallet-list](https://i.imgur.com/5KPvKZ5.png)

You can also click the `Hide Wallets` button to hide the list of wallets as well!

## Recap

In this lesson, we learned how to initialize data/state in a Vue application. We utilized methods to manipulate the state and how to display data in our jsx template.

We slightly touched on `directives` which we'll learn more about in a future lesson.

As you can see, Vue follows similar conventions to other frameworks on how data should be handled and updated. There are some differences, but every framework does things slightly different.

## Resources

- [Data](https://vuejs.org/v2/guide/components.html#data-Must-Be-a-Function)
- [List Rendering](https://vuejs.org/v2/guide/list.html)