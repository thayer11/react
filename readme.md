<!--
Creator: Ilias
Market: SF
-->

![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png)

<!-- 9:00 5 minutes -->

<!--Hook: So at this point, you're familiar with Angular which is one of many front-end frameworks.  Today, we'll talk about React, another incredibly popular front-end framework maintained mainly by Facebook. It fits with the philosophy that Taylor at Spruce mentioned over the weekend: it is a tool that does a lot of front-end work for you so you don't have to reinvent the wheel.  For those interested in putting it into your project 4 after this class, please follow up with me.-->

#ReactJS

### Why is this important?
<!-- framing the "why" in big-picture/real world examples -->
*This workshop is important because:*

React is a library that Facebook invented to help build custom HTML elements. React provides a declarative library that keeps the DOM (the view) in sync with your data (the model). It is concerned only with the **V** in **MVC** and as a result can be used in conjunction with other libraries to help manage state.

### What are the objectives?
<!-- specific/measurable goal for students to achieve -->
*After this workshop, developers will be able to:*

* **Create** and **render** React components in the browser
* **Nest** and **embed** React components
* **Modify** the state of a React component through events

### Where should we be now?
<!-- call out the skills that are prerequisites -->
*Before this workshop, developers should already be able to:*

* **Write** client-side applications in JavaScript
* **Know** Gulp as a build tool

<!--9:05 5 minutes -->

##From Docs

>We built React to solve one problem: **building large applications with data that changes over time**.

>Many people choose to think of React as the V in MVC.

>In fact, with React the only thing you do is build components. Since they're so encapsulated, components make code reuse, testing, and separation of concerns easy.

##JSX

JSX is a JavaScript syntax extension that looks similar to XML that helps represent DOM elements in JavaScript.

```js
var myElement = <MyComponent someProperty={true}/>;
```

Here's an example of JSX, don't worry about understanding it just yet.

```js
var Nav, Profile;
// Input (JSX):
var input = <Nav color="blue"><Profile>click</Profile></Nav>;
// Output (JS):
var output = React.createElement(
  Nav,
  {color:"blue"},
  React.createElement(Profile, null, "click")
);
```

<!--9:10 5 minutes -->

## Setup

###JSX Syntax in Sublime

To view JSX appropriately in Sublime:

* Install the package `babel` in sublime
* Select syntax `Babel > Javascript (Babel)` 

###Getting Started

`cd` into `starter-code`

* Contains a simple Express server that we'll use in today's class.
* Look at the `readme.md` file at the root of the application to see how to get it setup.

<!--Half-mast-->

<!--9:15 10 minutes -->

## React Components

### Hello World: A Very Basic Component

The basic unit you'll be working with in ReactJS is a **component**.

* Using "components" is a pretty different way of approaching web development.

It is common to separate HTML, CSS and Javascript from one another.

* With components, there is more integration and less separation of HTML CSS and JS.
* Instead, the pattern is to organize a web app into small, reusable components that encompass their own content, presentation and behavior.

In `app/components/hello-world.jsx` let's create a super simple component that just says, "Hello World". In order to do this we're going to write in ES6 as it makes the syntax easier (and yes, we are consciously not using semicolons).

```js
class HelloWorld extends React.Component {
  render() {
    return <p>Hello World!</p>
  }
}
```
From above, we can see that we are creating a new Component class that is inheriting from `React.Component`. It has one function `render`. React expects `render` to be defined as that is the function that will get called when it is being rendered to the DOM. Note we are using ES6's [method definition syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Method_definitions#Description).

Don't forget to require `React` and export your new component!

```js
import React from 'react'

class HelloWorld extends React.Component {
  render() {
    return <p>Hello World!</p>
  }
}
export default HelloWorld
```

It could be refactored to [export](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export) the new class in the same line it is being defined.

```js
import React from "react"

export default class HelloWorld extends React.Component {
  render() {
    return <p>Hello World!</p>
  }
}
```

###The Virtual DOM

* Every component has, at minimum, a render method that generates a Virtual DOM node to be added to the actual DOM.
* A Virtual DOM is just like a regular ol' DOM node, but it's not yet attached to the DOM. Try typing `var photo = new Image` in the console; that `photo` is part of the virtual DOM.
* For React, the virtual DOM is a staging area for changes that will eventually be implemented.
* The contents of this node are defined in the method's return statement using JSX.

So we've created the template for our component. But how do we actually render it?

The below `.render` function takes two arguments:

* The component to render
* The DOM element to append it to

```js
ReactDOM.render(
  <HelloWorld/>,
  document.getElementById("hello-world-component")
);
```

In your `app/index.jsx` let's import the new component and render it to the DOM. `React` is used for creating web components, while `ReactDOM` (used above) is used for actually rendering these elements to the DOM. Let's import our component and `ReactDOM` to get this working. It may look something like this:

```js
"use strict"

import React from 'react'
import ReactDOM from 'react-dom'
import HelloWorld from './components/hello-world'
 
ReactDOM.render(
  <HelloWorld/>,
  document.getElementById("hello-world-component")
)
```

>Note: It's a good idea to use [`"use strict"`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode) as it is a restricted variant of JavaScript that eliminates some of JavaScript's silent errors by changing them to throw errors.

### Mad Props

Our `Hello` component isn't too helpful. Let's make it more interesting.

* Rather than simply display "Hello world", let's display a greeting to the user.
* How do we feed a name to our `Hello` component without hardcoding it into our render method?

```js
// update your HelloWorld Component to accept a name prop
export default class HelloWorld extends React.Component {
  render() {
    return <p>Hello {this.props.name}!</p>
  }
}
```

```js
// render it by passing in a `name` prop
ReactDOM.render(
  <HelloWorld name="WDI"/>,
  document.getElementById("hello-world-component")
)
```

What are `.props`?

* Properties! Every React component has a `.props` method.
* You can think of them like custom html attributes for React.
* Properties are immutable and cannot be changed by the element itself, only by a parent.
* We define properties and pass them in as attributes to the JSX element in our `.render` method.

We can create multiple properties for a component.

```js
export default class HelloWorld extends React.Component {
  render() {
    return (
      <div>
        <p>Hello {this.props.name}</p>
        <p>You are {this.props.mood}!</p>
      </div>
    )
  }
}
```

>Note, if you have nested html elements in your JSX you have to wrap them in parenthesis.

```js
ReactDOM.render(
  <HelloWorld name="WDI" mood="loving life"/>,
  document.getElementById("hello-world-component")
)
```

<!--9:25 5 minutes -->
<!-- Turn over to devs -->

### Challenge: Greet the day!

Create a component called `<Greeting/>` that has two props: `timeOfDay` and `object`, such that it's interface would be `<Greeting timeOfDay="night" object="moon">` and it would print out "Goodnight moon" in an `h3`. Append it to a `div` with the `id` `greeting-component`. (Hint: don't forget to create that div in your HTML.)

<!--Example solution

```js
class Greeting extends React.Component {
  render() {
    return(
      <h3>Good{this.props.timeOfDay} {this.props.object}</h3>
    )
  }
}
```

```js
ReactDOM.render(
  <Greeting timeOfDay="night" object="moon"/>,
  document.getElementById("greeting-component")
)
```

-->

<!--9:30 5 minutes -->
<!-- Half-mast -->

###Variable Props

What if we don't know exactly what props we're going to get but we want to pass them to the element. We can do this with something new in ES6 called a [spread operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_operator).

For example lets say we to create `ProfilePic` component that is a clickable image tag and we want to be able to pass in any new props into it.

```js
class ProfilePic extends React.Component {
  render() {
    return (
      <a {...this.props}>
        <img style={{width: '200px'}} src="http://bit.ly/1MItzOs"/>
      </a>
    )
  }
}
```

`{...this.props}` is where all props we pass in will live. So now we can set an `href` and `class` on our element by just passing them in!

>Note because `class` is a reserved word in ES6 we have to use `className` instead.

```js
ReactDOM.render(
  <ProfilePic href="http://www.nyan.cat/" className="profile-pic"/>,
  document.getElementById("profile-pic-component")
)
```

###Nested Components

What if we want to add text just above our Image, but want it also to be wrapped in the anchor tag so it is clickable, like the image?

Well could we just put in some text inside of our `ProfilePic` component like such? You can nest HTML after all!

```js
ReactDOM.render(
  <ProfilePic href="http://www.nyan.cat/" className="profile-cat">
    <h3>This Kitten Cashes Checks</h3>
  </ProfilePic>,
  document.getElementById("profile-pic-component")
)
```

Yes, we could do this! We would just need to modify our original component to render any `children` we pass in the correct place, which is done with `this.props.children`.

```js
class ProfilePic extends React.Component {
  render() {
    return (
      <a {...this.props}>
        {this.props.children}
        <img style={{width: '200px'}} src="http://bit.ly/1MItzOs"/>
      </a>
    )
  }
}

```

<!-- 9:35 5 minutes -->
<!-- Turn over to devs -->

###Challenge: Mr Cat's Profile

* Pass an `id` into the `ProfilePic` component that is `"mr-cat"`
* Pass a `p` child, below the `h3`, into the `ProfilePic` component with the class `"bio"` that contains a brief description of this kitty.

<!-- 9:40 5 minutes -->

##Exercise: A Blog Post

Let's practice what we've learned so far by building a `Post` component for our blog.

Make and render a custom `<Post/>` component with the attributes `title`, `author`, and `body`. The exact HTML/CSS composition of the component is up to you.

<!--Example solution

```js
class Post extends React.Component {
  render() {
    return (
      <div class="post">
        <h2>Hello {this.props.title}</h2>
        <h4>By {this.props.author}</h4>
        <p>{this.props.body}</p>
      </div>
    )
  }
}
```

```js
ReactDOM.render(
  <Post
    title={"Are you feline me?"}
    author={"Kitkat the Cat"}
    body={"Cause I'm purrfect"}
  />,
  document.getElementById("blog-component")
)
```

-->

<!-- 9:45 5 minutes -->

What if we wanted to add an array of comments to the post?

<!--

```js
ReactDOM.render(
  <Post
    title={"Are you feline me?"}
    author={"Kitkat the Cat"}
    body={"Cause I'm purrfect"}
    comments={["first!", "more gifs plz", "i haz had enuf ov dis >_<"]}
  />,
  document.getElementById("blog-component")
)
```
-->

<details>
<summary>Let's try map!</summary>

```js
class Post extends React.Component {
  render() {
    return (
      <div className="post">
        <h2>{this.props.title}</h2>
        <h4>By {this.props.author}</h4>
        <p>{this.props.body}</p>
        <h3>Comments</h3>
          <ul> {
            this.props.comments.map((comment, index) => {
              return <li key={index}>{comment}</li>
            })
          }</ul>
      </div>
    )
  }
}
```

>Note: when iterating through a collection, React needs to put a `key` on each item with a unique index for that collection (otherwise errors will appear). `.map`'s second argument to its callback is the (unique) index, so that works just fine. However, the actual unique database id on each comment would be better.

```js
ReactDOM.render(
  <Post
    title={myPost.title}
    author={myPost.author}
    body={myPost.body}
    comments={myPost.comments}
  />,
  document.getElementById("blog-component")
)
```

</details>

### Embedded Components

* What if there was a comment component we could abstract some of this logic to?

* Let's reference a comment using an embedded `<Comment/>` component inside of PostView's render method.

<!--9:50 10 minutes -->

### Challenge: Add Embedded Comments To Blog

1. Create a `Comment` component in the same way we did for `Post`.
* Your `Comment` render method should render a `body` property.

2. Amend your `Post`'s render method so that its return value generates as many `<Comment/>` elements as there are comments.
* Make sure to pass in the body as an argument to the component.

<!-- Example solution

```js
class Post extends React.Component {
  render() {
    return (
      <div className="post">
        <h2>{this.props.title}</h2>
        <h4>By {this.props.author}</h4>
        <p>{this.props.body}</p>
        <h3>Comments</h3>
          <ul> {
            this.props.comments.map((comment) => {
              return <Comment key={index} body={comment}/>
            })
          }</ul>
      </div>
    )
  }
}

class Comment extends React.Component {
  render() {
    return (
      <li>{this.props.body}</li>
    )
  }
}
```
-->

<!--10:00 10 minutes -->

## State

We already went over properties.

* The thing about props is that they can only be changed by a parent.
* So, what do we do if our component needs to trigger an update for its parent or the application as a whole? That's where **state** comes in.
* State is similar to props, but is *meant to be changed*.
* Like properties, we can access state values using `this.state.val`.
* State is good when the applications needs to "respond to user input, a server request, or the passage of time" (all events).
* Setting up and modifying state is not as straightforward as properties and instead requires multiple methods.
* More on what [should & shouldn't go in state](http://facebook.github.io/react/docs/interactivity-and-dynamic-uis.html#what-should-go-in-state).

Let's modify our earlier `HelloWorld` example to be a new `MoodTracker` component. There will be a mood displayed and  eventually a user will click a button to indicate on a scale of 1-10 how much of that mood they are feeling.

Now, let's instead use React's `.createClass` method as opposed to extending standard ES6 classes, as it makes some of this behavior more straightforward.

```js
const MoodTracker = React.createClass({
  getInitialState() {
    return {points: 1}
  },
  render() {
    return (
      <div>
        <p>Hello {this.props.name}</p>
        <p>On a scale of 1-10</p>
        <p>You are {this.props.mood}</p>
        <p>This much: {this.state.points}</p>
      </div>
    )
  }
})
```

Next we need to enable the user to change the state of our component. Let's create an `onClick` event that triggers a method `increaseMood` to increment our counter by 1 for each click. Notice that it is important to use the [`.setState`](https://facebook.github.io/react/docs/component-api.html#setstate) method to update the state. Also, we can define the initial state with `getInitialState` a reserved method in React.

```js
const MoodTracker = React.createClass({
  getInitialState() {
    return {points: 1}
  },
  increaseMood() {
    this.setState({
      points: this.state.points + 1
    })
  },
  render() {
    return (
      <div>
        <p>Hello {this.props.name}</p>
        <p>On a scale of 1-10</p>
        <p>You are {this.props.mood}</p>
        <p>This much: {this.state.points}</p>
        <button onClick={this.increaseMood}>Up Your Mood!</button>
      </div>
    )
  }
})
```

Whenever we run `.setState`, our component runs a **diff** between the current DOM and the virtual DOM node to update the state of the DOM in as few manipulations as possible.

* Only replaces the current DOM with parts that have changed.
* This is super important! Using React, we only change parts of the DOM that need to be changed.
  * Implications on performance.
  * We **do not** re-render the entire component like we have been in class.
  * This is one of React's core advantages

<!-- 10:10 5 minutes -->

### Challenge: Count to 10

After 10 clicks, the user should see the counter reset to 1.

<!--Example solution

```js
const MoodTracker = React.createClass({
  getInitialState() {
    return {points: 1}
  },
  increaseMood() {
    let newPoints = this.state.points >= 10 ? 1 : this.state.points + 1
    this.setState({
      points: newPoints
    })
  },
  render() {
    return (
      <div>
        <p>Hello {this.props.name}</p>
        <p>On a scale of 1-10</p>
        <p>You are {this.props.mood}</p>
        <p>This much: {this.state.points}</p>
        <button onClick={this.increaseMood}>Up Your Mood!</button>
      </div>
    )
  }
})
```

-->

<!-- 10:15 5 minutes -->

## Challenge: I Like!

Let's create a state for our earlier blog example. We want to be able to edit the body of our post. Tip: update the component to the `React.createClass` syntax.

1. Initialize a `likes` state for our `Post` component that defaults to 0.
1. Render the number of likes somewhere in your component.
1. Create a method, `like` that adds 1 to the state's number of `likes`.
1. Create a button that triggers the above function.

<!-- Checkout the `solution` branch for solutions.-->

<!--10:20 5 minutes -->

## What's Next?

* Getting user input with [forms](https://facebook.github.io/react/docs/forms.html)
* Triggering [events](https://facebook.github.io/react/tips/dom-event-listeners.html)
* A more [complicated example](http://codepen.io/ZebGirouard/pen/eZqjLd) by a GA instructor

## Questions / Closing

<details>
<summary>Having learned the basics of React, what are some benefits to using it vs. a different framework or plain ol' Javascript?</summary>

* Reusable components.
* High level of control.
* Imperative components, declarative views.
* Convenient templating of data.
* Fast rendering.

</details>
