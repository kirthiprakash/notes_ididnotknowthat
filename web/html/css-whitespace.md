---
description: 'ref: https://developer.mozilla.org/en-US/docs/Web/CSS/white-space'
---

# \[CSS\] whitespace

To preserve a whitespace characters \(newline \n, spaces and tabs\), we can apply the whitespace CSS property.

If a text has a lot of  \n we HTML won't preserve this. To break a line, we have to use &lt;br&gt; tag.

Using CSS, we can apply `whitespace: pre-line` property to preserve the newline characters



The following table summarizes the behavior of the various `white-space` values:

|  | New lines | Spaces and tabs | Text wrapping | End-of-line spaces |
| :--- | :--- | :--- | :--- | :--- |
| `normal` | Collapse | Collapse | Wrap | Remove |
| `nowrap` | Collapse | Collapse | No wrap | Remove |
| `pre` | Preserve | Preserve | No wrap | Preserve |
| `pre-wrap` | Preserve | Preserve | Wrap | Hang |
| `pre-line` | Preserve | Collapse | Wrap | Remove |
| `break-spaces` | Preserve | Preserve | Wrap | Wrap |

