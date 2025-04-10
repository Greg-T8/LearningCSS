# Notes from CSS in Depth

<img src='images/20250406043718.png' width='300'/>

**Resources**:
- [Repository of code listings](https://github.com/CSSInDepth/css-in-depth-2)

**CSS Specificity**

<img src='images/20250410025548.png' width='350'/>

It is generally best to keep specificity low when you can, so when you need to
overrride something, your options are open.

Rules of thumb for cascading:
1. Don't use IDs in your CSS selectors.
2. Don't use `!important`.

Both of these make it harder to override styles later. IDs are very specific,
and `!important` is a hammer that can break things.


