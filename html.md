---
layout: default
title: HTML
---

# HTML #

## Specifications ##

<https://www.w3.org/TR/>:

* HTML 4: [html4/](https://www.w3.org/TR/html4/); 4.0: [html40/](https://www.w3.org/TR/html40/); 4.01: [html401/](https://www.w3.org/TR/html401/)
* HTML 5: [html5/](https://www.w3.org/TR/html5/)
  * HTML 5 (5.0) before 5.1: <[html50/](https://www.w3.org/TR/html50/)>; 2014 recommendation: <[2014/REC-html5-20141028/](https://www.w3.org/TR/2014/REC-html5-20141028/)>. Some URLs referring to ''html5'' can be fixed to ''html50''.

## Doctype ##

<pre>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01//EN"
    "http://www.w3.org/TR/html4/strict.dtd">
</pre>

In HTML 5, this is an "obsolete permitted" doctype. In XML, DOCTYPE is a keyword, and must be uppercase. The ''html'' refers to the <html> root element. HTML 4 has it in uppercase, but in XHTML, element names must be lowercase.

## Attribute values ##

* Ampersands "&" specify character references
* No value implies empty string
* Unquoted value: HTML 5.0 says whitespace, double quotes (`"`), apostrophes (`'`), equal signs "=", less-than and greater-than signs "< >", and backticks (`` ` ``) are illegal, and the value must not be empty. HTML 4.01 says only ASCII letters, digits, hyphens `"-"`, full stops ".", underscores "`_`", and colons ":" are allowed. HTML 4.0 (before 4.01) did not allow underscores "`_`" nor colons ":" either. <http://jkorpela.fi/qattr.html>
* Quoted value: only the quote type (single or double) is illegal inside. HTML 4.0 says the greater-than sign ">" ''should'' also be escaped, because it could be seen as the end of the element tag. Numeric references and "&amp;quot;" can be used to escape quotes.
* HTML 5.0: <https://www.w3.org/TR/html50/syntax.html#attributes-0>
* HTML 4.01: <https://www.w3.org/TR/html401/intro/sgmltut.html#h-3.2.2>
* HTML 4.0 before 4.01: <https://www.w3.org/TR/1998/REC-html40-19980424/intro/sgmltut.html#h-3.2.2>

## Forms ##

In HTML 4, the ''action'' attribute is required, but is allowed to be an empty URL. In HTML 5, it is not allowed to be empty, but is allowed to be omitted, which implies an empty URL. The reason for disallowing the explicit empty value seems to be due to confusion about whether the ''action'' URL is relative to a &lt;base&gt; element or not, and/or inconsistencies about the base with an empty URL: <https://stackoverflow.com/questions/415234/forms-with-action/617197#617197>.
