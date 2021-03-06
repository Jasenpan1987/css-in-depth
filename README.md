# 1. CSS selectors

https://estelle.github.io/cssmastery/

- try not to use id, use class instead.
- CSS selector levels:

level 1:

- element
- #id
- .class
- :link
- E F
- :active
  ...

level 2:

- \*
- E + F
- E > F
- E[attribute]
- :before :after
- :first-line :first-letter
- :hove :focus :visited
  ...

level 3:

- ::before ::after
- :root
- ::first-letter ::last-letter
- E-F
- type(n)
- E[attribute^=]
  ...

  1.1 Css Specificity

- ID: 1-0-0
- class: 0-1-0
- elem: 0-0-1

higher number wins, if same score, the later one applies

## 1.1 Relational selectors and combinators

```css
ul li,
ol li {
}
```

decendent selector, selects all the li under ul or all the li under ol

```css
ul > li {
}
```

all the li which are direct child of a ul.

```css
.foo + li {
}
```

find the li that come immeditely after the `foo` class, adjacent sibling.

```css
.foo ~ li {
}
```

find all the li that come after the `foo` class, don't have to be immeditely after, general sibling.

## 1.2 Attribute selector

### 1.2.1 Basic Selectors

```css
img[alt] {
}
```

```html
<img src="a.png" alt="img1"><!-- matches -->
<img src="b.png">
```

Matches all the img element as long as it has the `alt` attribute.

```css
img:not([alt]) {
}
```

This will match the second image

---

```html
<form>
  <input type="date">
  <select>
    <option></option>
  </select>
</form
```

```css
form [type] {
}
```

This will match only the input, because it is the desendent of the `form` element and has the `type` attribute

### 1.2.2 Advanced attribute selectors

```html
<form>
  <input type="text"> 1
  <input type="number"> 2
</form>
<p lang="en-us"></p> 3
<p lang="en-uk"></p> 4
```

```css
input[type="text"] {
}
```

it matches all the input elements which has a `type` attribute and its value matches the exact `text` (case sensitive).
here it mathes the `1`

```css
p[lang|="en"] {
}
```

`[lang|=en]` means the matched element should have a `lang` attributes and its value should start with `en-`, so here it matches the `3` and `4`

```html
<a href="mailto:jasenpan@gmail.com">email me</a> 1

<a href="http://google.com">Google</a> 2
<a href="https://ebay.com">Ebay</a> 3
```

```css
a[href^="mailto"] {
  // matches 1
}

a[href^="http"] {
  // matches 2, 3
}
```

`[attr^=val]` means the matched elements has the `attr` attribute which value STARTS WITH val.

---

```html
<li title="unicorns"></li> 1
<li title="happy unicorn"></li> 2
```

```css
li[title~=unicorn]{
  will match 2 only
}
```

`[attr^=val]` means the matched elements has the `attr` attribute which value `val` in a full word.

---

```html
<a href="/documents/foo.pdf">Download</a>
```

```css
a[href$="pdf"] {
  background-image: url(book.png);
}

a[href$="pdf"]:after {
  content: "(" attr(href) ")";
}
```

`[attr$=val]` means the matched elements has the `attr` attribute which value END WITH val.

---

```html
<a href="/documents/foo.pdf">Download</a> 1
<a href="http://myfood.com">Food link</a> 2
```

```css
a[href*=foo] {
  matches 1 and 2
}
```

`[attr*=val]` means the matched elements has the `attr` attribute which contains `val` in anywhere.

### 1.2.3 case sensitivity

```css
a[type="TEXT" i]{
  will match type=text
}
```

https://codepen.io/estelle/pen/lEGev

---

Printing friendly tricks

```css
@media print {
  abbr[title]:after {
    content: "(" attr(title) ")";
  }
  a[href^="http"]:after {
    content: "(" attr(href) ")";
  }
}
```

## 1.3 UI Selector

### 1.3.1 Form related selectors

input

- :disabled :enabled
- :checked
- :required :optional
- :valid :invalid
- :in-range :out-of-range

```css
input[type="checkbox"]:checked + label {
  color: red;
}
```

How to read:
a `label` that comming immeditely after a `checked input` which type is `checkbox` has the color `red`.

`:invalid` and `:not(:valid)` are identical.

## 1.4 Structual Selectors

- :root
- :first-child :nth-child
- :first-of-type
  ...

The structual selectors target on elements on the page based on their relationship other elements in the DOM.

### 1.4.1 first/last child

```css
body .foo:last-child {
}
```

it will match the last child element inside the `<body>` which also has a class of `foo`.

```css
p.foo:last-child {
}
```

it will find all the last child inside a `<p>` tag which also has the class of `foo`.

### 1.4.2 nth pseudo class

NOTE: css uses 1 based index (starts from 1)

```html
<ul>
  <li>item 1</li>
  <li>item 2</li>
  <li>item 3</li>
  <li>item 4</li>
  <li>item 5</li>
  <li>item 6</li>
  <li>item 7</li>
  <li>item 8</li>
  <li>item 9</li>
  <li>item 10</li>
</ul>
```

```css
li:first-child,
li:last-child {
  will match 1 and 10
}

li:nth-child(even) {
  will match 2,4,6,8,10
}

li:nth-child(3) {
  will match 3
}

li:nth-of-type(4n) {
  will match 4, 8
}

li:nth-of-type(3n-1) {
  will match 2, 5, 8
}
```

### 1.4.3 root and empty

`:root` selector refers to the root of the document, which is normally `<html>`.
You can define your `rem` unit here.

`:empty` selector refers to the elements that is empty, such as self-closing elements `<img />` and `<input>`, also `<div></div>` is an empty element, even `<div><!-- hello --></div>` is also an empty element.

## 1.5 Other logical combinators

### 1.5.1 negation

`:not(simple selector)` selector will match everything which is not satisfy the condition. Here simple selector means any non-combined selectors such as `(.foo.bar)`.

```css
div:not(.foo) {
}
```

This will match all the `<div>` elements don't have the `foo` class.

`:not` doesn't have any specificity, the previous selector and the simple selector inside the brackets give the specificity.
The previous example has `0-1-1` in it's specificity because it has 1 element selector and 1 class selector.

```css
div:not(.foo,.bar) {
  only work in safari
}
==
div:not(.foo):not(.bar){
  work in every modern browser
}
```

### 1.5.2 link pseudo class

`:link`, `:visited` are link pseudo classes, and `:hover`, `:active`, `:focus` are user action pseudo classes. When you want to add style on `:focus` (mouse), also do it on `active`(keyboard).

## 1.6 Specificity

simple case

- \* , +, >, \_ = 0-0-0
- element = 0-0-1
- class = 0-1-0
- [attr=val] = 0-1-0
- :not(.foo) = 0-1-0 // not has no value, but the class has 0-1-0
- pseudo elem = 0-1-0
- id = 1-0-0 // try not to use
- style = 1-0-0-0 // bad
- !important = 1-0-0-0-0 // bad

complex case

- li[title=bar] = 0-1-1
- li.foo = 0-1-1
- li:nth-child(3)~span = 0-1-2
- form input[type=text] = 0-1-2
- li.foo:nth-of-type(3n) = 0-2-1
- input[type=checkbox]:not(.foo) = 0-2-1

### 1.6.1 Specificity tricks

1)

```html
<div class="foo">Foo</div>
```

```css
.foo {
  color: red; // try override me
}
```

To override it without changing the html, we can do this, but don't forget to add comments

```css
.foo.foo {
  // it will give higher specificity
  color: blue;
}
```

2)

```html
<div class="foo">Foo</div>
```

```css
.foo {
  color: yellow !important; // try override me!
}
```

solution: 6666666~

```css
.foo {
  animation: colorhack forwards;
}
@keyframe colorhack {
  100% {
    color: blue;
  }
}
```

## 1.7 Pseudo Elements

- ::first-line ::first-letter
- ::selection (not in specification)
- ::before
- ::after

### 1.7.1 ::selection

```css
.foo::selection {
  background-color: red;
}
```

The selected part (mouse highlighted) will become to red instead of blue.

### 1.7.2 ::before and ::after

```html
<p>Im the content</p>
```

```css
p::before {
  content: "before- ";
  font-weight: bold;
}

p::after {
  content: " -after";
  font-weight: bold;
}
```

you will see the following
**before-** Im the content **-after**

always add a content to `::before` and `::after`, and don't put them on self-closing tags, one more thing, you can't select or highlight these pseudo elements because they are not exist on your html page.

# 2. Generated Content

## 2.1 Attribute values

```css
a[href^="http"]:hover {
  position: relative;
}

a[href^="http"]:hover:after {
  content: attr(href);
  position: absolute;
  top: 1rem;
  left: 0;
  color: white;
  background-color: black;
  padding: 3px 5px;
  line-height: 1;
}
```

It added pseudo elements after each `<a>` which has an external link, the `attr(href)` will give us the actual url.

## 2.2 counter

```html
<p>Hello this is paragraph</p>
<p>Hello this is paragraph</p>
<p>Hello this is paragraph</p>
<p>Hello this is paragraph</p>
```

```css
body {
  counter-reset: paractr;
}
p {
  counter-increment: paractr;
}
p:after {
  content: " - " counter(paractr);
  color: red;
}
```

## 2.3 Image

```css
.showMe {
  position: relative;
}
.showMe:hover::after {
  position: absolute;
  content: url(attr(data-url)); /* doesn't work */
  content: url(estelle.svg); /* does work */
  width: 200px;
  height: 200px;
  color: blue;
  bottom: -39px;
  left: 20px;
}
```

## 2.4 Add a bubble

```css
.quote {
  border-radius: 10px;
  position: relative;
  padding: 20px;
  background-color: red;
  color: white;
}
.quote:after {
  position: absolute;
  content: "";
  width: 0;
  height: 0;
  border: 20px solid blue;
  border-color: red transparent transparent transparent;
  bottom: -39px;
  left: 20px;
}
```

# 3. Media Query

## 3.1 Media type and screen size

```css
@media screen and (max-width: 600px) {
  #presentation {
    background: red;
  }
}
@media screen and (orientation: portrait) {
  #presentation {
    background: yellow;
  }
}

@media screen and (orientation: landscape) {
  a[href^="mailto:"]:before {
    content: url(icons/email.gif);
  }
}
```

- Portrait basically means the width is less than its height

And you can even do this:

```html
<link rel='stylesheet'
media='screen and (min-width: 320px) and (max-width: 480px)'
href='css/smartphone.css' />
```

- **You don't want to pick a break point to fit a specific device, you only want to pick break points that fits your design.**

- **Always use min/max to target height/width, otherwise, it will only target on 1 pixel.**

## 3.2 browser capability (feature query)

```css
@supports (display: table-cell) and (display: list-item) {
  .query .supports {
    display: block;
  }
}
```

The inner code will only run if the browser has the `display: table-cell` and `display: list-item` features. You can have the vendor prefix such as `webkit-` for the queries.

## 3.3 Media Query Usecases

### 3.3.1 Relative width

```css
@media screen and (min-width: 38em) {
  #content {
    padding: 0 21%;
  }
}
```

### 3.3.2 have hyphens in smaller screens

```css
@media screen and (max-width: 15rem) {
  #content {
    padding: 0 21%;
  }
}
p {
  hyphens: auto;
}
```

# 4.colors

## 4.1 Color formats

```css
color: white;
color: #fff;
color: #ffffff;
color: #ffffffff;
color: rgb(255, 255, 255);
color: rgb(100%, 100%, 100%);
color: rgba(255, 255, 255, 1);
color: rgba(100%, 100%, 100%, 1);
color: hsl(0, 100%, 100%);
color: hsla(0, 100%, 100%, 1);
color: transparent;
color: currentColor; // current color of the text
```

## 4.2 Color tips

1.  transparent = rgba(0,0,0,0)
2.  current color is the current TEXT color
3.  for accessibility, do not use the color as the only source of information
4.  changes the appearence of the buttons and other control to resemble native controls.

# 5. flexbox

## 5.1 browser support

All the modern browser except IE11, you need prefix.

## 5.2 examples

### 5.2.1 Basic example

```html
<ul>
  <li>Home</li>
  <li>About</li>
  <li>Contact</li>
  <li>Jobs</li>
</ul>
```

```css
ul {
  display: flex;
  padding: 3px;
  justify-content: center;
  align-items: baseline;
}

ul > li {
  text-align: center;
  flex: 1;
}
```

- `flex: 1` means the elements can grow and take up one unit.

### 5.2.2 Holy Grail Layout

```html
<header>
  header
</header>
<main>
  <article>article</article>
  <nav>nav</nav>
  <aside>aside</aside>
</main>
<footer>footer</footer>
```

```css
header,
article,
nav,
aside,
footer {
  background: gray;
  color: white;
  border-radius: 0.5em;
  padding: 0.5em;
  margin: 0.5em;
}

body {
  display: flex;
  flex-direction: column;
}

main {
  display: flex;
  flex: 1;
}

article {
  flex: 1;
}

nav {
  order: -1;
}
```

## 5.3 flexbox in detail

1.  Creation: display
2.  Direction: flex-flow (flex-direction, flex-wrap)
3.  Alignment: justify-content, align-items, align-self, align-content
4.  Ordering: order
5.  Flexibility: flex (flex-grow, flex-shrink, flex-basis)

Flex properties: https://estelle.github.io/cssmastery/flexbox/#slide16

### 5.3.1 steps

1.  Add display: flex; to the parent of the elements to be flexed.
2.  Set flex-direction to horizontal or vertical
3.  Set flex-wrap to control wrap direction

### 5.3.2 Flex items

Flex Items

- All non-absolutely positioned child nodes
- Generated Content
- anonymous flex items => non-empty text nodes

Not flex items

- ::first-line & ::first-letter
- white space

Kind of

- absolutely/fixed positioned elements

### 5.3.3 Impacted CSS Properties

Changed Properties

- margin: adjacent flex items margins do not collapse
- min-width & min-height: default is auto, not 0
- visibility: collapse;

Ignored Properties

- column-\* properties
- float
- clear
- vertical-align

### 5.3.4 flex-direction property

- row
- row-reverse
- column
- column-reverse

### 5.3.5 flex-wrap property

- nowrap
- wrap
- wrap-reverse

Purpose: is the whole shebang on one line, or can it wrap if necessary?

Flex-flow: short hand of flex-direction and flex-wrap

`flex-flow: column wrap` is the same to `flex-direction: column; flex-wrap: wrap;`.

## 5.4 flex container properties

### 5.4.1 justify-content property

- flex-start
- flex-end
- center
- space-between
- space-around
- space-evenly

aligns flex items along the main axis

```css
body > div {
  display: flex;
  justify-content: space-between;
}
.a,
.b,
.c {
  flex: 0 0 100px;
}
```

### 5.4.2 align-items property

- flex-start
- flex-end
- center
- baseline
- stretch

aligns flex items along the cross axis

### 5.4.3 align-content property

- flex-start
- flex-end
- center
- space-between
- space-around
- stretch
- space-evenly

Only applies to multi-line flex containers (wrap).

## 5.5 flex-item properties

### 5.5.1 align-self property

- auto: **inherit from its parent**
- flex-start
- flex-end
- center
- baseline
- stretch

Override the align-items on a per flex item basis

```css
.container {
  align-items: center;
}
.b {
  align-self: stretch;
}
```

### 5.5.2 order property

The default value is 0. Anything lower will come before those without set values. Anything above will come after.

```css
div:nth-of-type(3n) {
  order: -1;
}
```

## 5.6 Flex flexibility and shorthands

- flex-grow: How to divide the extra space. Non-negative number. default: 1.

- flex-shrink: How to shrink if there's not enough room. Non-negative number. default: 1.

- flex-basis: the starting size before free space is distributed. length value, content or auto . If set to auto, sets to flex item’s main size property.

```css
.a {
  flex-grow: 1;
  flex-shrink: 1;
  flex-basis: 200px;
}

// is identical to
.a {
  flex: 1 1 200px;
}
```

example

```html
<div class="container">
  <div class="a">aaa</div>
  <div class="b">aaa</div>
  <div class="c">aaa</div>
</div>
```

```css
.container {
  display: flex;
}

.a,
.b {
  flex: 2;
}

.c {
  flex: 1;
}
```

**Flex grow only divide the spare spaces.**

### 5.6.2 Flex basis

`flex-basis: auto` means the base size of the element is its content size, `flex-basis: 0%` means the base size is 0 and the size of the element is depends on its flex-grow factor.

example

```html
<div class="container">
  <div class="a">A</div>
  <div class="b">B</div>
  <div class="c">C</div>
</div>
```

1.  flex-basis auto

```css
.container {
  display: flex;
}

.a {
  flex: 2 1 auto;
}

.b {
  flex: 1 1 auto;
}

.c {
  flex: 3 1 auto;
}
```

2.  flex-basis 0%

```css
.container {
  display: flex;
}

.a {
  flex: 2 1 0%;
}

.b {
  flex: 1 1 0%;
}

.c {
  flex: 3 1 0%;
}
```

Let's say the width of the container is 1200px wide, and the content of a, b and c are 200px wide.

- In the `auto` case, each one has a width of 200, so the spare space is 1200 - 200*3 = 600px,
  so according to the flex grow ratio, .a will take up 2/6 * 600 = 200px plus 200px of the content is 400px, .b will take 1/6 \_ 600px = 100px plus its 200px content which is 300px, and c will take 3/6 \* 600 = 300px plus 200px which is 500px.

- In the `0%` case, each one has 0 width by default, the width is depends on its flex ratio. So .a will take 2/6 _ 1200 = 400px, b will take 1/6 _ 1200 = 200px, c will take 3/6 \* 1200 = 600px.

### 5.6.3 flex-shrink

- The element will never shrink less than its longest word.

```css
body > div {
  display: flex;
  flex-direction: row;
}
div > div {
  flex: 20%;
}
.b {
  order: -1;
  flex: 40%;
}
```

# 6. Tables

## 6.1 Table structure

```html
<table>
  <caption></caption>
  <colgroup>
    <col/>
  </colgroup>
  <thead>
    <tr>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td></td>
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <th></th>
    </tr>
  </tfoot>
</table>
```

## 6.2 Caption

- The title of the table.
- Always the first child of the table.
- Can be styled

```css
table {
  caption-side: bottom; // default is top
}

caption {
  color: #cc0000;
  font-weight: bold;
}
```

## 6.3 Border and spacing

```css
table,
th,
td {
  border: 1px solid;
}
table {
  border-spacing: 10px 5px;
  border-collapse: collapse;
}
```

`border-collapse: separate | collapse | inherit`

When the borders are collapsed border-spacing is relevant.

`table, td, th {border: none;}`

`table {border-spacing: 5px 10px;}`

- one length: vertical and horizontal padding are the same.
- two lengths: first is horizontal, second is vertical
- Note: not TRouBLe
- Irrelevant if border-collapse: collapse
- Empty space is part of the table, not the column, tbody, row or cell.

## 6.4 Other table properties

### 6.4.1 Show / hide empty cells

`empty-cell: show | hide`

is similar to

`td:empty, th:empty { visibility: none; }`

### 6.4.2 Verticle align

```css
vertical-align: baseline | sub | super | text-top | text-bottom | middle | top |
  bottom | <percentage> | <length>;
```

# 7. Css Grids

## 7.1 Terminology

- Grid lines: The vertical and horizontal lines that divide the grid and separate the columns and rows. Start counting at 1, not 0.
- Grid cell: A single child or unit of a CSS grid.
- Grid area: Any rectangular space surrounded by four grid lines. Can contain any number of grid cells.
- Grid track: The space between two grid lines. This space can be horizontal or vertical: a grid row or grid column
- Grid row: A horizontal grid track.
- Grid column: A vertical grid track.
- Gutter: Space between two rows and two columns.

## 7.2 Grid Container Properties

- `fr` is the fraction unit of one space, `repeat(4, 1fr)` means divide the space into 4, and each one takes one unit.
- `display: grid` will turn the elements into grid mode, and `display: grid-inline` will give each element the width of auto.

```html
<div>
  <span>1</span>
  <span>2</span>
  <span>3</span>
  <span>4</span>
  <span>5</span>
  <span>6</span>
  <span>7</span>
  <span>8</span>
  <span>9</span>
</div>
```

```css
div {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
}
```

will give you a 3 X 3 grid display.

## 7.2 Colmuns and Rows

### 7.2.1 basic row and column

```css
grid-template-columns:
  150px 150px 150px; // 1
  repeat(3, 150px); // 2
  275px repeat(2, 150px); // 3
  100px repeat(2, 1fr) 2fr; // 4
```

1. 3 columns, each column takes 150px wide.
2. 3 columns, each column takes 150px wide.
3. 3 columns, first columns takes 275px and the other two take up the rest of the space.
4. 4 columns, first columns takes 100px and second and third take up one unit of the rest of the space and the fourth column takes two units.

```css
grid-template-rows:
  150px 150px 150px; // 1
  repeat(3, 150px); // 2
  275px repeat(2, 150px); // 3
  100px 1fr 2fr; // 4
```

1. 3 rows, each row takes 150px height.
2. 3 rows, each row takes 150px height.
3. 3 rows, first row takes 275px height, and the second and the third row takes 150px height.
4. 3 rows, the first row takes 100px height, the second row takes 1 unit of the free space, and the last row takes 2 units.

### 7.2.2 name grid line

```css
grid-template-columns: [start] 150px 150px 150px [end];
```

This basically says first line will be called "start", second line no name, thrid line no name, fourth line no name, and the fifth line will be called "end". This is easier sometimes because we don't have to remember the line numbers.

### 7.2.3 Gutters

```css
ol {
  grid-template-columns: repeat(2, 2fr) repeat(3, 1fr) 3fr;
  grid-template-rows: 2fr 125px repeat(2, 4em);
  grid-row-gap: 10px; // 1
  grid-column-gap: 20px; // 2
  grid-gap: 10px 20px; // 3
  grid-gap: 1em; // 4
}
```

1. Give a 10px gap between rows.
2. Give a 20px gap between columns.
3. Equal to 1 + 2
4. Give a 1em gap to both row and column.

```css
// in an ordered list with 35 li
ol {
  display: grid;
  grid-template-column: 10px 20px 40px 80px 160px 320px 640px;
  grid-template-row: 10px 20px 40px 80px 160px 320px 640px;
  grid-gap: 20px;
  list-style-type: none;
}
```

### 7.2.4 Position an item

```css
.myItem {
  grid-row-start: 2;
  grid-row-end: 4;
  grid-column-start: 2;
  grid-column-end: 5;
}
.myItem {
  grid-row: 2 / 4;
  grid-column: 2 / 5;
}
.myItem {
  grid-area: 2 / 2 / 4 / 5;
}
```

These three are totally identical.

### 7.2.5 Holy Grail Layout

```html
<body>
  <header>header</header>
  <article>article</article>
  <nav>nav</nav>
  <aside>aside</aside>
  <footer>footer</footer>
</body>
```

```css
body {
  display: grid;
  grid-template-rows: 1fr 4fr 1fr;
  grid-template-columns: 3em 1fr 1.5em;
  grid-gap: 1em;
}

header {
  background-color: grey;
  grid-row: 1 / 2;
  grid-column: 1 / 4;
}

footer {
  background-color: yellow;
  grid-row: 3 / 4;
  grid-column: 1 / 4;
}

nav {
  background-color: red;
  grid-row: 2 / 3;
  grid-column: 1 / 2;
}

aside {
  background-color: pink;
  grid-row: 2 / 3;
  grid-column: 3 / 4;
}

article {
  background-color: blue;
  grid-row: 2 / 3;
  grid-column: 2 / 3;
}
```

### 7.2.6 Named Grid Area

```html
<body>
  <header>header</header>
  <article>article</article>
  <nav>nav</nav>
  <aside>aside</aside>
  <footer>footer</footer>
</body>
```

```css
body {
  display: grid;
  grid-template-rows: 2fr 9fr 2fr;
  grid-template-columns: 1fr 6fr 1fr;
  grid-gap: 1em;
  grid-template-areas:
    "header header header"
    "nav article aside"
    "footer footer footer";
}
header {
  background-color: grey;
  grid-area: header;
}

footer {
  background-color: yellow;
  grid-area: footer;
}

nav {
  background-color: red;
  grid-area: nav;
}

aside {
  background-color: pink;
  grid-area: aside;
}

article {
  background-color: blue;
  grid-area: article;
}
```

## 7.3 Alignment

### Container Properties

```
justify-items // horrizontal
align-items // verticle
justify-content
align-content
grid-auto-columns
grid-auto-rows
grid-auto-flow
grid
```

### Item Properties

```
justify-self
align-self
```

https://gridbyexample.com/

# 8. Background and Borders

## 8.1 Backgrounds

### 8.1.1 Background image

`background-image: url(path/aSingleImage.jpg);`

Background short hands

- background-repeat
- background-attachment
- background-position
- background-color

Don't use background short hands, it will overwrite other background properties. Use the specific properties

```css
background: blue; // the other properties will become to the default values!!!

background-color: blue; // others will remain
```

### 8.1.2 Multiple Background image

`background-image: url(green.gif), url(blue.gif);`

- Comma separated
- layered front to back
- Include background-color

### 8.1.2 Background-repeat

```css
background-repeat: repeat | repeat-x | repeat-y | no-repeat | space | round;
```

- `repeat` is the default value
- `space` and `around` works like `repeat` but has no cut offs, because they give the space to the margin of each images, `around` will resize the images.

### 8.1.3 Background-attachment

```css
background-attachment: fixed | local | scroll;
```

- `fixed` will stay behind the content
- `local` will stay where it is
- `scroll` will stay at the same position even scroll

### 8.1.4 background-position

```css
div {
  background-position: 75% 75%;
}
```

This basically means, the image will be positioned on 75% from the top corner and 75% from the left corner. But in css, we can do positioning relative to any corner.

```css
background-position: right 50px bottom 50px;
```

This will make the image 50px from the bottom and 50px from the right. And the top left corner is the default.

### 8.1.4 background clip and origin

example

```html
<div></div>
```

```css
div {
  border: 15px dotted #707071;
  border-radius: 5px;
  background-origin: border-box;
  background-clip: border-box;
}

background-image: radial-gradient(
  circle closest-side at 50px 50px,
  transparent 70%,
  #58ade3 70%,
  95%,
  transparent 95%
);
background-size: 95px 95px;
background-repeat: no-repeat;
height: 200px;
width: 200px;
margin: auto;
```

try to change around the `background-origin` and `background-clip` between `content-box`, `padding-box` and `border-box` to see the difference.

### 8.1.5 Background-size

```css
background-size: auto | contain | cover | 100px 200px;
```

- auto: image is actual size
- cover: image maintains aspect ratio, covering entire element even if that means part of the image is cut off.
- contain: image maintains aspect ratio, fitting the entire image into the element even if that means part of the background is showing or the image repeats.
- length: image maintains aspect ratio, growing or shrinking so that the width is the length defined
- auto length: image maintains aspect ratio, growing or shrinking so that the height is the length defined
- length length: image DOES NOT NECESSARILY maintain its aspect ratio, height and width both grow or shrink to the length defined

## 8.2 Border

### 8.2.1 border-color and border style

- The default border color is `current-color` and we can use any color structure to define a border-color.

- The border style has the following values:
  - none: No border. As if border-width: 0; except with border-image
  - hidden: Same as none, but relevant in border-collapsed tables
  - dotted: Round dots.
  - dashed: Square-ended dashes.
  - solid: a single solid line.
  - double: Two parallel solid lines. Only relevant if border-width >= 3px. line + space + line = border-width value.
  - groove: Shadow effect using two colors on each side, with a slightly lighter and darker than the border-color value. Top / left half of each side darker.
  - ridge: Looks as if it were coming out of the canvas. Bottom / Right darker
  - inset: Looks sunken into page. Top and left border are made darker, bottom and right are lighter.. Treated as ridge in border-collapsed tables.
  - outset: Looks as if popping out of page. Bottom and right are darker. Top and left border are lighter.. Treated as groove in border-collapsed tables.
- short hands: `3px solid #fff;`

### 8.2.2 border-radius and border image

Possible values:

```css
border-radius: 0;
border-radius: 20px;
border-radius: 50%;
border-radius: 10px 30px; // top-left+right-bottom and top-right+left-bottom

border-top-left-radius
border-top-right-radius
border-bottom-right-radius
border-bottom-left-radius
```

# 9. Css Gradient

Gradient is a sort of image, so we can use it anywhere when we want to use an image, such as background image, cursor image, list-style-type, border-image, pseudo element content. Generally there are 4 types of gradients:

1. Linear
2. Repeat Linear
3. Radial
4. Repeat Radial

## 9.1 Syntax

### 9.1.1 Linear

```css
linear-gradient([<angle> | to<side-or-corner>,]?[<color-stop>[,color-hint]?,]# <color-stop>)
```

- Use "to" for the key terms
- 0 degree is to top
- Angles go closewise

```css
.slide {
  background-image: linear-gradient(purple, red);
}
```

is equal to

```css
.slide {
  background-image: linear-gradient(to bottom, purple 0%, red 100%);
}
```

### 9.1.2 Radial

```css
radial-gradient([<shape> || <size> at <position>]?[<color-stop>[, <color-hint>]?, ]# <color-stop>)
```

- Use "at" with position
- Position is center of gradient
- If shape is specified as circle or omitted, the size can be a length or a percentage

### 9.2 Color stop

```css
<color> [<length> || <percentage>]?
```

```css
.slide {
  background-image: linear-gradient(
    red 0vh,
    orange 20vh,
    yellow 40vh,
    green 60vh,
    blue 80vh,
    purple 100vh
  );
}
```

0 -> 20: from red to orange,
20 -> 40: from orange to yellow
...

### 9.3 Color hints

```scss
// 1.
linear-gradient(purple, red);

// 2.
linear-gradient(purple, 50%, red);

// 3.
linear-gradient(purple, 20%, red);

// 4.
linear-gradient(purple, 80%, red);
```

The 50%, 20% and 80% are color hints, it tells where the mid-point of the color change should happen.
