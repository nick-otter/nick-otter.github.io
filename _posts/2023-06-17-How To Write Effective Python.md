---
layout: post
title:  "How To Write Effective Python"
tags: development
---

I recently read "Effective Python: 50 Specific Ways To Write Better Python" by Brett Slatkin. Here are my personal takeaways abbreviated in a doc. I highly recommend reading Slatkin, it is a fantastic book. 

### Using Standard Slice

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

### Use List Comprehensions Instead Of Map And Filter

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

### Avoid More Than Two Expressions In List Comprehensions

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

### Consider Generator Expressions instead Of List Comprehensions For Large Comprehensions

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

### Prefer Enumerate Over Range

Often, you'll want to iterate over a list and also know the index of the current item in the list. For example, say you want to print the ranking of your favourite ice cream flavors. One way to do it using range. 

```
flavor_list = ['vanilla', 'chocolate', 'pecan', 'strawberry']

for i in range(len(flavor_list)):
  flavor = flavor_list[i]
  print('%d: %s' % (i + 1, flavor))

# OR

for i, flavor in enumerate(flavor_list):
  print('%d: %s' % (i + 1, flavor))

for i, flavor in enumerate(flavor_list, 1):
  print('%d: %s' % (i, flavor))
```

`enumerate` provides concise syntax for looping over an interator and getting the index of each item from the iterator as you go. You can supply a second parameter to `enumerate` to specify the number from which to being counting from (zero is the default).

```
>>> for i in enumerate(flavor_list):
...     print(i)
...
(0, 'vanilla')
(1, 'chocolate')
(2, 'pecan')
(3, 'strawberry')

>>> for i in enumerate(flavor_list, 1):
...     print(i)
...
(1, 'vanilla')
(2, 'chocolate')
(3, 'pecan')
(4, 'strawberry')
```

### Use Zip To Process Iterators In Parallel

To iterate over multiple lists in parallel, `zip` is preferred. In Python 3, `zip` is a lazy generator that produces tuples. In Python 2, `zip` returns the full result as a list of tuples. `zip` truncates the output silently if you supply it with iterators of different lengths. 

```
>>> names = ['Celia', 'Lise', 'Marie']
>>> letters = [len(n) for n in names]
>>>
>>> longest_name = None
>>> max_letters = 0
>>>
>>> for i, name in enumerate(names):
...     count = letters[i]
...     longest_name = name
...     max_letters = count
...
>>> for name, count in zip(names, letters):
...     print(name)
...     print(count)
...
Celia
5
Lise
4
Marie
5
```

### Fail Upwards In `try` `except` `finally` Blocks

There are four distinct times that you may want to take action during exception handling in Python. These are captured in the functionality of `try` `except` `else` and `finally` blocks.

**Use try/finally when you want exceptions to propagate up but you also want to run cleanup code**
```
handle = open('random_data.txt') # May raise IOError
try:
  data = handle.read() # May raise UnicodeDecodeError
finally:
  handle.close()       # Always runs after try
```

**Use try/except/else to make it clear which exceptions will be handled by your code and which will propagate up**<br>
When the `try` block doesn't raise an exception the `else` block will run. The `else` block helps you minimize the amount of code in the `try` block and improves readability. 
```
def load_json_key(data, key):
  try:
     result_dict = json.loads(data) # May raise ValueError
  except ValueError as e:
     raise KeyError from e
  else:
    return result_dict[key]         # May raise KeyError
```

The `else` block helps you minimize the amount of code in `try` blocks and visually distinguish the success case from the `try` `except` blocks. An `else` block can be used to perform additional actions after a successful `try` block but before common cleanup in a `finally` block.
