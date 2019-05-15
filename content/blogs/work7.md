+++
showonlyimage = false
draft = false
image = "img/portfolio/content7/css.jpg"
date = "2018-04-9T12:00:00+07:00"
title = "CSS Specificity"
weight = 4
+++

This term came to me every single time I edited my css.
<!--more-->

### Table of content

- [What is CSS Specificity?](#css_specificity)
- [Specificity hierarchy](#specificity_hierarchy)
- [Specificity Rules](#specificity_rules)

#### What is CSS Specificity? <a name="css_specificity"></a>

To me, CSS Specificity is so familiar to Frontend guys since we use it often. Specificity determines which css rule is applied to a specific HTML element at that moment on browsers.

> “Specificity is a type of weighting that has a bearing on how your cascading style sheet (CSS) rules are displayed.”

It is true that every single selector has its own specificity and if two selectors apply to the same element, the one with higher specificity wins.

#### Specificity hierarchy <a name="specificity_hierarchy"></a>

Every selector has its place in the specificity hierarchy. There are four distinct categories which define the specificity level of a given selector:

1. Inline styles (Presence of style in document). An inline style lives within your XHTML document. It is attached directly to the element to be styled. E.g. ```<h1 style="color: #fff;">```
2. IDs (# of ID selectors). ID is an identifier for your page elements, such as #div.
3. Classes, attributes and pseudo-classes (# of class selectors). This group includes .classes, [attributes] and pseudo-classes such as :hover,:focus etc.
4. Elements and pseudo-elements (# of Element (type) selectors). Including for instance :before and :after.

Here is some examples.

1. A selector is the element that is linked to a particular style. E.g. p in

    ```css
        p { padding: 10px; }
    ```

2. A class selector is a selector that uses a defined class (multiple per page). E.g.p.section in

    ```css
        p.section { padding: 10px; }
    ```

3. An ID selector is a selector that uses an individually assigned identifier (one per page). E.g. p#section in

    ```css
        #section { padding: 10px; }
    ``` 

4. A contextual selector is a selector that defines a precise cascading order for the rule. E.g. p span in

    ```css
        p span { font-style: italic; }
    ``` 

    defines that all span-elements within a p-element should be styled in italics.

5. An attribute selector matches elements which have a specific attribute or its value. E.g. p span in

    ```css
        p[title] { font-weight: bold; }
    ``` 

    matches all p-elements which have a title attribute.

6. Pseudo-classes are special classes that are used to define the behavior of HTML elements. They are used to add special effects to some selectors, which are applied automatically in certain states. E.g. :visited in

    ```css
        a:visited { text-decoration: underline; }
    ```

7. Pseudo-elements provide designers a way to assign style to content that does not exist in the source document. Pseudo-element is a specific, unique part of an element that can be used to generate content "on the fly", automatic numbering and lists. E.g. :first-line or :after in

    ```css
        p:first-line { font-variant: small-caps; }
        a:link:after { content: " (" attr(href) ")"; }
    ```

#### Specificity Rules <a name="specificity_rules"></a>

##### How to measure specificity? a good question to start with.

Memorize how to measure specificity. “Start at 0, add 1000 for style attribute, add 100 for each ID, add 10 for each attribute, class or pseudo-class, add 1 for each element name or pseudo-element. So in

```css
    body #content .data img:hover
```

The specificity value would be 122 (0,1,2,2 or 0122): 100 for #content, 10 for .data, 10 for :hover, 1 for body and 1 for img.”

Specificity Examples

It’s easier to calculate the specificity using the first method. Let’s find out, how it actually is done.

|                       |                                       |
| :-----------------:   | :---------------------------------:   |
|       `* { }`         |                  0                    |
|      `li { }`         |           1 (one element)             |
| `li:first-line { }`   | 2 (one element, one pseudo-element)   |
| `ul li { }`           |2 (two elements)                       |
| `ul ol+li { }`        |3 (three elements)                     |
| `h1 + *[rel=up] { }`  |11 (one attribute, one element)        |
| `ul ol li.red { }`    |13 (one class, three elements)         |
| `li.red.level { }`    |21 (two classes, one element)          |
| `style=”"`            |1000 (one inline styling)              |
| `p { }`               |1 (one HTML selector)                  |
| `div p { }`           |2 (two HTML selectors)                 |
| `.sith`               |10 (one class selector)                |
| `div p.sith { }`      |12 (two HTML selectors and a class selector)|
| `#sith`               |100 (one id selector)                  |
| `body #darkside .sith p { }`|112 (HTML selector, id selector, class selector, HTML selector; 1+100+10+1)|

Easy right?

##### Basic Principles

Equal specificity: the latest rule is the one that counts. “If you have written the same rule into your external style sheet twice, than the lower rule in your style sheet is closer to the element to be styled, it is deemed to be more specific and therefore will be applied.
When selectors have an equal specificity value, such as

```css
#content h1 { padding: 5px; } 
#content h1 { padding: 10px; }
```

where both rules have the specificity 0, 1, 0, 1, the latter rule is always applied.

##### Specificity Rules

1. Rules with more specific selectors have a greater specificity. The more specific the references are in your selector, the greater the specificity for the rule.
2. ID selectors have a higher specificity than attribute selectors. For example, in HTML, the selector #p123 is more specific than [id=p123] in terms of the cascade.

    ```css
    Case A: a#a-02 { background-image : url(n.gif); }
    Case B: a[id="a-02"] { background-image : url(n.png); }
    ```
    the first rule (A) is more specific than the second one (B).

3. Contextual selectors are more specific than a single element selector. It also holds for other selectors involving more than one HTML element selector.
4. The embedded style sheet is closer to the element to be styled. So in the following situation CSS:

    ```css
    #content h1 { padding: 5px; }
    ```

    ```html
    <style type="text/css"> #content h1 { padding: 10px; } </style>
    ```

    the latter rule will be applied.

5. The last rule defined overrides any previous, conflicting rules. For example, given these two rules

    ```css
    p { color: red; background: yellow } 
    p { color: green }
    ```
    paragraphs would appear in green text. They would also have a yellow background however, because the first rule is not completely negated. 

6. A class selector beats any number of element selectors. .introduction beats html body div div h2 p.
7. The universal selector has a specificity of 0, 0, 0, 0. *, body * and similar selectors have a zero specificity. Inherited values also have a specificity of 0, 0, 0, 0.

##### Specificity Example

```css
A:  h1 {} 
B:  #content h1 {}
C:  <div id="content">
        <h1 style="color: #fff">Headline</h1>
    </div>
```

The specificity of A is 0,0,0,1 (one element), the specificity of B is 0,1,0,1 (one ID reference point and one element), the specificity value of C is 1,0,0,0, since it is an inline styling.

Since

0001 = 1 < 0101 = 101 < 1000,

the third rule has a greater level of specificity, and therefore will be applied. If the third rule didn’t exist, the second rule would have been applied.

#### References:

1. [CSS Specificity - w3schools](https://www.w3schools.com/css/css_specificity.asp)

2. [How is specificity calculated?](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity)

3. [Specifics on CSS Specificity](https://css-tricks.com/specifics-on-css-specificity/)