# 1. CSS selectors

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
