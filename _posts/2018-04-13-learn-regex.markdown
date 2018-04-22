---
layout: post
title:  "Learn Regex"
date:   2018-04-13 23:20:32 -0500
categories: Regex
---

### What is Regular Expressions
A regular expression(regex for short) is a special text string for describing a search pattern.

> You can think of regular expressions as wildcards on steroids. You are probably familiar with wildcard notations such as *.txt to find all text files in a file manager. The regex equivalent is ^.\*\\.txt$.

**Metacharacter**

Metacharacter | Description
---|---
. | Match any single character
^ | Matches the starting porsition within the string
[ ] | Match a single charcter that is contained whithin the brackets
[^ ] | Match a single character that is not contained within the brackets. 
$ | Matches the ending positionof the string or the position just before a string-ending newline
( ) | Defines a marked subexpression.
\n | 
* | Matches the preceding element zero or more times.
{m,n} | Matches the preceding element at leat M and not more than n
**Example:** How to validate an email address
```regex
^[a-z0-9]{1,20}@[a-z]{1,10}.com
```
```[a-z0-9]```: Match the charactor from a to z and from 1 to 9.
```{1, 20}```: The length need to be between 1 and 20.
```@```: There is a @ after the preceding part.

**Place to do practice:**

http://regexr.com/