---
layout: default
title: HTML
---

# HTML #

## Specifications ##

<https://www.w3.org/TR/>:

* HTML 4: [html4/](https://www.w3.org/TR/html4/); 4.0: [html40/](https://www.w3.org/TR/html40/); 4.01: [html401/](https://www.w3.org/TR/html401/)
* HTML 5: [html5/](https://www.w3.org/TR/html5/)
  * HTML 5 (5.0) before 5.1: <[html50/](https://www.w3.org/TR/html50/)>; 2014 recommendation: <[2014/REC-html5-20141028/](https://www.w3.org/TR/2014/REC-html5-20141028/)>. Some URLs referring to _html5_ can be fixed to _html50_.

## Doctype ##

<pre>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01//EN"
    "http://www.w3.org/TR/html4/strict.dtd"><!-- Encoding: UTF-8 -->
</pre>

In HTML 5, this is an "obsolete permitted" doctype. In XML, DOCTYPE is a keyword, and must be uppercase. The _html_ refers to the <html> root element. HTML 4 has it in uppercase, but in XHTML, element names must be lowercase.

Don't put comments before the doctype; that can prevent recognition of the
doctype.

## Attribute values ##

* Ampersands "&" specify character references
* No value implies empty string
* Unquoted value: HTML 5.0 says whitespace, double quotes (`"`), apostrophes (`'`), equal signs "=", less-than and greater-than signs "< >", and backticks (`` ` ``) are illegal, and the value must not be empty. HTML 4.01 says only ASCII letters, digits, hyphens `"-"`, full stops ".", underscores "`_`", and colons ":" are allowed. HTML 4.0 (before 4.01) did not allow underscores "`_`" nor colons ":" either. <http://jkorpela.fi/qattr.html>
* Quoted value: only the quote type (single or double) is illegal inside. HTML 4.0 says the greater-than sign ">" _should_ also be escaped, because it could be seen as the end of the element tag. Numeric references and "&amp;quot;" can be used to escape quotes.
* HTML 5.0: <https://www.w3.org/TR/html50/syntax.html#attributes-0>
* HTML 4.01: <https://www.w3.org/TR/html401/intro/sgmltut.html#h-3.2.2>
* HTML 4.0 before 4.01: <https://www.w3.org/TR/1998/REC-html40-19980424/intro/sgmltut.html#h-3.2.2>

## Forms and controls ##

In HTML 4, the _action_ attribute is required, but is allowed to be an empty URL. In HTML 5, it is not allowed to be empty, but is allowed to be omitted, which implies an empty URL. The reason for disallowing the explicit empty value seems to be due to confusion about whether the _action_ URL is relative to a &lt;base&gt; element or not, and/or inconsistencies about the base with an empty URL: <https://stackoverflow.com/questions/415234/forms-with-action/617197#617197>.

**Buttons:** Prefer &lt;input type=submit&gt; over &lt;button&gt; where the content is text only. &lt;Button&gt; has more functionality and is favoured by MDN, but &lt;input&gt; is older, and IE6 had troubles with multiple submit &lt;button&gt; buttons.

With &lt;input type=submit&gt;, the _value_ attribute is the button's label. With multiple submit buttons, use unique _name_ attributes to identify the button. Only the _name_ and _value_ of the activated submit button is submitted.

Need to specify type=submit?

## Scripts ##

The &lt;script type=`"text/javascript"`&gt; attribute is required by HTML 4,
and is optional in HTML 5. The type for "intrinsic events" should be
specified by &lt;META http-equiv=`"Content-Script-Type"`
content=`"text/javascript"`&gt;.
