---
permalink: /struggles.html
---
[Home](/README.md) | [Struggles](/struggles.md)

#### s3 bucket names

Just making sure, that we all understand this that: "An Amazon S3 bucket name is globally unique, and the namespace is shared by **all AWS accounts**. This means that after a bucket is created, the name of that bucket cannot be used by another AWS account in any AWS Region until the bucket is deleted"

This means `trial-bucket` is a name you cannot create a bucket with even if a bucket with that name does not exist in your account. `trial-bucket` has been used by some one, some where.

#### XHTML syntax for inserting code blocks in XHTML based wikis as an example

First, insert `DOCTYPE` before the `<head>` tag of the page if it is not existing. This will ensure that the browser uses a strict formatting, and is backward compatible. Without the `DOCTYPE` browsers use [quirks](https://developer.mozilla.org/en-US/docs/Web/HTML/Quirks_Mode_and_Standards_Mode) mode (before web standards). I have used "strict" DTD. You can choose to use any one of the 3 available [DTDs](https://www.tutorialspoint.com/xhtml/xhtml_doctypes.htm)

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">

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
