---
layout: post
title:  "Case Insensitive String Comparison In C"
date:   2024-04-07 00:00:00 -0400
categories: [c, snippets]
---

A simple function to case insensitively compare 2 strings, up to the n chars. This function follows the C standard of returning 0 if the strings are equal, a negative number if the first character that does not match has a lower value in str1 than in str2, or a positive number if the first character that does not match has a greater value in str1 than in str2.

Note that the type of n is `ptrdiff_t` in this function. This is simply because in my current project, an ll(1) parser for SQL statements, my n value is of this type. Typically this would be a type of `size_t` instead. `ptrdiff_t` cannot blindly be converted to a `size_t` as the latter is unsigned while the former is not. However, I am not using in a way where I'll have a negative `ptrdiff_t` so I could safely convert this to a `size_t` and not cause issues. 

```c
int strncmpci(char const *a, char const *b, ptrdiff_t n) {
  while (n && *a &&
         (tolower(*(unsigned char *)a) == tolower(*(unsigned char *)b))) {
    a++;
    b++;
    n--;
  }
  if (n == 0) {
    return 0;
  } else {
    return (tolower(*(unsigned char *)a) - tolower(*(unsigned char *)b));
  }
}
```
