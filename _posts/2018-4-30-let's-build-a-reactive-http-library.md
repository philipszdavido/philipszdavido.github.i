---
layout: post
title: Let's build a Reactive HTTP library for Angular
excerpt: In this article we will go over the concept of Reactive Programming, and in the process build a reactive Angular HTTP module like Angular's own.
post_url: https://cdn-images-1.medium.com/max/1600/1*BpaqVMW2RjQAg9cFHcX1pw.png
---


# Introduction

In this article we will go over the concept of Reactive Programming, and in the process build a reactive Angular HTTP module like Angular's own.

If you have tried your hands on Angular and used its HTTP module, you must have come across plethora of stuffs like `Observable`, `subscribe`.

> Angular uses Observables in many places when dealing with asynchronous code (e.g. HTTP requests)

Here, you will learn how all this RxJS stuff were incorparetd into it.

The Angular HTTP module is powered by [RxJS](https://github.com/ReactiveX/rxjs). RxJS is a JavaScript library for composing asynchronous and event-based programs by using observable sequences.

> __RxJS__ is a library for doing reactive programming in Javascript. It stands for __R__ eactive E __x__ tensions for __J__ ava __S__ cript.

Loosely speaking, `RxJS` gives us tools for working with `Observables`, which emit streams of data.

Before we dive into the nuts and bolts of how/what makes Angular HTTP library work, let's take a look at the core concepts of RxJS.

Let's go over the following topics:

* Reactive Programming
* Observable
* Observer

# Reactive programming

Reactive programming is a paradigm for software development that says that entire programs can be built uniquely around the notion of streams. 

In Angular, we can structure our application to use Observables as the backbone of our data architecture. Using Observables to structure our data is called Reactive Programming. But what are Observables, and Reactive Programming anyway? Reactive Programming is a way to work with asynchronous streams of data.

Observables are the main data structure we use to implement Reactive Programming. But I’ll admit, those terms may not be that clarifying. So we’ll look at concrete examples through the rest of this chapter that should be more enlightening.

# Observable

This represents a stream of events or data. It got its name from the Observer design pattern. The Observer design pattern is where an object maintains a list of observers and notifying them of any changes to state.

It's easy to implement a basic observer pattern in a few lines:

```js
class Observable {
    this.subscribers = []
    subscribe (subscriber) {
        this.subscribers.push(subscriber)
    }
    notify (msg) {
        this.subscribers.forEach((subscriber)=>{
            subscriber.next(msg)
        })
    }
}
```
The Observable keeps an array/list of subscribers, and will notify/`next` each of the subcsribers whenever there is a message, i.e when the Observable calls its `notify` method.

```js
// any listener will have the next method
const subscriber = {
    next: function(msg){
        console.log(`I received this message: ${msg}`)
    }
}

let observable = new Observable()
observable.add(subscriber)
observable.notify('message from the Observable')
```

When the program is run :

```sh
> I received this message: message from the Observer
```

This is a very simple illustration of the observer pattern but it shows how reactive programming basically works.

> Observable is a sequence of events/data over time

# Observer

Observer is a callback or list of callbacks that listens and knows how to deal with value delivered by the `Observable`. The put more simply, Observers are consumers of Observable.

> Observers are the listeners in the Observer pattern

Observers subcribe to Observable to receive values from it in a sequential manner. he thing there is that the Observer doen't request for them.

# Angular HTTP Module

In order to use Angular's `HTTP` module, we have to import it like this:

```ts
import { Http } from '@angular/http'
```
Then, inject it via Dependency Injection:

```ts
import { Http } from '@angular/http'

export class TodoService {
    constructor(private http: Http){}
}
```

To be finally be able to use the `Http` class, we have to import the `HttpModule` and add it to the `imports` array in `app.module.ts`.

```ts
// app.module.ts
import { HttpModule } from '@angular/http'

@NgModule({
    imports: [ HttpModule ]
})
```

With this we can perform different `CRUD`y requests to any resource, using the `Http`'s `get`, `post`, `delete`, and `put` methods.

Accessing a resource through any of this methods returns an Observable, unlike Promises returned by other `http` libraries (axios etc.):

```ts
import { Http } from '@angular/http'

export class TodoService {
    constructor(private http: Http) {}
    /** returns list of todos
    * [{title: 'Sweep the floor'},{title: 'Wash clothes'}]
    **/
    getTodoLists (): Observable {
        return this.http.get('/api/todos.json')
    }
}
```

With the Observable we can subcribe to it and receive the values of the reqeust:

```ts
import { TodoService } from './todoService'

@Component({
    selector: 'todo',
    template: '<div>{todos}</div>'
})

export class Todo {
    todos: Array<any>
    constructor(private todo: TodoService){
        this.todo.getTodoLists()
            .subscribe({
                todos => this.todos = todos,
                error => console.log('Error ocurred)
            })
    }    
}
```

Now, we have seen that the result of Http `POST`, `GET` methods returns an Observable. With this info, we can design our own HTTP module methods to return an Observable.

# Source Code

You can find the source code of this library [here](https://github.com/philipszdavido/ng.http).

# Project Setup

To recap on what we are going to build here. We are going to build a `http` library for Angular, it can be substitued for Angular's built-in `http` module and its going to be reactive.

This is an Angular Module, its setup will be different from an Angular app.

First, let's scaffold the project directory `ng-http`:

```sh
mkdir ng-http
cd ng-http
```
We now run the `npm init` command to generate `package.json`.

__NB:__ We could use ng-packagr to easily scaffold our Angular module, but I chose to do it manually so that we can also learn some tricks, and know how Angular module works.

```sh
npm init -y
```
This generates the `package.json` with our default credentials.

# Installing Dependencies

We are going to need several modules for our project:

* @angular/compiler
* @angular/compiler-cli
* @angular/core
* @angular/common
* @angular/platform-browser-dynamic
* core-js
* zone.js

These are the core modules we will use

```json
{
    "dependencies": {
        "@angular/common": "^5.0.0",
        "@angular/compiler": "^5.0.0",
        "@angular/core": "^5.0.0",
        "@angular/platform-browser": "^5.0.0",
        "@angular/platform-browser-dynamic": "^5.0.0",
        "core-js": "^2.4.1",
        "karma-typescript": "^3.0.12",
        "rxjs": "^5.5.2",
        "zone.js": "^0.8.14"
    },
    "devDependencies": {
        "@angular/cli": "1.5.4",
        "@angular/compiler-cli": "^5.0.0",
        "@types/jasmine": "~2.5.53",
        "@types/node": "~6.0.60",
        "jasmine-core": "~2.6.2",
        "karma": "~1.7.0",
        "karma-chrome-launcher": "~2.1.1",
        "karma-coverage-istanbul-reporter": "^1.2.1",
        "karma-jasmine": "~1.1.0",
        "karma-typescript-angular2-transform": "^1.0.2",
        "typescript": "~2.4.2"
    }
}
```
# Unit Testing with TestBed, Karma, and Jasmine

We are going to use

# Angular module setup



# Implementing HttpModule



# Implementing Http class



# Publishing our library



# Conclusion


