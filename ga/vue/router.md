# Vue Router

![](https://scotch-res.cloudinary.com/image/upload/w_1500,q_auto:good,f_auto/media/3660/h49pHk3ORDWKylIlU7bE_maxresdefault.jpg)

## Overview

In this lesson, we'll learn the basics of Vue Router. Vue Router is a tightly integrated extension for Vue that allows us to create multi page apps using the Vue framework.

## Getting Started

- `fork` and `clone`
- `cd` into the directory
- `npm install`
- `npm run serve`

## What is Vue Router?

Vue Router is the official routing library for VueJS. As you'll find with most Vue extensions/plugins such as `Vuex` and `Vue Router`, these are officially supported by the VueJS team.

Vue Router is known as a plugin in the Vue world. Plugins allow us to extend the functionality of our applications beyond the bare bones features of the framework itself.

In this case, Vue Router is used to create multi page applications that allow us to programmatically navigate from component to component giving the feeling of moving from page to page. In reality, Vue Router is actually conditionally rendering the component we want based on the URL address currently in the browser.

## Setting Up Vue Router

### Installation

To install **Vue Router**, we can utilize `npm` just like any other package:

```sh
npm install vue-router
```

Or, using the Vue CLI, you can also install it this way:

```sh
vue add router
```

We'll use `npm` for now.

### Setting Up The Router

Now that we've installed Vue Router, theres a little bit of set up that we have to do. Here are the steps we'll take in order for Vue to recognize Vue Router:

- create a `router.js` file in the src directory
  - we'll use this file to have a single entry point to configure routes and options (separation of concerns)
- import the createRouter and createWebHistory packages into our `router.js`
- import any components we want to display for our routes into `router.js`
- set up an array of objects, each object will contain the following:

  | Key       | Value                   | Use                                                                                                                |
  | --------- | ----------------------- | ------------------------------------------------------------------------------------------------------------------ |
  | path      | `url path`              | used by the browser to render a component                                                                          |
  | component | `imported component`    | used by vue router to determine what component should be used for which route                                      |
  | name      | `name of the component` | used by vue and vue router to enure that the correct component gets rendered, similar to `name` in a vue component, optional |

- create our router and set up the history mode
- export our router and use it in the `main.js` file.

Let's run through these steps together:

#### Step 1

Create a `router.js` file in the `src` folder.

#### Step 2

Import the packages from the vue router library:

```js
import { createWebHistory, createRouter } from "vue-router"
```

#### Step 3

Import the components we want to use as routes (these have been created for you already):

```js
import HomePage from './components/HomePage.vue'
import ListingsPage from './components/ListingsPage.vue'
import ListingView from './components/ListingView.vue'
```

#### Step 4

Set up the routes array:

```js
const routes = [
  { path: '/', component: HomePage, name: 'HomePage' },
  { path: '/listings', component: ListingsPage, name: 'ListingsPage' },
  { path: '/listings/:listing_id', component: ListingView, name: 'ListingView' }
]
```

#### Step 5

Create a router variable using createRouter that contains our routes and history mode, then export it:

```js
const router = createRouter({
  history: createWebHistory(),
  routes
})

export default router
```

*We'll talk about modes in the next section.*

#### Step 6

Next we need to import our new routes and add them to the `main.js` file:

Start by importing our router:

```js
import router from './router'
```

Once we've imported, we need to tell our Vue application that we are using Vue Router, modify the mount line to the following:

```js
createApp(App).use(router).mount('#app')
```

### Vue Router Modes

Out of the box:

> The default mode for vue-router is hash mode - it uses the URL hash to simulate a full URL so that the page won't be reloaded when the URL changes.

A hash URL would look like the following:

**`https://someurl.com/posts#index.html`**

This is not ideal for the following reasons:

- we can't dynamically change parameters in the URL (think ids, names, categories, etc.)
- can cause bizarre behavior with rendering components. The hash is typically utilized to display a different page or section of a page. One small problem though: we're using components, not multiple HTML pages.

So how do we fix this? We use the mode `history`. Here's why:

- history allows the browser to navigate back and forth between different pages
- allows us to pass dynamic values in the URL such as query strings or parameters
- browsers use history by default

So instead of having a weird hash URL, we can use standard URLs such as:

**`https://someurl.com/posts?post_query=tacos`**

**or**

**`https://someurl.com/posts/40`**

Long story short, you'll always use the history mode in Vue 3 and beyond!

## Linking Routes To Our Application

Although we've written the routes and linked them to `Vue Router`, we need to tell our Vue app about these routes. What we've written prior to this was a `plugin`. Although we've created a plugin, the actual Vue application, specifically the markup in our JSX, needs a place to display these routes.

`Vue Router` has a few built in components that will facilitate this communication:

- `router-view`
- `router-link`

**Router View** will display the routes we've created earlier and provide `properties` to these components that will allow us to interact with the browser's built in navigation.

**Router Link** is a component that will allow us to navigate between different components based on the `paths` we've defined for them.

### Implementing Router View

In the provided `App.vue`, add in the following to the `Routes go here section`:

```jsx
<router-view header="Listings In Miami"></router-view>
```

**Note: The `header` is being provided as a prop to other components, this will make the `header` prop available to any of our components we've defined in our routes.**

### Implementing Router Link

Next we'll set up some navigation for our application. Open the `NavBar.vue` component located in the `components` folder.
Add the following to the `nav` element:

```jsx
  <router-link to="/" name="HomePage">Home</router-link>
  <router-link to="/listings">Listings</router-link>
```

**Note: `to` is a required prop and must match the paths in the routes we created earlier. `name` is not a required prop. This is utilized in the case that we have similar routes. This will allow `Vue Router` to find the correct component by name.**

Head to your browser and you should see the following:

<div>
  <img src="https://i.imgur.com/A7WmktR.png" alt="app-with-routes">
</div>

## Programmatic Navigation With Vue Router

Vue Router also gives us some neat abilities to programmatically navigate from component to component using some in built methods. Here is a list of instances where programmatic navigation is useful:

- a user signs in to your app
- selecting a specific item to view details
- logging out a user
- when an action is complete (submitting a form, submitting a purchase order, etc.)

Let's take a look as to how we can accomplish this. Open the `ListingsPage.vue` file located in the `components` folder. In the `script` section, find the `navigateItem` method. The skeleton is already set up for us and the method is wired to each `Listing`. We'll start by making some modifications:

```js
    navigateItem(id) {
      this.$router.push(`/listings/${id}`)
    }
```

*Be sure and pass `id` in as a parameter!*

`this.$router` is a prop that:

- has certain methods built in that allow us to navigate
- allows us to access props such as `params`, `query`, etc.

The only reason we can use `this.$router` is because we've installed `Vue Router` and our component is listed in the `routes` we created earlier.

Once you've made the necessary modifications to the `navigateItem` method, head back to your browser and select a listing from the `Listings` page:

<div>
  <img src="https://i.imgur.com/boNuie5.png" alt="listings-nav"/>
</div>

In the next section we'll talk about how we have the ability to view a *specific* listing.

## Route Params

In this section we'll explore how the `Listing` is able to find the correct vehicle from the provided data. If you look closely at your browser's address bar, you'll notice that the URL changes depending on which listing is selected. We start with:

- `http://localhost:8080/listings`

  and end up with

- `http://localhost:8080/listings/819850`

This is what's known as a `URL parameter`. This is a dynamic value that changes depending on what we specifically select.

Let's take a look at the `ListingView.vue` component and find out exactly how this is being used.

Locate the `mounted` lifecycle method and you should see the following:

```js
  mounted: function() {
    this.listing = listings.find(
      listing => listing.id === parseInt(this.$route.params.listing_id)
    )
```

Pay close attention to this specifically:

```js
this.$route.params.listing_id
```

In this case we're using `this.$route` to access the `params` object that contains our `listing_id` that we defined earlier when creating our routes.

**Note: Whatever value gets set as a parameter always gets converted to a string. If you're working with numbers, it may be necessary to convert it back to an integer like in our example above.**

## Recap

In this lesson, we learned the basics of Vue Router. Vue Router can be used to build some very awesome applications and although it has a bit of setup, the end product is always worth it. It has a simple API that allows us to have the freedom to pick and choose how we go about building our apps.

## Resources

- [Vue Router](https://router.vuejs.org/)