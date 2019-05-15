+++
showonlyimage = false
draft = false
image = "img/portfolio/content14/curry.png"
date = "2018-29-5T12:00:00+07:00"
title = "Functional Programming Currying - Part 1"
weight = 3
+++

In this section, I gonna talk about **Currying** in functional programming.
<!--more-->

![currying representation][1]

### Table of content

- [Introduction](#introduction)
- [Real problems solving](#real_problem_solving)

####  Introduction<a name="introduction"></a>

**Can't Live If Livin' Is without You** there are certain things one can live without until one acquires them. That is life.

So, the concept is simple: You can call a function with fewer arguments than it expects. It returns a function that takes the remaining arguments.

```Javascript
const add = x => y => x + y;
const increment = add(1);
const addTen = add(10);

increment(2); // 3
addTen(2); // 12
```

The piece of code above take one argument and return a function. By calling this way, the return function will remember state from its parents via the closure. Calling it with full arguments at the same time is a bit of pain. However, we can use special helper function called **curry** in order to make defining and calling functions like this easier.

```Javascript
// curry :: ((a, b, ...) -> c) -> a -> b -> ... -> c
const curry = (fn) => {
  const arity = fn.length;

  return function $curry(...args) {
    if (args.length < arity) {
      return $curry.bind(null, ...args);
    }

    return fn.call(null, ...args);
  };
};
```

####  Real problems solving<a name="real_problem_solving"></a>

from that awesome curry function, we can implement some functions below:

```Javascript
const replace = curry((what, replacement, s) => s.replace(what, replacement));
const upperCase = l => l.toUpperCase();
const lowerCase = l => l.toUpperCase();
const replaceFirstCharTo = replace(/(^|\s)\S/g); // (replacement, s) => s.replace(/[aeiou]/ig, replacement)
const capitalFirstChar = replaceFirstCharTo(upperCase); // s => s.replace(/[aeiou]/ig, upperCase)

capitalFirstChar(lowerCase('functional progRamming CuRrying.')); // "Functional Programming Currying."
```

This one, I tried to capital first char in each letter. This way will help us to reuse code in the future and clear to developer why upon use.

Or look at another example:

```Javascript
const match = curry((what, s) => s.match(what));
const replace = curry((what, replacement, s) => s.replace(what, replacement));
const filter = curry((f, xs) => xs.filter(f));
const map = curry((f, xs) => xs.map(f));

match(/r/g, 'hello world'); // [ 'r' ]

const hasLetterR = match(/r/g); // x => x.match(/r/g)
hasLetterR('hello world'); // [ 'r' ]
hasLetterR('just j and s and t etc'); // null

filter(hasLetterR, ['rock and roll', 'smooth jazz']); // ['rock and roll']

const removeStringsWithoutRs = filter(hasLetterR); // xs => xs.filter(x => x.match(/r/g))
removeStringsWithoutRs(['rock and roll', 'smooth jazz', 'drum circle']); // ['rock and roll', 'drum circle']

const noVowels = replace(/[aeiou]/ig); // (r,x) => x.replace(/[aeiou]/ig, r)
const censored = noVowels('*'); // x => x.replace(/[aeiou]/ig, '*')
censored('Chocolate Rain'); // 'Ch*c*l*t* R**n'
```

[1]: /my-blog/img/portfolio/content14/bg.jpg