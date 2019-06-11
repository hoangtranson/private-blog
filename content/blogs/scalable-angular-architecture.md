---
date: "2019-06-01T15:00:00+07:00"
title: "Design a Scalable Angular Architecture"
tags: ["angular"]
---

### Table of content
- [Introduction](#introduction)
- [State Management](#state_management)
- [Component architecture](#component_architecture)
- [One-way data flow](#one_way_data)
- [Modules](#modules)

#### Introduction <a name="introduction"></a>

This article regarding to how to create a scalable angular project based on my experience from serveral angular projects.

#### State Management <a name="state_management"></a>

An effective state management is a crucial successfully key in large frontend applications. Normmally We call it is a store and an observable type that implemented using Rxjs and [Redux](https://redux.js.org/) Architecture.

Basically I am using [NgXs](https://ngxs.gitbook.io/ngxs/getting-started) to manage my store in project, But, You can consider another one is [NGRX](https://ngrx.io/) which provides reactive state management for angular apps inspired by Redux.


#### Component architecture <a name="component_architecture"></a>

In a big application, there are so many components and the code is so complicated and hard to maintain if we did not separate component by the logic.

Personally, I devided components into 2 types. One is presentational components and the other is container components.

##### Presentational components

Presentational components are responsible for rendering the current state (@input) into user interface that enables user to interact (@output) with the application.

Here are what we should put inside the presentational components:

1. Flag or bolean value that use to show or hide a section of UI.

2. Some computed methods to show it on UI.

3. Event handlers for user interaction such as click or double click

4. Method to throttle user input on form or button (usually I use this together with event emitter)

Here are what we should not put in:

1. No business logic

2. No direct app state update

3. No API call 

##### Container components

Container components are components which binds observable stores and other business logic to presentational components.

We can call it smart components because the component itself know how state is structured and what action from store should dispatch based on the output callback by presentational components.

#### One-way data flow <a name="one_way_data"></a>

One-way data flow is a great pattern to ensure state is consistent amongs components in application.

This is one-way data flow diagram in Angular.

![one way data flow in angular][1]

Here is breakdown:

1. container component  dispatch some actions to get data from server to state.

2. The updated state is pushed to all subscribers (container components).

3. The local state from component will compute again if needed.

4. Data will bind to presentational components via `@input()` with `async` pipe for observable value here is selector.

5. Presentational components will compute local state from `@input()` or `onChange()` lifecycle and re-render the UI again.


When user interact the interface:

1. Presentational components will emit event to container component via `@output()`.

2. Container component will do some business logic and dispatch an action to store plus payload. Payload here is parameter based on data emitting from event.

3. Store will update state based on payload or call API then update state based on http response.

4. The updated state is pushed to all subscribers.

5. Data will bind to presentational components via `@input()` with `async` pipe for observable value here is selector.

6. Presentational components will compute local state from `@input()` or `onChange()` lifecycle and re-render the UI again.

To sum up, There is always just one source of truth in an application and there is only one way to update to the state via dispatching action to store.

#### Modules <a name="modules"></a>

##### App root

Every application has at least one Angular module, the root module that you bootstrap to launch the application. By convention, it is usually called **AppModule**.

```javascript
    /* JavaScript imports */
    import { BrowserModule } from '@angular/platform-browser';
    import { NgModule } from '@angular/core';
    import { FormsModule } from '@angular/forms';
    import { HttpClientModule } from '@angular/common/http';
    
    import { AppComponent } from './app.component';
    
    /* the AppModule class with the @NgModule decorator */
    @NgModule({
    declarations: [
        AppComponent
    ],
    imports: [
        BrowserModule,
        FormsModule,
        HttpClientModule
    ],
    providers: [],
    bootstrap: [AppComponent]
    })
    export class AppModule { }
```
##### Core module


##### Business module
##### Shared root
##### Layout root
##### Views root

[1]: /my-blog/img/portfolio/one-way-data-flow.png