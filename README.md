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
