+++
showonlyimage = false
draft = false
image = "img/portfolio/content6/js_trick.jpg"
date = "2018-04-5T12:00:00+07:00"
title = "Javascript Trick part 1"
weight = 2
+++

In this section, I gonna talk about time-saving Javascript techniques that I used to apply to my code.
<!--more-->

### Table of content

- [Clearing or truncating an array](#truncating_array)
- [Simulating named parameters with object destructuring](#simulating_named_param)
- [Object destructuring for array items](#obj_destructuring)
- [Switch with ranges](#switch_range)
- [Creating pure objects](#create_pure_func)
- [Removing duplicate items from an array](#remove_duplicated_item_array)
- [Flattening multidimensional arrays](#flattening_multidimensional)

#### Clearing or truncating an array <a name="truncating_array"></a>

An easiest way to clear or truncate an array without reassigning is to use **length** property.

![clearing or truncating an array][1]

be careful when we assign new value to length.

```Javascript
const arr = [11, 22, 33, 44, 55, 66];
arr.length = 10;
console.log(arr); //=> [11, 22, 33, 44, 55, 66, empty × 4]
```

#### Simulating named parameters with object destructuring <a name="simulating_named_param"></a>

Remember topic [Clean Code Javascript](https://hoangtranson.github.io/my-blog/portfolio/work5/#functions) I mentioned to **Function arguments** we should only have one or two arguements in a function since it is hard to maintain code during time.

```Javascript
function doSomething({ foo = 'Hi', bar = 'Yo!', baz = 13 }) {
  //...
}
```

If you need to make the arguements object optional, it’s very simple, too:

```Javascript
function doSomething({ foo = 'Hi', bar = 'Yo!', baz = 13 } = {}) {
  //...
}
```

#### Object destructuring for array items <a name="obj_destructuring"></a>

This is common technique that I apply in my code last year and now in 2018. There are too many properties in Redux state and I just take the properties that I need to have at current screen. So I use this to take things that I really need to display on screen.

```Javascript
const { auth: { identity } } = $ngRedux.getState();
```

I tried to get identity information from auth in app state instead of I get all messy stuff.

#### Switch with ranges <a name="switch_range"></a>

I read this code from stackoverflow and tried to apply it sometimes in my code. It works!

```Javascript
function calculateStudentRange(point) {
  let result;
  switch (true) {
    case (point >= 80):
      result = 'excellent';
      break;
    case (point >= 50 && point < 80):
      result = 'ok';
      break;
    default:
      result = 'week';
  }
  return result;
}
```

#### Creating pure objects <a name="create_pure_func"></a>

In real life, I can create pure object which won’t inherit any property or method from Object such as constructor or toString()

```Javascript
const pureObject = Object.create(null);
```
cool huh?

#### Removing duplicate items from an array <a name="remove_duplicated_item_array"></a>

This is reason why I love Javascript so much.

```Javascript
const removeDuplicateItems = arr => [...new Set(arr)];
removeDuplicateItems([42, 'foo', 42, 'foo', true, true]);
//=> [42, "foo", true]
```

#### Flattening multidimensional arrays <a name="flattening_multidimensional"></a>

```Javascript
const arr = [11, [22, 33], [44, 55], 66];
const flatArr = [].concat(...arr); //=> [11, 22, 33, 44, 55, 66]
```
The above trick will only work with bidimensional arrays.

For more than 2 dimensions array:

```Javascript
function flattenArray(arr) {
  const flattened = [].concat(...arr);
  return flattened.some(item => Array.isArray(item)) ? 
    flattenArray(flattened) : flattened;
}

const arr = [11, [22, 33], [44, [55, 66, [77, [88]], 99]]];
const flatArr = flattenArray(arr); 
//=> [11, 22, 33, 44, 55, 66, 77, 88, 99]
```



[1]: /my-blog/img/portfolio/content6/img1.jpg