---
date: "2018-12-20T15:00:00+07:00"
title: "What is clossure?"
tags: ["javascript"]
---

In this section, I gonna talk about **clossure** in my view.
<!--more-->

### Table of content
- [Variable Scope](#variable_scope)
- [A closure is a persistent local variable scope](#local_scope)

#### Variable Scope <a name="variable_scope"></a>

Everyone knows that when we decalre a variable, then it will have a scope. Basically, it is only exist within the block or function in which you put them in.

```javascript
function(){
    const a = 10;
    console.log(a) // 10
}

console.log(a) // Error Unexpected token ( or Uncaught ReferenceError: a is not defined
```

So `a` got error because there is no variable `a` in current scope or parents scope. So, Here when we use `a` basically every languages will look for `a` at current scope then to parents scope until the root scope. And, If they cannot find any `a` then throw error like above.

Rewrite the code above:

```javascript
const a = 10;
function(){
    console.log(a) // 10
}

console.log(a) // 10
```

#### A closure is a persistent local variable scope <a name="local_scope"></a>

What does it mean?

This means that closure is a persistent scope which holds the local variable even after the code has executed. 

```javascript
const outer = () => {
    const a = 1;
    const inner = () => {
        console.log(a);
    }

    return inner;
}

const func = outer();
func(); // 1
```

Here we have a function inside a function. the `inner` function gains access to `outer` function local variable `a`.

Normally When `outer` was triggered then all its local variables should blow away. But in the example above the `func` is triggered and still give us value 1. This means that ***all of the variables that were in scope when inner was defined also persist***.

