---
title: Python-Switch-Case
tags: Python
abbrlink: 6bd0bccb
date: 2021-02-10 23:21:55
---

# Python's Switch/Case

**It doesn't have switch or case statement which unlike other programming language. There are a few ways to simulate the effect. Here is [official answer](https://docs.python.org/2/faq/design.html#why-isn-t-there-a-switch-or-case-statement-in-python).**

## Function

```python
def switch(switcher, case, default=None):
    c = switcher.get(case, default)
    if str(type(c)) == "<class 'function'>":
        return c()
    return c
```

```python
def func1():
  return 'zero'

switcher = {
  0: func1,
  1: 'one',
  2: lambda: "two",
}
switch(switcher, type_name)
```

## Class

```python
class Switcher(object):
    def numbers_to_methods_to_strings(self, argument):
        """Dispatch method"""
        # prefix the method_name with 'number_' because method names
        # cannot begin with an integer.
        method_name = 'number_' + str(argument)
        # Get the method from 'self'. Default to a lambda.
        method = getattr(self, method_name, lambda: "nothing")
        # Call the method as we return it
        return method()

    def number_0(self):
        return "zero"

    def number_1(self):
        return "one"

    def number_2(self):
        return "two"
```

## References

[Why Doesn't Python Have Switch/Case?](https://www.pydanny.com/why-doesnt-python-have-switch-case.html)
