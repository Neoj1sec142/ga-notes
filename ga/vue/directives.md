# Vue Directives

![](https://i.morioh.com/200524/d057f525.jpg)

## Getting Started

- Fork and Clone
- `npm install`
- `npm run serve`

## Overview

In this lesson we'll learn all about Vue directives and the roles they play in building functional Vue applications. Directives are the backbone to any Vue app, without them, we wouldn't be able to conditionally render components, pass props, trigger event functions or even display data!

## What Are Directives

From the VueJS docs:

> If you have not used AngularJS before, you probably don’t know what a directive is. Essentially, a directive is some special token in the markup that tells the library to do something to a DOM element.

In simpler terms, `directives` are a built in set of instructions that are written in our `jsx`.

### What roles do directives play?

Directives play an important role in Vue, they provide us with a declaritive and elegant way to make our jsx content dynamic. These `directives` do perform a little bit of magic behind the scenes to make our lives easier as developers, but remember these directives translate to regular javascript at runtime.

## Types Of Directives

There are quite a few `directives` built into Vue. In this lesson we'll touch on a couple of most commonly used ones. Below you'll find a list of common directives and the roles they play:

| Directive   | Role                                                                                          |
| ----------- | --------------------------------------------------------------------------------------------- |
| `v-bind`    | Binds some kind of data to a component, typically a dynamic value such as state or props      |
| `v-for`     | Iterates through data to return some kind of jsx with each item in the iterable               |
| `v-if`      | Checks for a truthy value and displays some kind of element                                   |
| `v-else-if` | Checks for a second truthy value and will display this element instead                        |
| `v-else`    | Fallback in case the `v-if` or `v-else-if` fails                                              |
| `v-on`      | Handles dom events such as `click` or `change`. In Vue, the `change` event is called `input`. |

## Binding Data

Let's start with the `v-bind` directive. This is a directive that you've already used during our lesson on props and components. The difference is we used the shorthand syntax `:someProp="prop"`.

The `v-bind` directive binds data to specific component, think of it as passing something valueable to someone and telling them to keep it safe.

Let's see how that works. In the provided codebase, create a components folder and a component called `Header.vue`.

In this component, add a `template` with a `h1` for now and your script tag with the components name:

```jsx
<template>
  <h1></h1>
</template>

<script>
export default {
  name: 'Header'
}
</script>

```

Next, add a `props` object to `Header.vue` and accept a prop called `header` with type of `String`:

```jsx
<template>
  <h1></h1>
</template>

<script>
export default {
  name: 'Header',
  props:{
      header: String
  }
}
</script>

```

Finally, render the `header` prop in your `h1`:

```jsx
<template>
  <h1>{{ header }}</h1>
</template>
```

### Displaying the Header

In `App.vue`, import the `Header` component and add it to the `components` object:

```jsx
<script>
import Header from './components/Header.vue'
export default {
  name: 'App',
  components: {
    Header
  },
  data: () => ({
    headerTxt: 'My Header',
    showHeader: false
  })
}
</script>
```

Display the `Header` underneath the current `img` element:

```jsx
<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <Header />
  </div>
</template>
```

We currently have a header provided for us in our `data`, let's see how we can pass that to the `Header` component. Modify the current `Header` tag to the following:

```jsx
<Header v-bind:header="headerTxt" />
```

Check your browser and you should see the header text displayed on the page:

<!-- ![vue-header](https://sei-r.s3.amazonaws.com/u4_lesson_vue_directives/vue-header.png) -->

If we're passing props, why did we do it differently in our `props` lesson? That's because we were using the shorthand syntax for binding!

Remove the `v-bind` directive from the header and leave only `:header="headerTxt"` and you should see the same result.

The `:` syntax header performs the same thing as `v-bind` in a much shorter syntax, they both achieve the same end result. `v-bind` is used to set dynamic values/javascript on some kind of jsx content. In this case, we're using it to pass a prop to our header component, but it can also be used to set the value of text inputs or any other jsx element!

For example, for an `input` element:

```jsx
<input :value="someDynamicValue"/>
```

**or**

```jsx
<input v-bind:value="someDynamicValue" />
```

## Conditional Rendering

In VueJS, there are also directives to help us conditonally render elements. We'll use `v-if` and `v-else` for now, but `v-else-if` is also and option. Let's see that in action.

On the `Header` tag in `App.vue` add the following:

```jsx
<Header v-if="showHeader" v-bind:header="headerTxt" />
```

If you check your browser, you'll notice our header disappeared!

Let's change the value of `showHeader` to `true` and the `Header` component should reappear.

What if we wanted to show something if `showHeader` was false? We can use the `v-else` directive. Let's add another header to `App.vue` and add the `v-else` directive to it:

```jsx
<Header v-else header="Second Header" />
```

**Note: We're not using the `v-bind` directive since we're hardcoding a value for the `header` prop**.

Change `showHeader` back to false and the second header should show instead!

Toggle the `showHeader` between `true` and `false` to observe the behavior.

By utilizing the `v-if/v-else` directive we created a more legible and neater component for other's to read!

## Iterating Through Data

Next on our list is iterating through data. We saw this briefly in our previous lesson with the `v-for` directive.

The `v-for` directive takes some kind of iterable and returns each element which in turn can be used to return multiple components.

Like most frameworks, each item will require a `key` to ensure the render order.

Let's try this for ourselves.

In `App.vue`, create a new key in `data` with the name `phoneNumbers` and add the following as the value:

```json
[
  {
    "createdAt": "2021-05-02T21:30:25.793Z",
    "phoneNumber": "584-685-9289",
    "id": "1"
  },
  {
    "createdAt": "2021-07-21T09:20:27.068Z",
    "phoneNumber": "(300) 472-7336",
    "id": "2"
  },
  {
    "createdAt": "2021-05-07T08:39:49.178Z",
    "phoneNumber": "287-308-1738",
    "id": "3"
  },
  {
    "createdAt": "2020-12-22T00:36:24.078Z",
    "phoneNumber": "(842) 660-8761",
    "id": "4"
  }
]
```

We'll use this array of phone numbers for our example.

In `App.vue`, we'll create a new `div` for each phone number object with a `h4` with the phone number as content and a `key` as the `id`:

```jsx
    <div :key="phoneNumber.id" v-for="phoneNumber in phoneNumbers">
      <h4>{{ phoneNumber.phoneNumber }}</h4>
    </div>
```

You should now have a list of phone numbers in your browser:

![phone-list](https://i.imgur.com/pbGtoEW.png)

## You Do

Using the provided phone numbers, create a new component called `ContactList` that accepts the `phoneNumber` array as a prop.

- remove the current `v-for` directive in `App.vue`.
- use the `v-for` directive to list the phone numbers in the `ContactList` component
- use the `v-if` directive to check if the phone number includes `()`, if it does, give the `h4` component a `class` of `underline` and style it with an underline for the number. You can use the `text-decoration` css style to accomplish this, other wise use the `v-else` directive to render the phone number with no decoration.

## Recap

In this lesson, we learned how `directives` play a huge part in the Vue ecosystem. Each directive does a specific job in order to achieve an interatible application. Directives ensure that the markup is legible and organized by being explicit and declarative.

## Resources

- [Vue Cheatsheet](https://devhints.io/vue)
- [List Rendering](https://vuejs.org/v2/guide/list.html)
- [Conditional Rendering](https://vuejs.org/v2/guide/conditional.html)