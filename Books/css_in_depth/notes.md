# Notes from CSS in Depth

<img src='images/20250406043718.png' width='300'/>

## Resources
- [Repository of book code listings](https://github.com/CSSInDepth/css-in-depth-2)
- [Can I use](https://caniuse.com/) - Check browser support for CSS features.

## Chapter 1: Cascade, Specificity, and Inheritance

### CSS Specificity

<img src='images/20250410025548.png' width='350'/>

It is generally best to keep specificity low when you can, so when you need to overrride something, your options are open.

Rules of thumb for cascading:
1. Don't use IDs in your CSS selectors.
2. Don't use `!important`.

Both of these make it harder to override styles later. IDs are very specific, and `!important` is a hammer that can break things.

### Special Values  

Use the `inherit` keyword to force a property to inherit from its parent. This is useful when you want to override a property when a cascaded value is preventing it.

Use the `initial` keyword to set a property to its default value. This is useful when you want to have styles to undo. Every CSS property has an initial, or default, value. Assigning the `initial` keyword to a property will set it to its default value.:

Use the `unset` keyword to set a property value back to `inherit` if it is inheritable, and to `initial` if it is not. Using `unset` makes it a little simpler and helps you avoid using the wrong keyword&mdash;`inherit` or `initial`&mdash;for the property you are working with.

When using `initial` and `unset`, it is important to understand the default property values.

Use `revert` when you want to override your previously set author styles but leave the user-agent styles in place. The `initial` and `unset` keywords essentially override all styles, both from author and user-agent stylesheets.

These keywords are normal cascaded values, so it is possible to override them when another selector with higher specificity targets the same element.

See [here](./ch01/1-inheritance/styles.css) for an example usage of these keywords.

### Shorthand Properties
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

### Progressive Enhancement
- Progressive enhancement enables you provide an acceptable (but less full-featured) experience to users with older browsers, while still providing a more advanced experience to users with modern browsers.
- 
