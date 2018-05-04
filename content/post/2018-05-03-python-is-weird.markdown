---
title: Python is Weird (an unabashedly biased intro to Python for R users)
author: Eric R. Scott
date: '2018-05-03'
slug: python-is-weird
categories:
  - Data Science
tags:
  - Python
  - R
draft: false
header:
  image: "headers/python_is_weird.png"
---


Last semester I took a class that used Python. It was my first time really seriously using any programing language other than R. The students were about half engineers and half biologists.  The vast majority of the biologists knew R to varying degrees, but had no experience with Python, and the engineers seemed to generally have some experience with Python, or at least with languages more similar to it than R. I wish that the instructor could have taught every Python lecture like "Ok, today we're going to learn the Python equivalent of doing ____ in R", but of course that wouldn't be fair to about half the students.

So for anyone else making the leap from R to Python, here are three things that are going to feel really weird about Python.

# 1. Indexing is not intuitive in Python
Let me just show you first and see if you can figure out what is going on:

*R Code:*

```r
# R
x = c(10, 20, 30, 40, 50)
x[1]
x[1:3]
x[4:5]
```

```
## [1] 10
## [1] 10 20 30
## [1] 40 50
```
Cool, cool.

*Equivalent in Python:*

```python
# Python
x = [10, 20, 30, 40, 50]
print(x[1])
```

```
## 20
```

```python
print(x[0])
```

```
## 10
```

```python
print(x[3:4])
```

```
## [40]
```

```python
print(x[3:5])
```

```
## [40, 50]
```
Wait, what? Two things are really weird about this.  First, the first position in the vector is not position 1, it is position 0.  Second, `x[3:4]` returns only a single number.  Why?!  Because in Python, the second number in the index is not inclusive, so if you want to get the 4th and 5th values of `x` (index positions 3 and 4 in Python world), then you have to use `x[3:5]` **even though there is NO POSITION 5**.  Terrible.

*Weird thing 1.1: Python is much more geared toward writing programs than R.  That means you can't really run python code one line at a time like R and you have to explicitly `print()` things that you want to be output to the screen.*

# 2. You need to load a package just to do vector math
R is built for doing math and statistics, so vectors and matrices are built in and you can do math on them!

*R Code:*

```r
# R
x = c(1, 2, 3)
x + 10
x * 2
y = c(5, 6, 7)
x + y
#Yay vector arithmetic!
```

```
## [1] 11 12 13
## [1] 2 4 6
## [1]  6  8 10
```

Python is **not** built with math and statistics in mind, and this doesn't work without using a package.

*Equivalent in Python:*

```python
# Python
x = [1, 2, 3]
print(x + [10]) 
```

```
## [1, 2, 3, 10]
```

```python
print(x*3) 
```

```
## [1, 2, 3, 1, 2, 3, 1, 2, 3]
```

```python
y = [5, 6, 7]
print(x + y) 
```

```
## [1, 2, 3, 5, 6, 7]
```

Clearly `+` is doing something different in base Python---it's concatenating `x` and `10`.  Similarly, `*` is not multiplying, but concatenating three `x`s in a row. This is completely ridiculous behavior for numbers, but when you're working with strings, it's actually pretty freakin' great.


```python
# Python
print(("Yay "+"Python! ") * 5)
```

```
## Yay Python! Yay Python! Yay Python! Yay Python! Yay Python!
```
If you want numerical vectors to work like they should, you have to use a special kind of vector called a **numpy array**.  Numpy is a package for Python that provides a bunch of functions that work on numbers.

```python
# Python
import numpy as np
x = np.array([1, 2, 3])
print(x + 10)
```

```
## [11 12 13]
```

```python
print(x * 2) 
```

```
## [2 4 6]
```

```python
y = np.array([5, 6, 7])
print(x + y)
```

```
## [ 6  8 10]
```
If you do math to numpy arrays, you get what you'd expect as an R user.

*Wierd thing 2.1: note that the `packagename.function()` form is equivalent to `packagename::function()` in R, but unlike R, it is always required.  That is, as far as I know, there is nothing you can do to make `array([1,2,3])` work without the preceding `np.`*

# 3. Default assignment behavior is aliasing

I'm still trying to wrap my mind around this one, so rather than trying to explain it, let me show you an example first:

*R Code:*

```r
# R
a1 = c(1,2,3)
a2 = a1
a2[1] = 100
a1
a2
```

```
## [1] 1 2 3
## [1] 100   2   3
```
`a1` is, of course, unchanged by changing a value in `a2`.  Let's see if that's true in Python.

*Equivalent in Python:*

```python
# Python
import numpy as np
a1 = np.array([1,2,3])
a2 = a1
a2[1] = 100
print(a1)
```

```
## [  1 100   3]
```

```python
print(a2)
```

```
## [  1 100   3]
```
Changing a value in `a2` *changes* the same value in `a1`! In this case, `a2` is an *alias* of `a1`, not a copy. This only happens when you do `object1 = object2` and not when you do something to `object2` as you're assigning it. Here's another example:


```python
# Python
import numpy as np
a1 = np.array([1,2,3])
a2 = a1 + 2
a2[1] = 100
print(a1)
```

```
## [1 2 3]
```

```python
print(a2)
```

```
## [  3 100   5]
```
Now `a2` is a separate object from `a1` instead of just an alias. If you want to make an *exact* copy, you have to do that explicitly with `a2 = np.copy(a1)` or `a2 = a1[:]`

# Try Python!
As many people in the data science world have pointed out, it's not R vs. Python, it's [R *and* Python](https://www.datasciencecentral.com/profiles/blogs/r-vs-python-r-and-python-and-something-else). From my limited experience, the benefits of Python over R I've are that it seems to be faster, defining classes and functions seems less painful, and it's great at working with strings out of the box. I don't really plan on working in Python more unless I have to, but knowing a bit of the language will be useful for talking shop with people who use it!
