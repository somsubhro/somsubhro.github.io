---
permalink: /struggles.html
---
[Home](/README.md) | [Struggles](/struggles.md)

#### XHTML syntax for inserting code blocks in XHTML based wikis as an example

First, insert `DOCTYPE` before the `<head>` tag of the page if it is not existing. This will ensure that the browser uses a strict formatting, and is backward compatible. Without the `DOCTYPE` browsers use [quirks](https://developer.mozilla.org/en-US/docs/Web/HTML/Quirks_Mode_and_Standards_Mode) mode (before web standards).

```xhtml
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">

<head>
  ....
</head>
```

Next, wrap your text that you want to represent within code block with `<pre>... Your text ... </pre>` tags. That simple. Took me a whole day trying with `<code>`, `<samp>`, `<kbd>`

<pre>
  code block
  code block
</pre>

#### CSS Validation service

[Go here](http://jigsaw.w3.org/css-validator/)

#### XHTML Validation service

[Go here](https://validator.w3.org/)
