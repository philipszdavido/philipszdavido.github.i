---
layout: post
title: Learn How Redux Works by building one
---

This tutorial is a hands-on creation of the popular state management library, __Redux__.
We will build our own clone of Redux. If you must know, Redux is a state management library for JavaScript apps. It actually combines all our app states into a store.

# Introduction

This tutorial is a hands-on creation of the popular state management library, __Redux__.

We will build our own clone of Redux. If you must know, Redux is a state management library for JavaScript apps. It actually combines all our app states into a store.

`State management` has been one of the most important aspects in frontend-development. We may lose track of our app's state over time during development. It may result in wrong presentation of data.

You see it's important that all the possible states of an application should originate from a single source - single source of truth. Thus, Redux was born, it encompasses the whole app's states in into a single store.

There were other methods used to manage states before Redux, but the mechanics on how to manage and track state depended wholelly on the developer. It was a difficult task, there are many states in an app that will change depending on time, user behavior, or
of different reasons, bugs and state leaks eventually emerges.

We are going to discuss the core concepts of Redux and also build a clone of the Redux library. We will build a `todoapp` powered by our Redux clone.

To make things easier we are going to setup our project with an automated testing. Testing is one of the most important thing in software development. It is very crucial to have your code tested before you release.

In the next sections, we will see how we will setup our project alongside the Facebook's testing framework, __Jest__.

# Source Code

The source code implemented in this article can be found on GitHub, at [https://github.com/philipszdavido/redux/](https://github.com/philipszdavido/redux/)

# Building blocks of Redux

__Store__, __Actions__ and __Reducers__ are the main core concepts of Redux. We will go over them in-depth below.

## Store

Store in Redux is a singleton object that holds the state of an app. When we create a store using the `createStore` function, it provides us with functions, `getState`, `subscribe`, and `dispatch` that are very useful in managing and tracking our app's state.

* __getState:__ This returns the current state of our application in the store.
* __subscribe:__ This is used to register listeners in the store. They execute whenever a state in the store updates.
* __dispatch:__ This is used to update the state.

To illustrate the compositions of a store in Redux, we created a book-manager code snippet using Redux below:

```js
import { createStore } from 'redux'

const booksReducer = (state= {}, action) => {
    switch(action.type) {
        case "ADD_BOOK" : 
            return Object.assign({}, state, action.payload)
        default:
            return state
    }
}
```
We create a store using the `createStore` function passing `booksReducer` as argument:

```js
const store = createStore(booksReducer);
```
With the `store` variable, we can add books to the store using the  dispatch function:
```js
store.dispatch( { type: "ADD_BOOK", payload: { title: "Brutus" } } ) // adds a book, 'Brutus' to the store
```

To get books in our store, we can do this:
```js
console.log(`Books: ${store.getState()}`) // outputs books in our store
```
Or we can listen in to events whenever a book is added:

```js
store.subscribe(() => {
    console.log(`Books: ${store.getState()}`)
})
```
You see how easy to use Redux, just define your reducers and let Redux know about them, then you are good to go.

Redux can be used in any framework: `React.js`, `Angular`, `Vue.js` etc.

## Actions

Actions tells the store how to affect the state. Actions are literally `dispatched` into the store.

Actions are plain JavaScript objects used to send payloads of information to the store. Store use the payloads to update the state. It consists of keys: `type` and `payload`.

* type: This tells the store where to affect the state.
* payload: They provides the store with info on how to update the state.

```js
{ type: " ", payload: [] || {} || String || Number || Boolean }
```
This is an example of the Action object. Notice the `key`, and ` payload` properties. The payload property is optional and can be of any data type, if the action should carry any info, then the payload will hold this data.

Actions are emitted in Redux using the `dispatch` function.

```js
store.dispatch({ type: "ADD_BOOK", payload: { title: "Julius Caesar" } })
```
`store.dispatch` takes in an action object as an arg, it calls the reducers in the store with the current state and the action object, `reducing process`, then, at last it calls all the listeners subscribed in the store, to update them with the new state.

## Reducers

`Reducers` are pure functions used by `Redux` to alter the current state in the store and returns a new one.

Reducers takes two args, `state` and `action`. The state arg is the current state in the store and the action arg is the JavaScript object containing a payload and type properties.

Being, a pure function reducers only take its inputs and produces an output without affecting any external variables. Here, immutability is the watch word. States of the store can't be mutated, a new state must be returned. It is adviseable to use only non-mutating Object,Array methods in your reducer functions.

###### Reducer example
```js
const (state = {}, action) => {
    switch(action.type) {
        case ADD_BOOK:
        return {
            book: action.payload
        }
        default:
            return state
    }
}
```
This is a classic example of a simple reducer function. It take in the current state and action object, sent by the `dispath` method. Working down, we seethe use of the `switch-case`. With that we are able to execute the particular action pre-defined by the developer. You see why the action object is useful, it tells the reducer the kind of action to take and also supplies it with an optional information on how to affect the state.


# Project setup

We will use Node.js runtime for our project development and testing. NPM, also will be used in pulling in our module dependencies.

__NB:__ NPM is an acronym for __N__ ode __P__ ackage __M__ anager.

Before we proceed, let's make sure the Node.js runtime is installed in our machine:
```sh
$ node -v
v6.10.0
```
We can test for NPM, though it comes installed with Node.js:

```sh
$ npm -v
3.10.10
```
The versions doen't matter, we just want to be sure they are all installed.

Next, we create our project directory:

```sh
mkdir redux && cd redux
mkdir src && mkdir test
```

Next, we initiate a Node.js project:
```sh
npm init -y
```
I generates a package.json. Package.json is used to give info to NPM that allows it to identify the folder as a Node.js project. It also, tells NPM which module(s) the project depend on.

# Enable Unit Testing with Jest

As stated earlier, a good software dev. process goes along with unit testing. Unit testing reduces the number of bugs released during deployment, making it very effective in software development. In unit testing, individual components of the project is tested, therefore, when any of the components are modified, your unit tests should not fail. Any failure indicates that some components depend on some components. 

We will be using the `Jest` framework for unit testing.

# createStore

## subscribe
## getState
## dispatch

# combineReducers



# Conclusion


