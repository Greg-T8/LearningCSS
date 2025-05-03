# Notes from CSS in Depth - by Keith J. Grant

<img src='images/20250406043718.png' width='300'/>

<details>
<summary>Book Resources</summary>

- [Repository of book code listings](https://github.com/CSSInDepth/css-in-depth-2)
- [Can I use](https://caniuse.com/) - Check browser support for CSS features.

</details>

## Chapter 1: Cascade, Specificity, and Inheritance

<details open>
<summary>Details</summary>

### CSS Specificity

<img src='images/20250410025548.png' width='350'/>

It is generally best to keep specificity low when you can, so when you need to overrride something, your options are open.

Rules of thumb for cascading:
1. Don't use IDs in your CSS selectors.
2. Don't use `!important`.

Both of these make it harder to override styles later. IDs are very specific, and `!important` is a hammer that can break things.

### 1.3 Special Values  

Use the `inherit` keyword to force a property to inherit from its parent. This is useful when you want to override a property when a cascaded value is preventing it.

Use the `initial` keyword to set a property to its default value. This is useful when you want to have styles to undo. Every CSS property has an initial, or default, value. Assigning the `initial` keyword to a property will set it to its default value.:

Use the `unset` keyword to set a property value back to `inherit` if it is inheritable, and to `initial` if it is not. Using `unset` makes it a little simpler and helps you avoid using the wrong keyword&mdash;`inherit` or `initial`&mdash;for the property you are working with.

When using `initial` and `unset`, it is important to understand the default property values.

Use `revert` when you want to override your previously set author styles but leave the user-agent styles in place. The `initial` and `unset` keywords essentially override all styles, both from author and user-agent stylesheets.

These keywords are normal cascaded values, so it is possible to override them when another selector with higher specificity targets the same element.

See [here](./ch01/1-inheritance/styles.css) for an example usage of these keywords.

### 1.4 Shorthand Properties
- Shorthand properties enable you to set values of several properties at the same time.

```css
h1 {
  font: 16px/1.5 "Helvetica Neue", Helvetica, Arial, sans-serif;
}
```
- However, be careful with shorthand properties, as they can silently override other properties.
- Avoid using font with shorthand properties because it sets a wide array of properties that can be silently overridden.
- Some shorthand properties have ordering rules. Ordering goes from top, right, bottom, left.

```css
.nav a {
  padding: 10px 15px 0 5px;
}
```

- Specify three values, and the first value is for the top, the second value is for the right and left, and the third value is for the bottom.

```css
.nav a {
  padding: 10px 15px 5px;
}
```

- Specify two values, and the first value is for the top and bottom, and the second value is for the right and left.

```css
.nav a {
  padding: 5px 10px;
}
```

- In most cases, you'll see two values. For example buttons have a higher padding on the left and right (second value) than the top and bottom (first value).
- The top, right, bottom, left rule only applies for properties with four values.
- Other properties, such as `background-position`, have two values, and in this case the first value is for the horizontal position and the second value is for the vertical position.
- The reason for this is that two-value properties represent coordinates x and y on the Cartesian grid.

```css
.nav .featured {
  background-color: orange;
  box-shadow: 10px 2px #6f9090;
}
```
<img src='images/20250416044518.png' width='450'/>  

- If you're working with a property that specifies two meaurements from a corner, think "Cartesian grid". If you're working with a property that specifies four measurements, think "clock".

### 1.5 Progressive Enhancement
- Progressive enhancement lets you offer a basic experience for older browsers and advanced features for modern ones.
- Visit [Can I use](https://caniuse.com/) to check browser support for CSS features.
- Progressive enhancement is built into the cascade.
```css
aside {
  background-color: #333333;      /* Default for all browsers */
  background-color: #333333aa;    /* Adds semi-transparency for modern browsers */
}
```
- Older browsers ignore the second rule. Modern browsers apply the second rule.

#### Progressively enhancing selectors
- When a ruleset has multiple selectors, the browser will ignore the entire ruleset if any of the selectors are not supported.
```css
input.invalid,
input:invalid {
  border: 1px solid red;
}
```
- In this case, if the browser doesn't support `:invalid`, it will ignore the entire rule. This is a problem because the first selector is supported by all browsers.
- In this scenario, the best approach is to separate each selector into its own ruleset.
```css
input.invalid {
  border: 1px solid red; /* All browsers */
}
input:invalid {
  border: 1px solid red; /* Modern browsers */
}
```
#### Feature queries using `@supports`
- Use feature queries to specify multiple declarations for broswers that support a feature compared to those that do not.
```css
@supports(display: grid) {...}
```
- If the browser supports grid, it applies any rulesets inside the braces; otherwise, it ignores them.
```css
.coffees {
  margin: 20px 0;
}

.coffees a {
  display: inline-block;
  min-width: 300px;
  padding: 10px 15px;
  margin-right: 10px;
  margin-bottom: 10px;
  color: black;
  background-color: transparent;
  border: 1px solid gray;
  border-radius: 5px;
}

@supports (display:grid) {
  .coffees {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 10px;
  }
  .coffees a {
    margin: unset;
    min-width: unset;
  }
```
- The fallback and other basic styles, e.g. colors, are outside the feature query, so they'll apply in all browsers.
- See [here](./ch01/3-feature_queries/styles.css) for an example usage of feature queries.

- Feature queries may be constructed in a few other ways:
  - `@supports not (<declaration>)` - This will apply the ruleset if the browser does not support the feature.
  - `@supports (<declaration>) and (<declaration>)` - This will apply the ruleset if the browser supports both features.
  - `@supports (<declaration>) or (<declaration>)` - This will apply the ruleset if the browser supports either feature.
  - `@supports (<selector>)` - This will apply the ruleset if the browser supports the selector.

</details>

## Chapter 2: Working with Relative Units

**Length**: a formal name for a CSS value that deontes a distance measurement. 

### 2.1 The Power of Relative Units

CSS supports several absolute lengths:
- `px` - pixels
- `cm` - centimeters
- `mm` - millimeters
- `Q` - quarter millimeters
- `in` - inches
- `pt` - points (1/72 of an inch)
- `pc` - picas (1/6 of an inch)

Pixel is slightly misleading because a CSS pixel does not strictly equate to a monitor's pixel. This is notably in the case of high-DPI displays, where a CSS pixel is larger than a monitor pixel. This is because the browser scales the CSS pixel to fit the display.

**Responsive design**: refers to styles that "respond" differently based on the size of the browser window. This is typically done using relative units, such as percentages, `em`, and `rem`.

### 2.2 Ems and rems

*Ems*, the most common relative unit, are measured in typography, referring to a specific font size.
```css
.padded {
  font-size: 16px;
  padding: 1em; /* 16px */
}
.padded {
  font-size: 16px;
  padding: 2em; /* 32px */
}
```
  - 1 em means the font size of the current element; it's exact value varies depending on the element you're applying it to.
  - IMPORTANT: Values declared using relative units are evaluated by the browser to an absolute value, called the **computed value**. 
  - Using ems are convenient when setting properties like padding, height, or border-radius, because these scale evenly with the element if it inherits different font sizes or if the user changes font settings.

A powerful feature of ems allows you to define the size of an element and then scale the entire thing up or down with a single declaration that changes the font size:

```css
.box {
  padding: 1em;
  border-radius: 1em;
  background-color: lightgray;
}

.box-small {
  font-size: 12px;
}

.box-large {
  font-size: 18px;   /* Changes the size of the entire box */
}
```
<img src='images/20250503051321.png' width='350'/>

#### 2.2.1 Using ems to define font-size

With regard to ems, the font-size property is a special case. Since an ems is defined by the current element's font size, the font-size property is derived from the inherited font size.

```css
body {
  font-size: 16px;
}

.slogan {
  font-size: 1.2em; /* Calculated size: 1.2 * 16px = 19.2px */
}
```
<img src='images/20250503054014.png' width='350'/>

> For most browsers, the default font size is 16px. This means that 1em is equal to 16px.

#### Ems for font size together with other properties

What makes ems tricky is when you use them for font size together with other properties. When you do this, the browser calculates the font size first, and then it uses that value to calculate the other values.

Both properties can have the same declared value but different computed values:

```css
body {
  font-size: 16px;
}

.slogan {
  font-size: 1.2em;       /* 1.2 * 16px = 19.2px */
  padding: 1.2em;         /* 1.2 * 19.2px = 23.04px */
  background-color: #ccc;
}
```
<img src='images/20250503054940.png' width='400'/>

#### The shrinking font problem

Ems can produce unexpected results when you use them to specify the font sizes of multiple nested elements.

```css
body {
  font-size: 16px;
}

ul {
  font-size: 0.8em;
}
```
<img src='images/20250503055338.png' width='400'/>

Each list has a font size of 0.8em, which is 80% of the font size of the parent element.

**Solution:** Target all unordered lists within an unordered list and set the font size to 1em. This will make the font size of the nested list equal to the font size of the parent list.

```css
body {
  font-size: 16px;
}

ul {
  font-size: 0.8em;
}

ul ul {
  font-size: 1em;
}
```

