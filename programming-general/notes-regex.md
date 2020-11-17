# Quick notes - regex

Creation date: 2008-4-22

Keywords: *regex*

---


## Metacharacters

`.` : Any character, except when found in a brackets *[ ]* then it becomes a literal dot character.

`?` : Zero or one occurrence of preceding element. e.g. *abc?d* matches *abd* and *abcd*.

`*` : zero or more occurrences of preceding element. e.g. *abc\*d* matches *abd*, *abcd*, *abccd*, *abcccd*.

`+` : One or more occurrences of preceding element. e.g. *abc+d* matches *abcd* and *abccd*.

`|` : Matches the expression either before or after this metacharacter.

`^` : Matches next element only at beginning of a string or line.

`$` : Matches preceding element only at ending of a string or line.

`[ ]` : Any single character within the brackets. Dash character (*-*) can be use to specify range (note: implementation specific), e.g. *[a-z0-9]*. The “*-*” character will be treat as literal character if it appears as first or last character within brackets, or preceded with backslash escape character \\.

`[^]` : Any single character that is not within the brackets.

`\{x,y\}` : Matches preceding element at least *x* times but no more than *y* times.

`( )` : Define grouping and precedence.


## Character classes

`[:alnum:]` : A to Z, a to z, 0 to 9

`[:alpha:]` : A to Z, a to z

`[:digit:]` : 0 to 9

`[:lower:]` : Lower case characters

`[:upper:]` : Upper case characters

`[:punct:]` : Punctuation characters

`[:space:]` : White space characters

`[:blank:]` : Space and tab characters

`[:cntrl:]` : Control characters

`[:xdigit:]` : Hexadecimal digits

---


This note is summarized from wiki. More details can be found at:

http://en.wikipedia.org/wiki/Regular_expression
