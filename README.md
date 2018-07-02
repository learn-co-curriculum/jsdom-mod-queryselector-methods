# JavaScript Node Search Power Tools

## Problem Statement

One of the most essential skills in our web development toolbox is finding
elements in the DOM.

While `document.getElementById()` and `document.getElementsByClassName()` are
good, we can search even more effectively when we leverage document structure
(tag, `id`, `class`) **and** the tree structure of the DOM.

Two methods that provide a huge leap forward in quick finding of elements are
`document.querySelector()` and `document.querySelectorAll()`. In this lesson
we'll see how to use these general-purpose superbeasts of element location.

## Objectives

1. Use `document.querySelector()` and `document.querySelectorAll()` to find nested nodes
2. Change value of targeted DOM nodes

## Introduction

To practice finding elements in the DOM, we're going to make use of two methods
that are useful for navigating the DOM: `document.querySelector()` and
`document.querySelectorAll()`.

### `querySelector()`

`querySelector()` takes one argument, a string of [selectors][], and returns
the first element that matches these selectors. For those familiar with CSS,
descending this tree with CSS selector symbols as way markers will feel
familiar.

Given a document like

``` html
<body>
  <div>
    Hello!
  </div>

  <div>
    Goodbye!
  </div>
</body>
```

If we called `document.querySelector('div')`, the method would return the first
`div` (whose content is "Hello!").

Selectors aren't limited to tag names, though (otherwise why not just use
`document.getElementsByTagName('div')[0]`?). We can get _very_ fancy.

``` html
<body>
  <div>
    <ul class="ranked-list">
      <li>1</li>
      <li>
        <div>
          <ul>
            <li>2</li>
          </ul>
        </div>
      </li>
      <li>3</li>
    </ul>
  </div>

  <div>
    <ul class="unranked-list">
      <li>6</li>
      <li>2</li>
      <li>
        <div>4</div>
      </li>
    </ul>
  </div>
</body>
```

```javascript

// get <li>2</li>
const li2 = document.querySelector('ul.ranked-list li ul li')

// get <div>4</div>
const div4 = document.querySelector('ul.unranked-list li div')

```

In the above example, the first query says, "Starting from `document` (the
object we've called `querySelector()` on), find a `ul` with a `className` of
`ranked-list` (the `.` is for `className`).

_Then_ find an `li` that is a child of that `ul`. Then find a `ul` that is a
child (but not necessarily a direct descendant) of that `li`. Finally, find an
`li` that is a child of that (second) `ul`."

**NOTE:** The HTML property `class` is referred to as `className` in JavaScript.
It's...unfortunate.

What, then, does the second call to `querySelector()` say? Puzzle it out for a
bit, and then read on.

Puzzle a bit longer!

Just a bit longer!

Okay, the second call says, "Starting from `document`, find a `ul` with a
`className` of `unranked-list`. Then find an `li` descended from `ul.unranked-
list` and a `div` descended from that `li`."

### Interlude: Selectors

Now is probably a good time to brush up on [selectors][]. Play around on the
MDN page, then come back when you're ready.

### `querySelectorAll()`

`querySelectorAll` works a lot like `querySelector()` — it accepts a selector
as its argument, and it searches starting from the element that it's called on
(or from `document`) — but instead of returning the _first_ match, it returns a
NodeList (which, remember, is _not_ an _Array_) of _all_ matching elements.

Given a document like

``` html
<main id="app">
  <ul class="ranked-list">
    <li>1</li>
    <li>2</li>
  </ul>

  <ul class="ranked-list">
    <li>10</li>
    <li>11</li>
  </ul>
</main>
```

If we called `document.getElementById('app').querySelectorAll('ul.ranked-list li')`, we'd get back a list of Nodes corresponding to: `<li>1</li>, <li>2</li>, <li>10</li>, <li>11</li>`.

We've been working with JavaScript several lessons, let's try using it to
update our page. An important fact is that the HTML inside of a selected node
can be found by asking the node of its `.innerHTML` property.

We could change the content of these `li`s like so:

``` javascript
const lis = document.getElementById('app').querySelectorAll('ul.ranked-list li')

for (let i = 0; i < lis.length; i++) {
  lis[i].innerHTML = (i + 1).toString()
}
```

Now our `li`s, even though they're children of two separate `ul`s, will count up
from 1 to 4.

Using this loop construct, we could even, say, call `querySelector()` or
`querySelectorAll()` on these children to look deeper and deeper into a nested
structure.

## Conclusion

The DOM selection methods `document.querySelector()` and
`document.querySelectorAll()` are powerful tools for finding elements we need
to update and change.


## Resources

- [document.querySelector()](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector)
- [document.querySelectorAll()](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelectorAll)
- [parseInt()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt)
- [Selectors](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Getting_Started/Selectors)
[selectors]: https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Getting_Started/Selectors
