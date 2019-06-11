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
- [Configuration](#configuration)

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

To me, Core module is dedicated to singleton provider provided in root injector.

```
core/
├── constants (contains consstants for application)
├── data (contains data in json for testing)
├── directives (contains custom directives)
├── interceptors (handle interceptors for http request)
├── interfaces (contains model and interfaces)
├── pipes (contains custom pipes)
├── services (contains services)
├── utils (contains utils function)
├── public-api.ts (public files)
└── core.module.ts
```

public-api.ts sample

```typescript
export * from './services/...';
export * from './constants/...';
export * from './interfaces/...';
export * from './pipes/...';
```

So In the project we can import these services, constants, interfaces, pipes from public api (not core module)
##### Feature module

Feature module is a module composed of related components, constants, directives, pipes, constants, routing etc.

A feature module should never communicate with others directly. They will communicate via core module or store module.

```
features/
├── feature-1/
│   ├── components/ (presentational components)
│   │   ├── component-1/
│   │   │   ├── component-1.component.html
│   │   │   ├── component-1.component.scss
│   │   │   ├── component-1.component.ts
│   │   └── ...
│   ├── containers/ (container components that CAN'T be routed to)
│   │   ├── container-1/
│   │   │   ├── container-1.container.html
│   │   │   ├── container-1.container.scss
│   │   │   └── container-1.container.ts
│   │   └── ...
│   ├── helpers/ (pure helper functions grouped by related functionalities)
│   │   ├── h1.helpers.ts
│   │   └── ...
│   ├── types/ (TypeScript types, interfaces and classes)
│   │   ├── type-1.ts
│   │   └── ...
│   ├── views/ (container components that CAN be routed to)
│   │   ├── view-1/
│   │   │   ├── view-1.view.html
│   │   │   ├── view-1.view.scss
│   │   │   └── view-1.view.ts
│   │   └── ...
│   ├── feature-1-routing.module.ts
│   ├── feature-1.configs.ts
│   ├── feature-1.constants.ts
│   └── feature-1.module.ts
└── feature-2/
    └── ... (same as above)
```

So In the feature 1 module, it should be like

```typescript
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';

import { View1 } from './views/view-1.view';
import { View2 } from './views/view-2.view';

const routes: Routes = [{
  path: 'feature1',
  children: [
      { path: '',   component: View1 },
      { path: '',   component: View2 },
      ...
  ]
}];

// { path: '**', component: LandingComponent }

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class Feature1Module { }
```
##### Shared module

SharedModule is a place to store all the reuseable presentational components and abstraction class used be features. Componnents and Abstraction should not depend on other module and should not have business logic inside like we mentioned in presentational components concept above.

```typescript
shared/
├── abstract/
│   ├── abstract1.abstract.ts
│   └── ...
├── components/
│   ├── component1/
│   │   ├── component1.html
│   │   ├── component1.scss
│   │   ├── component1.ts
│   └── ...
└── shared.module.ts
```
##### Store Module

This is a module where we centralize our data into single source of truth.

```typescript
store/
├── business1/
│   ├── business1.action.ts
│   └── business1.state.ts
├── ...
└── store.module.ts
```

The `store.module.ts` is a file that we configure more than a state there. We can add debug mode in order to debug state on console log for develop environment. We can add router plugin to control navigation via state and we can add plugin for form too to centralize the form model to state.

```typescript
    import { NgModule } from '@angular/core';
    import { NgxsModule } from '@ngxs/store';
    import { CommonModule } from '@angular/common';
    import { NgxsFormPluginModule } from '@ngxs/form-plugin';
    import { NgxsLoggerPluginModule } from '@ngxs/logger-plugin';
    import { NgxsReduxDevtoolsPluginModule } from '@ngxs/devtools-plugin';
    import { NgxsRouterPluginModule } from '@ngxs/router-plugin';

    import { environment as env } from '../environments/environment';
    import { UserState } from './user/user.state';

    @NgModule({
    imports: [
        CommonModule,
        NgxsFormPluginModule.forRoot(),
        NgxsLoggerPluginModule.forRoot({ logger: console, collapsed: false }),
        NgxsReduxDevtoolsPluginModule.forRoot({ disabled: env.production }),
        NgxsRouterPluginModule.forRoot(),
        NgxsModule.forRoot([UserState], { developmentMode: !env.production })
    ],
    exports: [
        NgxsFormPluginModule,
        NgxsLoggerPluginModule,
        NgxsReduxDevtoolsPluginModule,
        NgxsModule
    ]
    })
    export class NgxsStoreModule {}
```

#### Configuration <a name="configuration"></a>

You use the Angular CLI to create projects, generate application and library code, and perform a variety of ongoing development tasks such as testing, bundling, and deployment.

Install the Angular CLI globally.

To install the CLI using npm, open a terminal/console window and enter the following command:

```bash
npm install -g @angular/cli
```

Create a workspace and initial application

```bash
ng new my-app
```

The Angular CLI includes a server, so that you can easily build and serve your app locally.

```bash
cd my-app
ng serve
```

Install store

```bash
npm install @ngxs/store --save
```

create `NgxsModule` and then in `app.module.ts`, import the `NgxsModule`:

```typescript
    import { NgModule } from '@angular/core';
    import { NgxsModule } from '@ngxs/store';
    import { environment as env } from '../environments/environment';

    import { UserState } from './user/user.state';

    @NgModule({
        imports: [
            CommonModule,
            NgxsModule.forRoot([UserState], { developmentMode: !env.production })
        ],
        exports: [
            NgxsModule
        ]
    })
    export class NgxsStoreModule {}
```

Then run `ng g module module-name --routing=true` to create module src/app/module-name/module-name.module.ts for shared, core, and feature module.

And run `ng g component module-name/component-name --module=module-name` to create component src/app/module-name/landing/landing.component.ts


[1]: /my-blog/img/portfolio/one-way-data-flow.png