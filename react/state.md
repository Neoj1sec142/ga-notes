# Intro to State in React & React Hooks: useState

![Hook](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2F68.media.tumblr.com%2F4abecc3bda7a596a142b27e0554fbeb1%2Ftumblr_oaued85JWK1vreqoxo6_r1_500.gif&f=1&nofb=1)

## Overview
In this lesson, we'll learn about how React manages state and a newer addition to React: Hooks! We'll talk about what Hooks are, and get some practice using one called `useState`.  We'll also build a simple ToDo List! Bangarang!

## Objectives
- Discuss the need for `state` in front-end applications
- Manipulate state within a React application
- Differentiate between state and props
- Discuss the need to share `state` across different parts of the application
- Share state across multiple React components
- Learn about Hooks in React
- Learn how we can use the hook `useState` to manage our state
- Implement `useState` to build a todo list

## Getting Started
- `Fork` and `Clone`
- `cd` into this folder
- `code .` and open in VSCode
- `npm i` to get our dependencies installed
- `npm start` to spin up our React App

![Bye](https://external-content.duckduckgo.com/iu/?u=http%3A%2F%2Fmedia.giphy.com%2Fmedia%2FSvOrq4OA7TQTC%2Fgiphy.gif&f=1&nofb=1)

## What is "State"? 

As we have seen in our exploration so far, most of our job as web developers centers around displaying, storing, and manipulating **data**. This data is rarely *static*, and nearly every action a user takes modifies some or all of the data. Because of this, the *shape* of our data is constantly changing as our application runs. Another word for the *shape* of our data at a given point in time is `state`.

Until now, we have stored `state` for HTML in global JS variables. We've used `JavaScript` to set and retrieve these values as we react to user input. This has worked out fine, but it requires quite a bit of work on our part. On top of that, the responsibility for handling our state has been shared between our HTML and Javascript files.

React gives us a much simpler way to manage this state, and it allows us to keep all of it inside of our Javascript alone.

## How does React manage state? 

Open your `TodoList.js` file and you'll see the following:

```jsx
import React from 'react'
import Tasks from './Tasks'
import Input from './Input'

const TodoList = () => {

  return (
    <div className="list">
      <Input />
      <Tasks tasks={[]} />
    </div>
  )
}

export default TodoList
```

![Peter](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2F31.media.tumblr.com%2F4dd9097369563409ff79a8c1a3fbaed1%2Ftumblr_na7pik5KuV1qjzijvo6_400.gif&f=1&nofb=1)

Now you may be wondering how we can hold state here...

Let me introduce you to the `useState` hook!

The `useState` hook is a declarative way to manage state, each `useState` is in charge of managing it's own state! Let's see this in action. In order to utilize `useState`, we need to import it from React.

Modify your `React` import statement in your `TodoList.js` to the following:

```jsx
import React, { useState } from 'react'
```

Next let's set up some state:

```jsx
import React, { useState } from 'react'
import Tasks from './Tasks'
import Input from './Input'

const TodoList = () => {

  const [tasks, manageTasks] = useState([])

  return (
    <div className="list">
      <Input />
      <Tasks tasks={[]} />
    </div>
  )
}

export default TodoList
```

Let's break this down:
- We're importing the `useState` hook from react *(It's built into the newer versions of the React library)*
- We're using some fancy code to gain access to two items, `tasks` and `manageTasks`. This is **[Array Destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)**.
- We're setting both of these items equal to `useState` which is a function *(all hooks are functions)* and initializing it with an empty array.

The `useState` hook provides us with two things: 1) a variable to access our state and 2) a method to manipulate it. The `manageTasks` method is directly in control of the `tasks` state. `useState` accepts an argument that sets our initial state, in this case an empty array.

![Smile](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fmedia.giphy.com%2Fmedia%2FiPuYCSSb2pspa%2F200.gif&f=1&nofb=1)

## How is State different from Props?

In many ways State and Props operate similarly.  Both are plain JS Objects that hold data about your application and both of them trigger a render when they are updated.  However, there are some important differences between the two.

### Props are passed to child components from the parent component. State can be stored anywhere in your application, however it can only be passed in one direction, **down**.

- When we refer to Props, we're referring to the JSX "attributes" (properties) that have been passed down from a parent component. Like this:

  `<Movie title={"Blade Runner"} year={1982} />`

- State is declared within a component.

### Props Are Immutable, State is Not

- It is impossible to directly change a components `prop` values. Props are `read-only` values
- State is able to be mutated (changed) via the method in the hook. More on that soon.

### State can be passed down into another component's Props, but not vice-versa

- We will often pass parts of a component's state down to child components.  The child gains the value in its own `props`
- Child components cannot pass state up to a parent (Remember Unidirectional Data Flow)
- Only down the component tree... like this...

![Cannonball](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fmedia.tenor.co%2Fimages%2Fad40b517ae9388840a43e4f7ab493bff%2Fraw&f=1&nofb=1)
___

## Back to Our ToDo List

Now that we understand *what* state is, let's add some items to our initial state and pass these tasks to our `Tasks` component:

```js
import React, { useState } from 'react'
import Tasks from './Tasks'
import Input from './Input'

const TodoList = () => {

  const [tasks, manageTasks] = useState([
    'Sweep the Floor',
    'Do Dishes',
    'Make Bed',
    'Feed the Dog'
  ])

  return (
    <div className="list">
      <Input />
      <Tasks tasks={tasks} />
    </div>
  )
}

export default TodoList
```

You'll notice we can directly access the `tasks` state via the `tasks` variable. That's the beauty of `hooks`: less code, more features! Be mindful, the downward data flow rule still applies, even with hooks!

![Bangarang](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fmedia1.tenor.com%2Fimages%2F28e0a769ba0dd705c2e20444df10dff3%2Ftenor.gif%3Fitemid%3D3294382&f=1&nofb=1)

---

## Setting State

Let's add some items to our `tasks` state.
Start by creating two functions in your `TodoList` component:

```js
import React, { useState } from 'react'
import Tasks from './Tasks'
import Input from './Input'

const TodoList = () => {

  const [tasks, manageTasks] = useState([
    'Sweep the Floor',
    'Do Dishes',
    'Make Bed',
    'Feed the Dog'
  ])

  const addTask = () => {}

  const handleChange = (event) => {}

  return (
    <div className="list">
      <Input />
      <Tasks tasks={tasks} />
    </div>
  )
}

export default TodoList
```

Let's pass these functions down to the correct components:
- `handleChange` should go to the `Input` component.
- `addTask` should go to the `Input` component.

```jsx
import React, { useState } from 'react'
import Tasks from './Tasks'
import Input from './Input'

const TodoList = () => {

  const [tasks, manageTasks] = useState([
    'Sweep the Floor',
    'Do Dishes',
    'Make Bed',
    'Feed the Dog'
  ])

  const addTask = () => {}

  const handleChange = (event) => {}

  return (
    <div className="list">
      <Input handleChange={handleChange} addTask={addTask} />
      <Tasks tasks={tasks} />
    </div>
  )
}

export default TodoList
```

Let's run a test with our `addTask` function, add in the following:

`addTask`:

```js
const addTask = () => {
  let myNewList = [...tasks, 'My Task']
  manageTasks(myNewList)
  console.log(tasks)
}
```

In your `Input.js` wire the `addTask` function to the button provided:

```jsx
import React from 'react'

const Input = (props) => {
  
  return (
    <div className="tasks">
      <label>Input Task: </label>
      <input type="text" name="task" />
      <button onClick={props.addTask}>Add</button>
    </div>
  )
}

export default Input
```

Try clicking the add button in your browser!

You should see `My Task` added to the list!

The `manageTask` function mutates the state it is associated with!

It accepts an argument of what we want to set a specific state to, the `manageTasks` is bound specifically to the `tasks` state and is the only function that can directly control it!

![Bravo](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2F64.media.tumblr.com%2F5d5b7c004e5cff775da309ec580876b3%2Ftumblr_phwi528fPb1vlacsto1_500.gifv&f=1&nofb=1)

## You Do

Implement the following features:
- Create a new task utilizing the `Input` component. Hint: You may need another `useState`. The value of the input should be stored in this state. The `handleChange` function is there to help, **USE IT**.
- I should be able to add the newly typed task to our `tasks` state, half of this has already been done for you. 😉

## Removing a Task
So now we can add tasks to our ToDo list. Bangarang! But what if we want to remove a task from the list?  Let's code that together!

First, similar to how `addTask` allows us to *add* items to our list, we'll need to create a new function to help us *remove* items! Let's create a `removeTask` function!

In our **TodoList.jsx**, let's set something up like this...
```js
const removeTask = () => {

}
```

Now, because we know that we can't directly mutate our **tasks** array, we'll need to add a local variable to store a snapshot of that state (exactly how we did with *addTask*)...
```js
const removeTask = () => {
  let taskList = [...tasks]
}
```

We use the spread (...) operator to bring in our current **tasks** state and save it to our new variable (**taskList**). 

Before we continue on, let's be sure and pass our new function in as a prop to our **Tasks** component. Still in **TodoList.jsx**...
```js
<Tasks tasks={tasks} removeTask={removeTask} />
```

Now we'll need to use some sort of array method to make a change to our **taskList** array.  Let's take a look at [.splice()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice) to see how we could use that to remove a task from our list.

Let's chain .splice() onto our new task array. We read the docs (right?), so we know that splice takes in two arguments: an **index**, and the **amount of items to remove**. So let's be sure and pass those two things in, like this...
```js
const removeTask = () => {
  let taskList = [...tasks]
  taskList.splice(index, 1)
}
```
But wait! We still need to pass **index** in as an argument to our **removeTask** function. Luckily the [.map()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) taking place in our **Tasks.jsx** gives us access to one.  Let's go take a look!

In **Tasks.jsx**, we see that a unique index number is *mapped* out with every new ToDo item.  Now all we have to do is make sure we give that to our **removeTask** function! First, let's add a button to be created with every task...
```js
    <ul>
      {props.tasks.map((task, index) => (
        <li key={index}>
          {task} 
          <button>
            x
          </button>
        </li>
      ))}
    </ul>
```

Then, let's add the HTML property *onClick* to our button to indicate what we want to happen...
```js
    <ul>
      {props.tasks.map((task, index) => (
        <li key={index}>
          {task} 
          <button onClick={}>
            x
          </button>
        </li>
      ))}
    </ul>
```

Now let's bring in our **removeTask** function from props. Like this...
```js
    <ul>
      {props.tasks.map((task, index) => (
        <li key={index}>
          {task} 
          <button onClick={() => props.removeTask(index)}>
            x
          </button>
        </li>
      ))}
    </ul>
```
Let's discuss why we've written our **removeTask** function this way.  

In *JSX*, whenever we use **{ }**, we are telling our script that we are going to do some sort of *JavaScript* evaluation. 

In order to pass in our **index**, we have to add parentheses to our **removeTask** function. This effectively *invokes* the function immediately when the line of code is read.  We don't want that! So to avoid it, we precede it with an *anonymous function*.  This **holds** the action until the *onClick* occurs and allows us to pass our argument (**index**) without issue!

![Yay](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fmedia1.tenor.com%2Fimages%2F9257ba2e05056ada8d18ee40a7a1b945%2Ftenor.gif%3Fitemid%3D19236972&f=1&nofb=1)

Back in **TodoList.jsx** we simply need to pass the **index** in as an argument to our **removeTask** function. Like this...
```js
const removeTask = (index) => {
  let taskList = [...tasks]
  taskList.splice(index, 1)
}
```

Now we can use our **manageTasks** method from our *useState* hook to modify the state...
```js
const removeTask = (index) => {
  let taskList = [...tasks]
  taskList.splice(index, 1)
  manageTasks(taskList)
}
```

We pass our temporarily stored state (**taskList**) into the method as the new state, and boom! Let's try removing an item!

![Lost Boys](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fi0.wp.com%2Fbraindeadradio.com%2Fwp-content%2Fuploads%2F2012%2F04%2Fhook.gif&f=1&nofb=1)

Us ^

## Lesson Recap
In this lesson, we learned about State and how it differs from Props.  We learned how to change our State using our first React Hook: useState.  We created a cool ToDoList and used both a button and a form to modify our state! Remember, data is always passed down, not up.

## Resources
- [React State](https://reactjs.org/docs/state-and-lifecycle.html)
- [React Components and Props](https://reactjs.org/docs/components-and-props.html)
- [React Hooks](https://reactjs.org/docs/hooks-intro.html)
- [Array Destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)
- [.splice()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)
- [.map()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
- [The Steelhanded Stingray](https://youtu.be/7ikcZfXiFRQ)