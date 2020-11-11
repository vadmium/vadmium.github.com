---
layout: default
title: Unix shell
---

# Unix shell #

## Arithmetic expansion ##

Required by Posix. Integer arithmetic on C signed long (or higher rank)
values. Includes assignment operators, but may be limited to assigning to
variables that were previoiusly unset. Excludes increment and decrement
operators, and the comma operator. Includes decimal, hexadecimal, and octal
constants. The expression can include parameter (and other) expansions.

## Redirections ##

Unix Base Specifications, Issue 7, [§2.7
Redirection](http://pubs.opengroup.org/onlinepubs/9699919799/utilities/V3_chap02.html#tag_18_07)

Any file descriptor must immediately precede the redirection symbol, with no intervening space. The argument following the redirection is considered a separate word an may be preceded by intervening space. Space is often used for file redirections and here documents, but rarely for redirecting with a duplicated file descriptor or closing a file descriptor. By default, input and output redirections respectively apply to standard input and output.

### File descriptors ###

* 0: Standard input
* 1: Standard output
* 2: Standard error

### Redirection types ###

* `>`, `>|`: Redirect output to file. If the vertical bar form (`>|`) is used, any existing file is truncated, otherwise the overwrite behaviour depends on the _noclobber_ (-C) shell option.
* `>>`: Append output to file.
* `<`: Read input from file.
* `<<`, `<<-`: Read input from here document. The here document may be indented with tabs if the form with a dash (`<<-`) is used.
* `<>`: Open a file for reading and writing, as file descriptor 0 (standard input) by default. File is created if necessary.
* `<&`, `>&`: Read input from, or redirect output to, a duplicate of a file descriptor.
* `<&-`, `>&-`: Close input or output file descriptor, if open. The dash is actually the redirect target argument and may be preceded by intervening space.

### Redirection order ###

Each redirection is effectively applied to temporary file descriptors in the order given. The first line redirects _stdout_ to null and then also redirects _stderr_ there. The second line redirects _stderr_ to the original _stdout_ target, but then redirects the actual _stdout_ to null.

``` bash
> /dev/null 2>&1
2>&1 > /dev/null
```
