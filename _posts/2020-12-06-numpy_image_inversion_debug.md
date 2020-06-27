---
layout: post
title: Solving A Small Numpy Bug
subtitle: Quirky edge with numpy data types
published: true
enable_latex: true
permalink: numpy_dt_quirks
frontpage: false
technical: true
funstuff: false
tags: 
  - programming
  - python
  - ml
  - bugfix
  - numpy
concepts:
  - image-processing
  - numpy
  - datatypes
---

# Interesting Debug
Over the weekend I saw this stack overflow question asking about the behavior of two python snippets, and I was stumped by the [question](https://stackoverflow.com/questions/65159521/inverting-an-image-with-abs-and-numpy/65161201). 

Essentially, the user was confused why the results of "inverting" an image (flipping the colors to "negative") from two similar snippets were different. If you looked at the SO post, the results weren't even close. Furthermore, testing the code on my own images also yielded different inversions. 

```python
# I've modified the code snippets to make it more reproducible
import numpy as np
from PIL import Image

# open an image
img = Image.open("image_name")
original = np.asarray(img, dtype='uint8')
print(original.shape) # some (x,y,3) if rgb, or (x,y,4) if it contains an alpha channel
copy1 = np.copy(original)
copy2 = np.copy(original)

# Method 1
for i in range(copy1.shape[0]): # invert colors
    for j in range(copy1.shape[1]):
        for k in range(3): # only first three dims r g b
            copy1[i,j,k] = np.abs(copy1[i,j,k] - 255)

# Method 2
copy2[:,:,:3] = np.abs(copy2[:,:,:3] - 255) # the problem

print(np.all(copy1 == copy2))
>> False
```
The question was interesting because it didn't seem like there were any (clear) logical errors in slicing or arithmetic, so I took a crack at it.

**TL;DR**: It turns out that the bug stemmed from quirks of integer datatypes (more specifically overflow with unsigned integers), so read more if you're interested. 

### Quick Refresher: What is "image (color) inversion"?
"Image inversion" (in this context) is when you take the [negative colors](https://en.wikipedia.org/wiki/Negative_(photography)) of the (R,G,B) channels of an image. I can't really describe it well, but it makes things look cool. 

{% assign caption1 = "A typical RGB Image" %}
{% include caption_image.html imgpath="/figures/pre_inverse.jpg" alt="fig2" caption=caption1 %}

{% assign caption2 = "Image with inverted colors" %}
{% include caption_image.html imgpath="/figures/inverted.jpg" alt="fig2" caption=caption2 %}

The way you calculate the negative color is to calculate $(255, 255, 255) - (R,G,B)$. 

# How to fix the behavior with `method 3`
The easiest way to fix the above snippet is to change method 2's line to `255 - copy2[:,:,:3]` and all of the problems go away! I'll call this approach `Method 3` for now. 

```python
copy2[:,:,:3] = 255 - copy2[:,:,:3] # no more problem

print(np.all(copy1 == copy2))
>> True
```

But that doesn't really explain **why** the code is behaving this way. 

# What was the issue? (Spoiler: Overflow with unsigned integer)

### Why it *should* work
Intuitively, the two snippets should behave the same! Moreover, calculating `np.abs(x - 255)` should behave the same as `255 - x`! After all $ f(x) = | x-255 |$ and $f(x) = 255 -x$ are identical for $x \leq 255$! Why does the minor fix solve the problem?

### Why it *shouldn't* work
The reason why `np.abs(x-255) != 255-x` is because the datatype of our array is `np.uint8` - [short for "unsigned Integer with 8 bits"](https://en.wikipedia.org/wiki/Integer_(computer_science)). "Unsigned" means that the values are only positive, while the 8 represents the number of bits in our datatype - meaning that our values can only stretch from [0,255]. Evaluating `uint8 - 255` doesn't result in a negative number, but an overflow of x - 255. The relationship is represented by `(x + 1) % 256` or $(x+1)\mod 256$.

A Demonstration:
```python
a = np.array([10, 100, 125, 250], dtype='uint8')
print(a - 255) # incorrect because of overflow
>> array([11, 101, 126, 251], dtype=uint8)

print(255 - a) # what we want
>> array([245, 155, 130,   5], dtype=uint8)
```
Even though `np.abs(data[:,:,:3] - 255)` "seems" like `255 - data[:,:,:3]`, the datatype makes this conversion incorrect. 

### Why did `method 1` worked
We can see understand why method 1 doesn't face the same overflow issue by examining how `numpy` handles data types. Essentially, when `numpy` calculated `value (type=uint8) - 255`, it realized the value would be negative and converted the array to type `np.int32`. As a result, `np.abs` took the proper absolute value, and the correct conversion was made.

```python
print(type(copy1[40,125,2]))
>> 'numpy.uint8' 
print(type(copy1[40,125,2] - 255))
>> 'numpy.int32'
print(type(255 - copy1[40,125,2]))
>> 'numpy.int32'
```

# Which should we use? (pick `Method 3`)
The natural extension follow-up question is, "which method should we use?", and I think hands down, `method 3` is the better option. `Method 3` is concise, cleaner, less work to write (no typing in loops), and most importantly faster (98% faster by letting `numpy` do the heavy lifting). The trade-off is that it's harder for a beginner to understand what the code is doing, but I think this is fine for the time being. 

We can even measure the performance increase! We can trust our results since we used the [`timeit` library](https://docs.python.org/3/library/timeit.html) to precisely measure execution time. Feel free to try on your own machine.

```python
import timeit
setup = '''
import numpy as np
from PIL import Image

def invert1(img):
    for i in range(img.shape[0]):
        for j in range(img.shape[1]):
            for k in range(3):
                img[i,j,k] = np.abs(img[i,j,k] - 255)
    return img

def invert1_optimized(img):
    for i in range(img.shape[0]):
        for j in range(img.shape[1]):
            for k in range(3):
                img[i,j,k] = 255 - img[i,j,k]
    return img
        
def invert2_correct(img):
    img[:,:,:3] = 255 - img[:,:,:3]
    return img

img = Image.open('demo.png')
original = np.asarray(img, dtype='uint8')
data = np.copy(original)
'''
run = 'invert1(data)'
res1 = timeit.timeit(stmt=run, setup=setup, number=100)

run2 = 'invert1_optimized(data)'
res2 = timeit.timeit(stmt=run2, setup=setup, number=100)

run3 = 'invert2_correct(data)'
res3 = timeit.timeit(stmt=run3, setup=setup, number=100)
print(res1, res2, res3)
>> 27.11316429999988 21.26800110000022 0.03969150000011723
```

# An Alternative Fix
Aside from my recommendation of reordering the arithmetic, an easy fix would be to load the original image as type `np.int32`. 
```python
original = np.asarray(img, dtype=np.int32)
```
However, I wouldn't recommend this solution as the memory footprint of your image becomes 4 times larger, and your code is (marginally) slower.

```python
setup = '''
import numpy as np
from PIL import Image

def invert2_correct(img):
    img[:,:,:3] = 255 - img[:,:,:3]
    return img

img = Image.open('demo.png')
original_uint8 = np.asarray(img, dtype='uint8')
data_uint8 = np.copy(original_uint8)
original_int32 = np.asarray(img, dtype='int32')
data_int32 = np.copy(original_int32)

'''
run_uint8 = 'invert2_correct(data_uint8)'
run_int32 = 'invert2_correct(data_int32)'
res_uint8 = timeit.timeit(stmt=run_uint8, setup=setup, number=1000)
res_int32 = timeit.timeit(stmt=run_int32, setup=setup, number=1000)
print(res_uint8, res_int32)
>> 0.3727033000004667 0.5797044999999343
```

# Conclusion
I need better things to do on the weekend and pay attention to your `ndarray` datatypes if you're running into bugs!. 
