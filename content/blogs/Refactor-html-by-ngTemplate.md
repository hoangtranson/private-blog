---
date: "2019-01-15T15:00:00+07:00"
title: "Refactor HTML by ng-template"
tags: ["angular"]
---

### Table of content
- [Introduction ng-template](#introduction)
- [Problem](#problem)
- [Solution with ng-template](#solution)

#### Introduction ng-template<a name="introduction"></a>

The `<ng-template>` is an Angular element for rendering HTML. It is never displayed directly. In fact, before rendering the view, Angular replaces the `<ng-template>` and its contents with a comment.

see [here](https://angular.io/guide/structural-directives#the-ng-template) for more information.

#### Problem <a name="problem"></a>

In the angular component there are many pieces of code repeatedly that makes your template long. This causes us hard to maintain our code base.

I have refactored a lot of HTML template like that 250 lines or 500 lines to nearly 100 lines only by using `ng-template`.

#### Solution with ng-template <a name="solution"></a>

So here is what I did:

1.Find repeated code.

2.Put it into `ng-template`

    ```html
        <ng-template #carTemplate let-item>
            <div class="container">
                <div class="row">
                <div class="col-12">
                    <a class="content1" data-toggle="collapse" href="{{(globalUrl$ | async)}}/{{item.id}}">
                    <div class="row pl-5 pl-md-0">
                        <div class="col-10 px-0">
                            <span>{{ item.carName }}</span>
                        </div>
                    </div>
                    </a>
                </div>
                </div>
            </div>
        </ng-template>
    ```

3.Replace the current html code by our new template

    ```html
    ...
        <ng-container *ngTemplateOutlet="carTemplate; context: {$implicit: {carName: 'bmw', id: 1}}"></ng-container>
    ```
