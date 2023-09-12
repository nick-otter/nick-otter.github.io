---
layout: post
title:  "How To Write Effective Python"
categories: python
---

# How To Write Effective Python
{: style="text-align: center"}

# Introduction

I recently read "Effective Python: 50 Specific Ways To Write Better Python" by Brett Slatkin. Here are my personal takeaways abbreviated in a doc. I highly recommend reading Slatkin, it is a fantastic book. 

# Using Standard Slice

Slicing deals properly with `start` and `end` indexes. That are beyond the boundaries of a list. 

```
foo = ['a','b','c','d']

>>> foo[-20]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range

>>> foo[-20:]
['a', 'b', 'c', 'd']
```

Forms of slicing clear to a new reader of your code.
```
foo = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h']

>>> foo[:] ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h']
>>> foo[:5] ['a', 'b', 'c', 'd', 'e']
>>> foo[:-1] ['a', 'b', 'c', 'd', 'e', 'f', 'g']
>>> foo[4:] ['e', 'f', 'g', 'h']
>>> foo[-3:] ['f', 'g', 'h']
>>> foo[2:5] ['c', 'd', 'e']
>>> foo[2:-1] ['c', 'd', 'e', 'f', 'g']
>>> foo[-3:-1] ['f', 'g']
```

# Use List Comprehensions Instead Of Map And Filter

```
a = [1,2,3,4,5,6,7,8,9.10]
squares = [x**2 for x in a]
print(squares)

>>>
[1,4,9.16,25,36,49,64,81,100]
```

Instead of `squares = map(lambda x: x ** 2, a)`. Unlike `map` List Comprehensions let you easily filter items from the input list, removing corresponding outputs from the result.

```
even_squares = [x**2 for x in a if x % 2 == 0]
print(even_squares)

>>>
[4,16,36,64,100]
```

The `filter` built-in function can be used along with `map` to acheive the same outcome, but it is much harder to read.

# Avoid More Than Two Expressions In List Comprehensions

```
my_lists = [
    [[1,2,3], [4,5,6]]
]

flat = [x for sublist1 in my_lists
        for sublist2 in sublist 1
        for x ub sublist2]

# OR

flat = []
for sublist1 in my_lists:
    for sublist2 in sublist1:
        flat.extend(sublist2)
```

# Consider Generator Expressions instead Of List Comprehensions For Large Comprehensions

Reading a file and returning the number of characters on each line: Using `List Comprehension` would require holding the length of every line of the file in memory which will cause many problems at large scale. `Generator Expressions` don't materialize the whole output sequence when they're run. Instead, generator expressions evaluate to an iterator that yields one item at a time from the expression.

```
# List Comprehension

value = [len(x) for x in open('my_file.txt')]
print(value)

>>>
[100,57,15,1,12,75,5,86,89,11]

# Generator Expression

it = (len(x) for x in open('my_file.txt'))
print(it)

>>>
<generator object <genexpr> at 0x101b81480>

print(next(it))
print(next(it))

>>>
100
57
```

Another powerful outcome of generator expressions is that they can be composed together. E.g. Taking the iterator returned by the generator expression above and using it as input for another generator expression.

```
roots = ((x, x**0.5) for x in it)

print(next(roots))

>>>
(15, 3.872983346207417)
```

# Prefer Enumerate Over Range p. 20

```








---
