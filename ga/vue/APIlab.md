# Vue Splashgram

![](https://d2jq2hx2dbkw6t.cloudfront.net/374/vuejs-instagram-feed.png)

## Getting Started

- Fork and Clone
- `npm install`

## What You'll Be Building

![API Example](https://i.imgur.com/dq1sbMi.png)

## Instructions

### Step 1

You'll need an API key from `unsplash` to utilize their API:
- Sign up for API access [HERE](https://unsplash.com/developers)
- Once you've signed up, log in and go to `Your Apps`
- Create a new application and follow the prompts.
- Save your `Access Key` and store it within a `.env` file at the root of your project
- Store it under the variable `VUE_APP_UNSPLASH_KEY`

### Step 2

A feed component has been provided for you and has already been linked within `App.vue`.

In `Feed.vue`:
- Make a request to the unsplash API utilizing the provided `BASE_URL` in `globals.js`
- You'll want to utilize this endpoint `${BASE_URL}/photos?client_id=${API_KEY}`
- Set up some initial state, we'll want to store an array of photos and a selected photo.
- Set up an `axios` request that is triggered once the component mounts, you can make this `async`.
- If your API request is successful, store the data from the response in state/data.

### Step 3

Import the `Feedcard` component, this component has been set up for you.

- Place the `FeedCard` component within the `scroll-feed` div in `Feed.vue`
- You'll want to return a card for every photo in your state.
- Pass in a `photo`prop to the feed card component.

**Hint: The `v-for` directive may be useful.**

### Step 4

Start by importing the `DetailView` component, this component has already been set up for you.

- Place this component within the `item-view` div and pass your `selectedPhoto` state to this component as a `photo` prop.
- Replace the `somestate` variable in the `item-view` div's `v-if` directive with whatever you called your selected photo state.

If you've done this correctly, after Step 5 you will see an image show on the right hand side of the page.

### Step 5

We want to be able to select a photo from our feed.

- Start by creating an **asynchronous** `selectPhoto` method within `Feed.vue`.
- The `selectPhoto` method should accept an argument of `photoId`
- In this method, make a request to the following endpoint: `${BASE_URL}/photos/${photoId}?client_id=${API_KEY}`
- Take the response from this request and store it within your `selectedPhoto` state.
- Finally, pass the `selectPhoto` method as an emitter to `FeedCard`.

### Step 6

Try out the application, test out your features and make sure that you can:
- retrieve photos
- select a photo to view details

_Additional Steps:_

Browse the current code base and try to:
- Identify how the layout is implemented
- follow how the methods are all tied together
- Understand how the photo is being selected.

## Resources

- [List Rendering](https://vuejs.org/v2/guide/list.html)
- [Lifecycle Hooks](https://vuejs.org/v2/api/#Options-Lifecycle-Hooks)
- [Custom Events](https://vuejs.org/v2/guide/components-custom-events.html)