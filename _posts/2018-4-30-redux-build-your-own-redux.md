---
layout: post
title: Learn How Redux Works by building one
excerpt: This tutorial is a hands-on creation of the popular state management library, __Redux__.
post_url: https://cdn-images-1.medium.com/max/1600/1*BpaqVMW2RjQAg9cFHcX1pw.png
---

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

* __type__: This tells the store where to affect the state.
* __payload__: They provides the store with info on how to update the state.

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

Wuth that our project directory will look like this:

```
+- redux
 +- src
 +- test
 +- package.json
```

# Enable Unit Testing with Jest

As stated earlier, a good software dev. process goes along with unit testing. Unit testing reduces the number of bugs released during deployment, making it very effective in software development. In unit testing, individual components of the project is tested, therefore, when any of the components are modified, your unit tests should not fail. Any failure indicates that some components depend on some components. 

We will be using the `Jest` framework for unit testing.
Jest is a JavaScript testing framework developed by Facebook, used to unit test JS/React apps. It works out of the box for both JS/React apps. One of its great feateures is snapshot testing, it captures snapshots of serialible values to simlpfy testing and to analyze how state changes over time.

Jest expects to find our tests files in the test folder. That was the reason we created the test folder earlier. Although, it has been a norm in JS testing community to put all your test flles in test folder, we are going to stick to it.

To start with, we need to install jest:

```sh
npm i jest -D
```
This installed jest as a dev dependency:
```json
'''
  "devDependencies": {
    "jest": "^22.4.3"
  },
'''
```
We won't need jest to be bundled alongside our redux library, that's the reason we installed it as a dev Dep. We need to add an NPM script `test: jest` into package.json:

```json
...
    "test": "jest"
...
```

We made `Jest` easier to invoke by using an NPM run script. By saving the command into `package.json` we don’t have to remember what it was and can instead invoke a shorthand version:

```sh
npm run test
```

Let's put a dummy test to check everything is working correctly:

```js
// test/test.spec.js
describe("Equality test:", () => {
    it("2 should equal 2", () => {
        expect(2).toEqual(2)
    })
})
```
Now, if we run npm run test, we will see our test run and pass:

```sh
$ npm run test

> @chidumennamdi/redux@0.0.1 test C:\wamp\www\developerse\projects\redux
> jest

 PASS  test\test.spec.js (20.908s)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        40.67s
Ran all test suites related to changed files.
```
We are actually going to use ES7/ES6/ES5 features, and with that Node.js was built on ES5 and Jest uses Node.js to run our tests, so it will throw errors whenever it encounters any ES6/7 code. To transpile our code, we are going to use `Babel`.

Babel is a tool for converting cutting-edge JS/ES feateurs in old JS that old brower can understand. Babel itself doen't do everything it uses plugins/presets to support a perticular language features.

Before using any presets or plugins, babel-core must be present:

```sh
npm i babel-core -D
```

We install `babel-jest`, since we are using Jest as our test framework. `babel-jest` is a `babel` plugin for `jest`, that supports ES6/7.

Next, we need a babel preset that will transpile our code to ES5. We will use babel-preset-env.

babel-preset-env is new babel plugin that enables plugin/presets to be set per environment.

Go ahead and install the plugin:

```sh
npm i babel-preset-env -D
```
To tell babel the configurations we are going to use, we are going to create a .babelrc file.

Babel reads the config in .babelrc and uses the info to transpile our code.

```sh
touch .babelrc
```
This creates `.babelrc` in our project's root directory. We open it and add our configurations:

```json
{
    "presets": [
        [
            "env",
            {
                "loose": true,
                "targets": {
                    "node": "current"
                }
            }
        ]
    ]
}
```
Every `.babelrc` configs starts with presets or plugins property.
We set the `target` to `node` with the `current` version. This is so because Jest will run our tests on the Node environment.

Before that, we define the `loose` key and set it to true, this activates `loose` mode when transpiling. `loose` mode transpiles ES6/7/8 code to ES5.

Babel and its configuartions and a library full of presets/plugins is a very huge topic dedicated to its own series of tutorials. We just explained here the configurations and presets we will be using in this project. You can visit [Babel website](https://babeljs.io) for more information on its configurations and presets/plugins.

Last thing, we don't want to continously run the `npm run test` NPM script to run our __tests__. We would need Jest to watch our test folder and rerun all tests whenever there is a file change (i.e when we add more tests or edit an existing test). Jest provides an option for that `--watch` or `--watchAll`.

* __--watch:__ reruns only the changed file.
* __--watchAll:__ reruns all files.

We will edit our `test` script to add the `--watch` flag:

```json
// package.json
...
    "test": "jest --watch"
...
```
We can now test it out:

```sh
Admin@PHILIPSZDAVIDO MINGW64 /c/wamp/www/developerse/projects/redux (master)
$ npm run test

> @chidumennamdi/redux@0.0.1 test C:\wamp\www\developerse\projects\redux
> jest --watch

 PASS  test\test.spec.js (16.941s)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        35.629s
Ran all test suites related to changed files.

Watch Usage
 › Press a to run all tests.
 › Press f to run only failed tests.
 › Press p to filter by a filename regex pattern.
 › Press t to filter by a test name regex pattern.
 › Press q to quit watch mode.
 › Press Enter to trigger a test run.
```

All our tests are run and `jest` waits idly watching for file change. Also, notice the command doesn't exist, so if you want to make changes to your project, you have to open another terminal.

In this section we have setup our unit tests that will aid us in the implementation of our very own Redux.

__NB:__ Throughout this article, I'll assume your test suite is continuosly running, so will not explicitly state when to run the tests.

# Implementing Redux APIs

Redux has many functions that helps immensely in state management:

* createStore
* combineReducers
* applyMiddleWare
* bindActionCreators
* compose

We are going to implement these two basic ones:

* `createStore` : creates the initial store/state and returns functions:

    * subscribe
    * getState
    * dispatch 

* `combineReducers`: merges reducer functions into one single reducer function.

We will begin our implementation of Redux with `createStore`.

## createStore

###### Arguments
1. __(reducer)__: A reducing function
1. __(preLoadedState)__: Initial state of the app
###### Returns
1. __Object__: This object contains functions subcribe, getState, dispatch.


We have seen what to expect, the args and returns. The function takes in a reducing function and an object, then returns an object containig functions. Let's make our very first test case to pass this behaviour.

We start by creating `createStore.js` file in `src`:

```sh
touch src/createStore.js
```

__NB:__ As we delve further into this article, I will only tell you when and where to create a file. No bash code will be shown again. Also, you must be in the root directory of the project.

Let's create a test file for `createStore` in `test/createStore.spec.js` and add the following test case to it:

```js
// test/createStore.spec.js
import { createStore } from './../src/createStore'

describe("createStore", () => {
    it("should expect to Throw", () => {
        expect(() => createStore()).toThrow()
    })
    it("should return an object", () => {
        expect(typeof (() => createStore(()=>{},{}))()).toBe('object')
    })
    it("should expect to Throw", () => {
        expect(() => createStore({}, {})).toThrow()
    })
})
```
The first test checks `createStore` function throws when called without params. The secound, verifies an `object` was returned when called with params. The last case verifies that the `reducer` arg must be a function.

We can make the test pass easily, Create `src/createStore` and set the contents:

```js
// src/createStore.js
export function createStore(reducer, preLoadedState) {

    let currentReducer = reducer
    let currentState = preLoadedState

    // we make sure the reducer is a function
    if (typeof reducer != 'function') {
        throw new Error('reducer must be a function')
    }

    return {}
}
```
Here, we declared the function `createStore` with args `reducer` and `preLoadedState`. `reducer` parameter is assigned to a variable `currentReducer`, and the `preLoadedstate` which holds the initial state is stored in the `currentState` variable. We will soon see where and how we will employ `currentReducer` and ` currentState` variables.

Next, we checked the `reducer` type to make sure it's a function. The `reducer` arg must be a pure function which takes a state and action object as arguments.

We returned an object `{}`. This object as we progress will contain the functions Redux exposes to users.

#### Checking for store functions

Here, we make our createStore return object containing functions `subscribe`, `getState` and `dispatch`. These are functions used by Redux to manage states. 

We add a test case, to assert that createStore returns an object containing `subscribe`, `getState` and `dispatch`:

```js
// test/createStore.spec.js
describe('createStore', () => {
    it("should return an object with subscribe, getState, and dispatch", () => {
        const store = createStore(() => {}, {})

        const methods = Object.keys(store)
        expect(methods.length).toBe(3)
        expect(methods).toContain('getState')
        expect(methods).toContain('subscribe')
        expect(methods).toContain('dispatch')
    })
})
```

To make this test pass, we define functions getState, dispatch, and subscribe:

```js
// src/createStore.js
...
function getState() {

}
function dispatch() {

}
function subscribe() {

}
...
```

Then, we add them to the returning object:

```js
// src/createStore.js
...
function getState() {

}
function dispatch() {

}
function subscribe() {

}
return {
    dispatch,
    subscribe,
    dispatch
}
```
With these, our test cases passes. 

#### Implementing subscribers/listeners

[RxJS](https://github.com/ReactiveX) made popular the notion of subscribers. Where a subscriber is notified of data change over time. Likewise in Redux, we can implement subscriber to subscribe to the store to be notified of any change to the store or when an action is dispatched. 

In this case, listeners are regitsered using subscribe function and the listeners are notified/called when actions are `dispatch`ed. Let's add a test case to assert that listener(s) are called when actions are dispatched:

```js
// test/createStore.spec.js
describe('', () => {
    it('', () => {
        const listener = jest.fn()
        const store = createStore(() => {}, {})
        store.subscribe(listener)
        store.dispatch()
        expect(listener.mock.calls.length).toBe(1)
    })
})
```
There are a few things we need to do to make this test case pass. First of all, the `createStore` needs to have some place to store all the listeners that have been registered. Let’s add an array for them:

```js
// src/createStore.js
export function createStore(reducer, preLoadedState) {

    let currentReducer = reducer
    let currentState = preLoadedState
    let listeners = [] // holds all listeners
...
}
```
Now we can define the subscribe function. It’ll take the one function as argument, and store it in the listeners array:

```js
// src/createStore.js
...
    function subscribe(listener) {

        /* Since our listeners are of type array, 
        *  we simply push the arg listener to the array.
        */
        listeners.push(listener)
    }
...
```
Finally there is the dispatch function. For now, let's define a very simple version of it, which just iterates over all registered listeners and calls their listener functions:

```js
// src/createStore.js
...
    function dispatch(action) {
        for (var index = 0; index < listeners.length; index++) {
            const listener = listeners[index]
            listener()
        }
    }
...
```
The test passes but this versions of `subscribe` and `dispatch` isn't very useful yet.

#### Unsubscribing listeners

Whenever we subcribe to receive updates, we need to have a way from unsubscribing so as to no longer receive updates or to prevent memory leak.

We will make the subscribe function to return a function closure, which when invoked will unsubscribe the listener from the listeners array. 

Let's create a test case that checks our sunscribe function returns a function:

```js
// test/createStore.spec.js
describe('subscribe', ()=>{
    it('should return a function', ()=>{
        const listener = jest.fn()
        const store = createStore(() => {}, {})
        expect(store.subscribe(listener)).toBeDefined()
    })
})
```
To make this test pass, we make the subscribe function return a closure function:

```js
// src/createStore.js
...
    function subscribe(listener) {

        /* Since our listeners are of type array, 
        we simply push the arg listener to the array.
        */
        listeners.push(listener)

        /** we return an unsubscribe function back to the
         * user so they can remove their listener when they
         * want
         */
        return function unsubscribe() {
        }
    }
...
```
Now, we are sure our subcribe function returns an unsubscribe function. Next, we add a test case that asserts that a regstered listener is unsubscribed when the unsubscribing function is called:

```js
// test/createStore.spec.js
describe('', () => {
    it('', () => {
        const listenerA = jest.fn()
        const listenerB = jest.fn()
        const store = createStore(() => {}, {})
        const unsubscribeA = store.subscribe(listenerA)
        const unsubscribeB = store.subscribe(listenerB)
        store.dispatch({})
        expect(listenerA.mock.calls.length).toBe(1)
        expect(listenerB.mock.calls.length).toBe(1)
        unsubscribeA()
        store.dispatch({})
        expect(listenerA.mock.calls.length).toBe(1)
        expect(listenerB.mock.calls.length).toBe(2)
    })
})
```
We used `jest.fn()` to create two listeners, `listenerA` and `listenerB`. Then, we subscribed them to the store for changes, assigning their returning `unsubscribe` functions to `unsubscribeA` and `unsubscribeB` respectively.

`unsubscribeA` when called, removes listenerA from the listeners array and `unsubscribeB` also removes listenerB from the listeners array.

Next we ran dispatch function, which will make listenerA and listenerB to be called. Then, we unsubscribes listenerA and `dispatch`ed an action. Here, only listenerB calls should be `2` because listenerA has been unsubscribed from the store.

To make this work, we have to make the unsubscibe function remove the listener from the listeners array since we can reference the listener through closure.

Here is the new re-definition of `subscribe`:

```js
// src/createStore.js
...
    function subscribe(listener) {

        /* Since our listeners are of type array, 
        we simply push the arg listener to the array.
        */
        listeners.push(listener)

        /** we return an unsubscribe function back to the
         * user so they can remove their listener when they
         * want
         */
        return function unsubscribe() {
            /**
             * we are still in the same context, we can still acees
             * the listener arg. To unsbscribe, we use the splice method
             * to remove the listener from the listeners array.
             */
 ?          listeners.splice(listeners.indexOf(listener), 1)
        }
    }
...
```
Here, we were able to still reference the exact listener because of the use of closure. With the listener reference all we did to remove it from the array was just to get the index in the array and `splice` out the index using `Array`'s `splice` method.

#### Checking for non-function argument in subscribe

Subscribers to the store must be a function. Any other data types can't serve as a listener because it has to be executed to be notified of data change.

So, let's add test case that cheks the listener arg is a function:

```js
// src/createStore.spec.js
describe('subscribe', () => {
    it('checks the listener arg is a function', () => {
        const store = createStore(() => {}, {})
        expect(() => store.subscribe(3)).toThrow()
        expect(typeof store.subscribe(jest.fn())).toBe('function')
    })
})
```
All we need to make this test pass is to check the listener arg is of `type` function at the top of our subscribe function:

```js
// src/createStore.js
...
    function subscribe(listener) {

        // checks if the listener is a function
?      if (typeof listener != 'function') {
            throw new Error('listener must be a function')
        }
    }
...
```

#### Checking for non-object argument in dispatch

subscribe must take a function, dispatch must take an object. Actions disptached to the store are objects with property `type` which tells the store which action to take and an optional payload which holds tha value on how the state should be effected.

Let's add a test case that asserts that an object argument is to be dispatched:

```js
describe('dispatch', () => {
    it('checks the action arg is an object', () => {
        const store = createStore(() => {}, {})
        expect(() => store.dispatch(3)).toThrow()
        expect(() => store.dispatch('dispatch')).toThrow()
        expect(() => store.dispatch(() => {})).toThrow()
    })
})
```
To make this test pass, we add an object check at the top of dispatch function:

```js
// src/createStore.js
...
    function dispatch(action) {
        // We check if action is an object
?     if (typeof action != 'object') {
            throw new Error('action must be an object')
        }
    }
...
```

#### Returning the current state

We declared our `currentState` variable earlier. The variable holds the current state tree of an application. We will add functionality to `getState` function to return the current state of our app.

Here, add a test case that checks the `getState` function returns the current state:

```js
// src/createStore.spec.js
describe('getState', () => {
    it('should return an object', () => {
        const { getState } = createStore(() => {return {}}, {})
        const currentState = getState()
        expect(typeof currentState).toBe('object')
    })
    it('should return an array', () => {
        const { getState } = createStore(() => {return []}, [])
        const currentState = getState()
        expect(typeof currentState).toBe('object')
    })
})
```

To make this test pass, we simply return the `currentState` variable:

```js
...
    /** This function returns the current state of our app */
    function getState() {
        return currentState
    }
...
```

OK, our test passed because it was pre-loaded with an initial state. What happens when we call createStore without an initial state?:

```js
const store = createStore(()=>{})
const currentState = store.getState(); // returns undefined state

// src/createStore.spec.js
describe('getState', () => {
    it('should return the current state', () => {
        const { getState } = createStore(() => {return {}})
        const currentState = getState()
        expect(currentState).toBeDefined()
    })
})
```
It will return an undefined state. But, the current state of an app should be also defined. Our test case above will fail:

```sh
 FAIL  test\createStore.spec.js (7.998s)
  ? getState › should return the current state

    expect(received).toBeDefined()

    Expected value to be defined, instead received
      undefined

      145 |         const { getState } = createStore(() => {})
      146 |         const currentState = getState()
    > 147 |         expect(currentState).toBeDefined()
      148 |     })
      149 | })

      at Object.it (test/createStore.spec.js:147:30)
```

To make our test case pass. First we will make dispatch function call the `currentReducer` function and assign the return value to `currentState`. The `currentReducer` is a pure function that returns the new state of the store. The new state it returns will be stored as the new/current state.

When we create a store without an initial state, we need to dispatch an `INIT`ial action so that every reducer will return their initial state. With these we won't have an undefined `currentState`.

To make this work we have to touch our dispatch function. When an action is dispatched we call the reducer, passing in the action and the current state to get the new state. To implement it, we add this to our dispatch function:

```js
// src/createStore.js
...
    function dispatch(action) {
        // We check if action is an object
        if (typeof action != 'object') {
            throw new Error('action must be an object')
        }

        /* we pass in action and the current state, o get a new
        * state.
        * the new state is assigned to the currentState     
        * variable. Updating our store state.
        */ 
?       currentState = currentReducer(currentState, action)
        
        for (var index = 0; index < listeners.length; index++) {
            const listener = listeners[index]
            listener()
        }
        return action
    }
...
```
Next, as we said before we need to dispatch an `INIT` to get the current state of our reducer so as to stave off an undefined state error. We have to do that before the createStore function returns.

Let's add `dispatch({type: "INIT"})` before the return statement:

```js
// src/createStore.js
export function createStore() {
...
?  dispatch({ type: "INIT" })
    return {
        dispatch,
        getState,
        subscribe
    }
}
```
With these we get the current state whenever a store is created. Now, our test case passes.

With this we've now implemented the createStore function in full. Everything that Redux's createStore do, our store now
do too.

## combineReducers

We now have a full implementation of createStore, which forms the core of Redux state management system. We'll now turn our attention to `combineReducers` - combineReducers as the name implies merges all reducer functions into a single reducer function.

So far we've seen how we can use Redux to manage our app state. We have been using only one reducer function, which has been very easy to manage. But when our app grows complex, we have different data structures to manage. To keep the data updated, each data structure has to been managed by different functions.

To illustrate further, let's say we are to build a fruit-tracker app, that will keep inventory of friuts in our store. Let's narrow it down to two friuts, ie we only keep track of only two friuts in tha app (orange and mango). Here, we have two states to manage and keep tarck of. With these states, comes reducer functions for each state.

```js
// reducer for `orange` state.
function orangeReducer() {

}

// reducer for `mango` state.
function mangoReducer() {

}
```

Now, we can use combineReducers to combine these reducers into a single function which we can pass into createStore. The single reducer function calls its every reducer function composition with the passed in state tree and merges their result into one single state object.

Example:

```js
const reducer = combineReducers({ mango: mangoReducer, orange: orangeReducer })

// The reducer variable will contain these:

/**
* {
*    mango: [],
*    orange: []
* }
*
*/
```

You see dividing states and their reducers makes it simple to manage and keep track of different states in our app.

Now, let's begin creating our custom combineReducers function. We need combineReducers to return another function which we can pass the app state.

But, first the combineReducers take an object composed of reducer functions. These functions will be merged into a single function. 

Next, we make combineReducers return a function. To make sure combineReducers returns a function, add this test case:

```js
// test/combineReducers.spec.js
describe("combineReducers", () => {
    it("must return a function", () => {
        const reducer = combineReducers({ orange: () => {} })
        expect(typeof reducer).toBe('function')
    })
})
```
To make this test pass, we need combineReducers to return a function:

```js
/* This function takes reducers as arg, then combine 
the reducers into a single reducer function */
export function combineReducers(reducers) {
    return function combination(state = {}, action) {
    }
}
```

Our combineReducers is returning a new reducer function that takes two arguments: state and action. Next, we need the function returned to call each of the reducers in the `reducers` object. To do this, we are going to get the keys of the reducers in an array using Object.keys method. 
We will loop through the array and call each reducer by its key.
We will gather the results into a single state object, whose keys corresponds to the keys of the passed reducer functions.

Also, immutability is the watch word in Redux, to prevent mutation, we will check for state mutations using shallow checking. To achieve this, we will store the previous state of a reducer function and the current state which is derived from calling the reducer function. Then, we check for referential equality using the `===` operator. If the previous state doesn't point by reference to the next state, we know state has changed.

First, let's begin by adding a test case that asserts that combineReducers returns a reducer that maps the state keys to the given reducer.

```js
// test/combineReducers.js
describe("combineReducers", () => {
    it("must return a reducer that maps state keys to the given reducer", () => {
        const reducer = combineReducers({ orange: (state = [], action) => action.type === 'ADD_ORANGE' ? [...state, action.value] : state })
        const r = reducer({}, { type: 'ADD_ORANGE', value: 'green_orange' })
        expect(r).toEqual({ orange: ['green_orange'] })
    })
})
```
Here, we passed a reducer with key `orange` to combineRedcuers. The reducer has one action type `ADD_ORANGE` that pushes a string to the array state. The `reducer` is then called with action `ADD_ORANGE`. We check to see if the returned state object contains the same keys as it was given in the `reducer` object argument.

To make this test pass, we will loop through the keys of `reducer` and call each function object with the passed in state and action:

```js
// src/combineReducers
/* This function takes an reducers as arg, then combine 
the reducers into a single reducer function */
export function combineReducers(reducers) {

    let reducerKeys = Object.keys(reducers)

    return function combination(state = {}, action) {
        let _state = {}
        for (var index = 0; index < reducerKeys.length; index++) {
            let reducer = reducers[reducerKeys[index]]

            let state = reducer(state, action)

            _state[reducerKeys[index]] = state 
        }
        return _state
    }
}
```
We got the keys of `reducers` in an array, then `for`-looped through the array, getting each reducer function by its key and calling them with the passed in state and action. The result of each reducer function call is stored in an object with the corresponding key index of the reducer function which is returned as the new state.

With this our test passes because our function calls and returns a state object with the keys corresponding to the passed in reducers.

#### Immutability check

Redux operates on an immutable data structure. Its reducer functions are pure functions that return the same ouput given the same input.

Immutability is very essential in data management because it makes data handling safer, also time travel debugging can be implemented where we can jump to different states of an app.

Using immutable states, we can write code that can tell if the state has changed so that we won't recursively compare the state to check if something has changed. React-Redux's `connect` uses immutability to see if the `Wrapped` components need to re-render.

To implement immutabilty, we will compare the previous state of the reducer and the current state gotten from the call on the reducer using the `===` operator to check whether the state has changed.

We already have a `for-loop` iteration. So at each stage of iteration we perform a shallow check on the previous state and the current state. If the check fails we set a `hasChanged` flag to true.

After, the iteration is completed we will check the `hasChanged` flag. We will return the newly constructed if it is `true` or return the previous state if `false`.

We have a function that calls it child reducers and returns a state object. Let's write a test case that checks the combineReducer returns the same state when its reducers doesn't change the reference. 

```js
// test/combineReducers.spec.js
describe("combineReducers", () => {
    it("must return the same state if reducers doesn't change the state reference", () => {
        const reducer = combineReducers({ orange: (state = [], action) => action.type === 'ADD_ORANGE' ? [...state, action.value] : state })
        const initialState = reducer({}, { type: 'INIT' })
        expect(reducer(initialState, { type: 'NULL' })).toBe(initialState)
    })
})
```
Here, we called `combineReducers` with a reducer function `orange` which takes in a state and action, it adds an orange to a new Array when the acion type `ADD_ORANGE` is passed in. Further down, we called the resulting single function from combineReducers with an action `INIT`. This returns `state=[]` which we will be the initial state because no action type was matched.

Next, we expect another call with action type `NULL` to reference the initialState. This should be so because the action type didn't return a new state. Returning a new state breaks the reference.

Let's add another test case, that asserts that a state reference change in any of `combineReducers` child reducers returns a new state:

```js
// test/combineReducers.spec.js
describe("combineReducers", () => {
    it("must not return the same state if reducers change something", () => {
        const fruits = {
            orange: [],
            mango: []
        }
        const reducer = combineReducers({
            fruits(state = fruits, action) {
                if (action.type == 'ADD_ORANGE') {
                    return {...state, orange: [...state.orange, action.value] }
                } else {
                    return state
                }
            }
        })
        const initialState = reducer({}, { type: 'INIT' })
        const p = reducer(initialState, { type: 'ADD_ORANGE', value: 'red_orange' })
        expect(p).not.toBe(initialState)
    })
})
```

We setup our reducers with a `fruits` reducer. We've provided our fruits state object with an empty array. We used a `if-else` condition statement to account for when an action type is defined or not. If our reducer recieves an action other than adding an orange, it will return the original state object.

This is the same as above, we setup our initialState with a `INIT` action. It will return the state passed which we assign to `initialsState`, we is our initial state, next we call the reducer with action `ADD_ORANGE` which will return a new state. This new state should not be referential equal to the `initialState`.

To make this test pass, we are going to check the reference of our previos state and current state. We alreday have an iteration, that calls each reducers from its key, then builds a new state object and return it.

To implememt referentiality check, we store the previous state which we get foem the passed in state and the current state which we get from calling the reducer fucntion, we shallow check them using `===`, if it turns out false we set a `hasChanged` flag to true.

To start off, we define the `hasChanged` flag inside the `combination` function and set it to false:

```js
// src/combineReducers.js
...
    return function combination(state = {}, action) {
?      let hasChanged = false
    }
...
```
Next, we get the previous state from `state` and store it in `prevState` variable:

```js
// src/combineReducers.js
...
    return function combination(state = {}, action) {
        const newState = {}
        let hasChanged = false

        for (var index = 0; index < reducerKeys.length; index++) {
            const reducer = reducers[reducerKeys[index]]

            // store previous state
?          const prevState = state[reducerKeys[index]]
...
```
We got the previous state by getting it from the `state` using the current key in the iteration. 
Next, we get the current state. We already have a variable `state` which holds the state object gotten from the `redcuer` call. This variable will now be rewritten to `newState`, also the `reducer` will be passed the `prevState` variable. So we edit the `state` arg to `prevState`:

```js
// src/combineReducers.js
...
    return function combination(state = {}, action) {
        const newState = {}
        let hasChanged = false

        for (var index = 0; index < reducerKeys.length; index++) {
            const reducer = reducers[reducerKeys[index]]

            // store previous state
            const prevState = state[reducerKeys[index]]

            //store next state
?          const nextState = reducer(prevState, action)
...
```
OK, following the trend, we rename the previous `_state` object to `newState`. Also, we now construct the new state from `nextState`:

```js
// src/combineReducers.js
...
    return function combination(state = {}, action) {
        const newState = {}
        let hasChanged = false

        for (var index = 0; index < reducerKeys.length; index++) {
            const reducer = reducers[reducerKeys[index]]

            // store previous state
            const prevState = state[reducerKeys[index]]

            //store next state
            const nextState = reducer(prevState, action)

?          newState[reducerKeys[index]] = nextState
...
```
Now, we have the previous state and current state, we perform the shallow check on them and set the `hasChanged` flag to true if it didn't pass:

```js
// src/combineReducers.js
...
    return function combination(state = {}, action) {
        const newState = {}
        let hasChanged = false

        for (var index = 0; index < reducerKeys.length; index++) {
            const reducer = reducers[reducerKeys[index]]

            // store previous state
            const prevState = state[reducerKeys[index]]

            //store next state
            const nextState = reducer(prevState, action)

            newState[reducerKeys[index]] = nextState

?          nextState !== prevState ? hasChanged = true : null
...
```
We jsut used ternary operator to check if the check passes. If true `hasChanged` is set to true, if not nothing happens.

At the end of the iteration, we initially just returne dthe newly constructed state, but now, we are gong to check if the `hasChanged` flag is true to determine which state to send back. If the flag is true we send the newly constructed state `nextState`, if false we send the original state:

```js
// src/combineReducers.js
...
    return function combination(state = {}, action) {
        const newState = {}
        let hasChanged = false

        for (var index = 0; index < reducerKeys.length; index++) {
            const reducer = reducers[reducerKeys[index]]

            // store previous state
            const prevState = state[reducerKeys[index]]

            //store next state
            const nextState = reducer(prevState, action)

            newState[reducerKeys[index]] = nextState

            nextState !== prevState ? hasChanged = true : null
        }
?      return hasChanged ? newState : state
    }
...
```
Again, we used a ternary operator to determine which state to return. If all the reducer functions return the same state sent to it, then the current state object `state` is returned, not the newly constructed state `newState`. With this, our test cases passes.

# Bundling our Redux library with Rollup

Now we have a basic clone of the Redux library, we have to bundle it into a file that can be loaded in a browser or in a Node.js environment.

We are going to use `Rollup` to bundle our files. First, we install the library:

```sh
npm i rollup -D
```

The above command pulls in `Rollup` from `npmjs.com` registry into our project and registers it in our `package.json` as a dev dependency `-D` library.

To let Rollup know how to bundle our JS files, we got to create a configuration file, `rollup.config.js`:

```sh
touch rollup.config.js
```

We can actually pass our options to rollup using commands, but to save ourselves the stress of repetition, we created the `js` file, `rollup.config.js` to pass all our options to it. Upon execution, rollup reads the options in it and responds accordingly.

So, now we open up the `rollup.config.js` and add these following code:

```js
// rollup.config.js
const config = {
    input: 'src/index.js',
    output: {
        format: 'umd',
        name: 'redux'
    }
}
export default config
```

Let's go through the following configuartions to see what they does:

* __input__: The entrypoint of `Rollup`. This property should contain the path of the file that exports all the function you want to expose to the user.
* __output__: This object holds information on how `Rollup` should handle the output file.
* __output.format__: This specifies the bundling format to use (`amd`, `iife`, `umd`, `cjs`, `es` or `system`).
* __output.name__: The name by which other scripts can access our module. 

Let's add `src/index.js` file, here will form the base of all our Redux functions:

```sh
touch src/index.js
```

Next, we export all (`*`) our functions `createStore` and `combineReducers` so that `Rollup` can bundle them:

```js
// src/index.js
export * from './createStore'
export * from './combineReducers'
```

Let's add an NPM script `build` in our package.json so that we can convientiely bunlde our files from the terminal:

```json
// package.json
...
    "scripts": {
        "build": "rollup -c -o dist/redux.js",
...
    },
...
```

Rollup can actually be used in cmd and in JS (API). Here, we using the cmd option. Also we pass a couple of flags along with the rollup command. `-o` is the output folder where the final bundle will be kept, here we specified `dist/`. If the folder doesn't exist, `Rollup` will create it and store the bundled file in it as `redux.js`.

Now, let's invoke the `build` script to produce our bundle:

```sh
npm run build
```

Rollup bundles all the files in `src/` and stores it in `dist/`. We shpuld have a dist folder in our project directory with `react.js in it.

```
?  dist/
     +- redux.js
    src/
     +- createStore.js
     +- combineReducers.js
    test/
     +- createStore.spec.js
     +- combineReducers.spec.js
    .babelrc
    package.json
    rollup.config.js
```
Ta-da !! we have our bundle.

# Running an Example app

We now have our library bundle, let's see how it works by creating a simple Fruits app. This is to show that our Redux-library clone does the same thing as Redux itself.

To begin create folder `examples/` in your root directory. Add `examples/fruits.html`, it loads the `dist/redux.js` library:

```html
<!-- examples/fruits.html-->
<html>

<head>
    <title>Redux Fruits app</title>
    <script src="./../dist/redux.js"></script>
</head>

<body>
</body>
</html>
```

We created our example app in our project folder so that we can easily reference the path of our library. You can create yours anywhere in your system but you have to copy our library from the `dist` folder so you can reference it.

To deomnstate the use of both createStore and combineReducers fucmtions, we are going to write a simple Fruits app. 

We can add a new fruit (orange or mango or any other fruits), remove a fruit from the store or select a fruit.

We will have two states fruits and selectedFruits. fruits state will hold all fruits added to the store and selectedFruit will hold the fruit selected to be diplayed.

Let's begin by adding the following code to `examples/fruits.html`:

```html
<!-- examples/fruits.html-->
<html>

<head>
    <title>Redux Fruits app</title>
    <script src="./../dist/redux.js"></script>
</head>

<body>
    <div id="fruitsContainer" style="float:left">
        <h1 id="h1">Fruits app</h1>

        <div id="div">
            <input type="text" id="fruit" placeholder="Add Fruit" />
            <button onclick="addFruit()">Add Fruit</button>
        </div>

        <div>
            <h1>Fruits List</h1>
            <ul id="fruitList">

            </ul>
        </div>
    </div>
    <div id="selectedFruitContainer" style="float:left">
        <h1> Selected fruit: </h1>
        <b id="selectedFruit"></b>
    </div>
</body>
</html>
```

Looking at the above code, we have two container `div`s fruitsContainer and selectedFruitContainer. The former holds code for adding and removal of fruits on our store. The input tag holds the fruit name we type in and the button event `addTodo()` adds the name fo the fruit into our store. The `ul` element will hold the list of the fruits in our store.

The `b` element in `selectedFruitContainer` will display the fruit selected. Let's implement the store, first we add a `script` tag after `<div id="selectedFruitContainer">...</div>` , between `<script>` and `</scriipt>` is where we add our Js code. We define our reducers function selectedFruit and fruits:

```html
<!-- examples/fruits.html-->
...
    <script>
        function selectedFruit(state = '', action) {
            switch (action.type) {
                case 'SELECT_FRUIT':
                    return action.value
                    break;
                default:
                    return state
                    break;
            }
        }

        function fruits(state = [], action) {
            if (action.type == 'ADD_FRUIT') {
                return [...state, action.value]
            }
            if (action.type == 'REMOVE_FRUIT') {
                return state.filter(fruit => fruit != action.value)
            } else {
                return state
            }
        }
    </script>
</body>
</html>
```

Here, we have our two reducers, both takes in a state and action as parameters. selectedFruit has one action `SELECT_FRUIT` that adds the selected fruit to the state, fruits function has two action cases `ADD_FRUIT` and `REMOVE_FRUIT`. `ADD_FRUIT` adds a fruit to the store while `REMOVE_FRUIT` removes a fruit by its id from the store.

We have two reducers, so we will use our custom combinReducers to merge them into a single reducer function:

```html
        const reducer = redux.combineReducers({
            fruits,
            selectedFruit
        })
```

You remember the external property we set to `redux` in rollup.config .js file when bundling with Rollup? The external property exposed the value `redux` to be global so that we can reference or call our functions from it. That's how we were able to do `redux.combineReducers()`. OK, remember combineReducers takes in object of functions, thats why we passed our functions fruits and selectedFruit encased in an object. combineReducers merges them into a single function and returns it as `reducers`. So calling reducer with a state and action calls our fruits and selectedFruit with the parameters it got.

Next, we create our store:

```html
        var store = redux.createStore(reducer)
```

Here, we reference createStore using the global variable `redux` and call it with our reducer function. This returns our store, this store variable is composed of functions `getState`, `subscribe` and `dispatch`. With these functions we can manage our state.

Whenever an event is dispatched, state is changed, to keep up with the state changes we have to subscribe to the store to get updates. With the new updates we can render the data on our UI. We create a function render:

```html
function render() {

}
```
Here, we write the code that will update and re-render the data on our UI:

```html
        function render() {
            let {
                selectedFruit,
                fruits
            } = store.getState()

            let fruitList = document.getElementById('fruitList')
            let selectedFruit$ = document.getElementById('selectedFruit')
            let html = ''

            fruitList.innerHTML = ''
            selectedFruit$.innerHTML = ''

            // render todos
            for (var index = 0; index < fruits.length; index++) {
                html += `<li>${fruits[index]} | <button onclick="removeFruit('${fruits[index]}')">RemoveFruit</button> | <button onclick="selectFruit('${fruits[index]}')">Select Fruit</button></li>`
            }
            fruitList.innerHTML = html

            // render selectedFruit
            selectedFruit$.innerHTML = selectedFruit
        }
```
Here, we destructure todos and selectedFruit from the store. The render containers `fruitList` and `SelectedFruit` HTMLElement is stored, it will be used later to insert HTML code into them.

Next, we looped through the todos variable and generate HTML string for each todo. In each HTML string, we added buttons with texts `RemoveFruit`, `SelectFruit` this buttons have events that calls funcions `removeFruit()` and `selectFruit()`. `removeFruit()` which is passed the current fruit index removes the fruit from the store by dispatching `REMOVE_FRUIT` action, `selectFruit` also passed in the current fruit, dispatches the `SELECT_FRUIT` action. We use `innerHTML` property to insert the final HTML string into the `fruitList` HTMLElement.
After this, we also append the `selectedFruit` into the `selecteFruit` HTMLElement.

Now done with the `render` fucntion, we register it into our store as a subscriber, by calling the `store.subscribe` function passing it as an arg:

```html
        store.subscribe(render)
```

Next we define all events in our buttons:

```html
        function addFruit() {
            const fruit = document.getElementById('fruit').value
            store.dispatch({
                type: 'ADD_FRUIT',
                value: fruit
            })
        }

        function removeFruit(fruit) {
            store.dispatch({
                type: 'REMOVE_FRUIT',
                value: fruit
            })
        }

        function selectFruit(fruit) {
            store.dispatch({
                type: 'SELECT_FRUIT',
                value: fruit
            })
        }
```

At the end, our entire app code will look like this:

```html
<!-- examples/fruits.html-->
<html>

<head>
    <title>Redux Fruits app</title>
    <script src="./../dist/redux.js"></script>
</head>

<body>
    <div id="fruitsContainer" style="float:left">
        <h1 id="h1">Fruits app</h1>

        <div id="div">
            <input type="text" id="fruit" placeholder="Add Fruit" />
            <button onclick="addFruit()">Add Fruit</button>
        </div>

        <div>
            <h1>Fruits List</h1>
            <ul id="fruitList">

            </ul>
        </div>
    </div>
    <div id="selectedFruitContainer" style="float:left">
        <h1> Selected fruit: </h1>
        <b id="selectedFruit"></b>
    </div>

    <script>
        function selectedFruit(state = '', action) {
            switch (action.type) {
                case 'SELECT_FRUIT':
                    return action.value
                    break;
                default:
                    return state
                    break;
            }
        }

        function fruits(state = [], action) {
            if (action.type == 'ADD_FRUIT') {
                return [...state, action.value]
            }
            if (action.type == 'REMOVE_FRUIT') {
                return state.filter(fruit => fruit != action.value)
            } else {
                return state
            }
        }
        const reducer = redux.combineReducers({
            fruits,
            selectedFruit
        })
        var store = redux.createStore(reducer)

        function render() {
            let {
                selectedFruit,
                fruits
            } = store.getState()

            let fruitList = document.getElementById('fruitList')
            let selectedFruit$ = document.getElementById('selectedFruit')
            let html = ''

            fruitList.innerHTML = ''
            selectedFruit$.innerHTML = ''

            // render todos
            for (var index = 0; index < fruits.length; index++) {
                html += `<li>${fruits[index]} | <button onclick="removeFruit('${fruits[index]}')">RemoveFruit</button> | <button onclick="selectFruit('${fruits[index]}')">Select Fruit</button></li>`
            }
            fruitList.innerHTML = html

            // render selectedFruit
            selectedFruit$.innerHTML = selectedFruit
        }
        store.subscribe(render)

        function addFruit() {
            const fruit = document.getElementById('fruit').value
            store.dispatch({
                type: 'ADD_FRUIT',
                value: fruit
            })
        }

        function removeFruit(fruit) {
            store.dispatch({
                type: 'REMOVE_FRUIT',
                value: fruit
            })
        }

        function selectFruit(fruit) {
            store.dispatch({
                type: 'SELECT_FRUIT',
                value: fruit
            })
        }
    </script>
</body>

</html>
```
Now we have a fully functional app using our Redux-clone we implemented from scratch as the state management library!!!

# Conclusion

We have come a long way!! From introducing Redux concepts, Jest setup to running a demo app on our custom Redux library.

We learnt a lot during the course of this tutorial:

* Basic concepts of `Redux`.
* How to use `babel` to transpile `JS` code from `ES7` to `ES5/6`.
* Writing test using `Jest`.
* How to bundle JS code using `Rollup`.
* How `createStore` and `combineReducers` functions work and how to implement your own.
* How all the code we have written actually supports a Redux app.

Moreover, we now have a very rich and deep knowlegde of how Redux works.

# Complete `createStore` source code

```js
// src/createStore.js

export function createStore(reducer, preLoadedState) {

    let currentReducer = reducer
    let currentState = preLoadedState
    let listeners = [] // hold all listeners

    // we make sure the reducer is a function
    if (typeof reducer != 'function') {
        throw new Error('reducer must be a function')
    }

    /** This function returns the current state of our app */
    function getState() {
        return currentState
    }

    function subscribe(listener) {

        // checks if the listener is a function
        if (typeof listener != 'function') {
            throw new Error('listener must be a function')
        }

        /* Since our listeners are of type array, 
        we simply push the arg listener to the array.
        */
        listeners.push(listener)

        /** we return an unsubscribe function back to the
         * user so they can remove their listener when they
         * want
         */
        return function unsubscribe() {
            /**
             * we are still in the same context, we can still acees
             * the listener arg. To unsbscribe, we use the splice method
             * to remove the listener from the listeners array.
             */
            listeners.splice(listeners.indexOf(listener), 1)
        }
    }

    function dispatch(action) {
        // We check if action is an object
        if (typeof action != 'object') {
            throw new Error('action must be an object')
        }

        currentState = currentReducer(currentState, action) 

        for (var index = 0; index < listeners.length; index++) {
            const listener = listeners[index]
            listener()
        }
        return action
    }
    dispatch({ type: "INIT" })
    return {
        dispatch,
        getState,
        subscribe
    }
}
```

# Complete `combineReducers` source code

```js
// src/combineReducers.js

/* This function takes an reducers as arg, then combine 
the reducers into a single reducer function */
export function combineReducers(reducers) {

    var reducerKeys = Object.keys(reducers)

    return function combination(state = {}, action) {
        const newState = {}
        let hasChanged = false

        for (var index = 0; index < reducerKeys.length; index++) {
            const reducer = reducers[reducerKeys[index]]

            // store previous state
            const prevState = state[reducerKeys[index]]

            //store next state
            const nextState = reducer(prevState, action)

            newState[reducerKeys[index]] = nextState

            nextState !== prevState ? hasChanged = true : null
        }
        return hasChanged ? newState : state
    }
}
```