# CSS Outline property

The `outline` property in CSS draws a line around the outside of an element. It’s similar to [border](https://css-tricks.com/almanac/properties/b/border/) except that:

1. It always goes around all the sides, you can’t specify particular sides
2. It’s not a part of the [box model](https://css-tricks.com/the-css-box-model/), so it won’t affect the position of the element or adjacent elements \(nice for debugging!\). i.e. it doesn't affect the width and height of the element.

Adding outline on all elements on a page for debuging purpose. \(Try this on developer tool console of any webpage\)

```text
document.head.insertAdjacentHTML("beforeend", "<style>* { 
    outline: 1px solid red; 
}</style>");
```

