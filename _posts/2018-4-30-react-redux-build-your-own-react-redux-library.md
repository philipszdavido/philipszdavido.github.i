---
layout: post
title: Build your own React-Redux library
---

During the course of this article we will be build our own `react-redux` library from beginning to the end.

# Build your own React-Redux library

**TL;DR:** This article is for experienced React devs who wants to know how `react-redux` works its nuts and bolts.

# How to Read this article

During the course of this article we will be build our own `react-redux` library from beginning to the end.

I highly encourage you to not only read the code and the text but also actually type it in and build your own `react-redux` library.

# Source Code

The source code implemented in this article can be found on GitHub, at [https://github.com/philipszdavido/_react-redux/](https://github.com/philipszdavido/_react-redux/)

# What is Redux?

`Redux` is a state management created by __Dan Abramov__ on June 2, 2015. It was heavily inspired by `Flux` and `Elm`. It helps you write applications that behave consistently, run in different environments and are easy to test.

`Redux` can be used together with `React`, `Angular`, vanilla or other JavaScript libraries.

`Redux` maintains the state of an entire app, in an immutable store or state tree which can't be changed be changed directly. Your application emits an action, that defines something that just happened that will affect the state. Reducers specify how to change the state when the action is received. It creates a new state and replaces it with the current state (Immutability).

# What is React-Redux?

`React-Redux` is the library that connects React to Redux. It seamlessly intregrates the store (single source of truth) into the context of every React component. `react-redux` provides useful functions and components that can be used to interact with our app store:

* __connect:__ This function actually `connect`s React components to Redux store.
* __Provider:__ This makes the store available to all React component tree.

# Project setup

We are going to set up our project with a build process and automated testing. There are super-awesome tools available for us to use. We are just going to pull them in and we are good to go.

# Install Node and NPM

`Node.js` will serve as our underlying test/build bed. `NPM` is the official `Node.js` package manager, we are going to use to pull in our needed Node modules from npmjs registry.

Make sure you have `Node.js` and `NPM` installed on your machine, anyway `NPM` comes bundled with `Node.js`.

Here are my own `Node.js` and `NPM` versions:
```sh
node -v
v6.10.0

npm -v
3.10.10
```

Yours will be different, depending on the versions installed on your machine. The versions aren't important, the important thing is that the commands work.

# Scaffold Project

We are going to set up our project directory. It will contain `src` and `test` folders. Let's run the following commands to begin:

```sh
mkdir react-redux
cd react-redux
mkdir test && mkdir src
```

