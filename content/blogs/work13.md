+++
showonlyimage = false
draft = false
image = "img/portfolio/content13/pure-fn.png"
date = "2018-04-5T12:00:00+07:00"
title = "Pure function in Javascript"
weight = 3
+++

Pure functions are essential for a variety of purposes such as functional programming, reliable concurrency, and Redux app.
<!--more-->

![pure functions][1]

### Table of content

- [What is pure function?](#pure_function)
- [Pure function the good part](#pf_good_part)

#### What is pure function? <a name="pure_function"></a>

To answer question above, we have to understand what function is in advanced.

```
input --> function --> output
```

As you see, Function take input and give us output right? so simple isn't it?

In reality we can do many stuff with function such as mapping, procedures, and I/O.

A **Pure function** is a function with the same input we get the same output.

As a developer, I think we should favor pure function when we can use it to develop program. Pure function take some input and give some output with that input, so we can reuse it in many places in our program. The primary principle in computer science I think are KISS(Keep it simple stupid). Pure function is that stupid thing we can do during implementation.

We get benefits from using pure function such as indepenedent, easy to move around, refactor, re-organize, flexible and adaptable to extend in the future.

#### Pure function the good part <a name="pf_good_part"></a>

##### Given the Same Input, Always Return the Same Output

You may think that every single method will generate the same output from the same input, but actually it is not. So weird huh?

Assume that we have function double which always double itput as output.

``` Javascript
 function double(num){
     return num*2;
 }
```

So we call it three times

``` Javascript
double(2); // 4
double(2); // 4
double(2); // 4
```

The input is 2. we always receive back the value 4. So double is a pure function.

Consider this:

``` Javascript
Math.random(); // => 0.490327772182118
Math.random(); // => 0.6670939642421154
Math.random(); // => 0.11873054846370912
```

The same input we receive different output in this case. So Math.random() is not pure or impure function.

To sum up, A function is only pure if, given the same input, it will always produce the same output.


[1]: /my-blog/img/portfolio/content13/pure.jpeg