---
layout: post
title: Function Decorators To Reduce Labor 
subtitle: syntactic sugar is nice
published: true
enable_latex: false
permalink: python_decorators
frontpage: false
technical: true
funstuff: false
tags:
  - programming
  - python
  - ml
  - data-sci
  - numpy
concepts:
  - python
  - syntactic sugar
  - debugging
---

# TL;DR
I wrote a set of Python decorators that I use while writing/debugging code. [Here's the link to the gist](https://gist.github.com/tedkim97/32e4ce8d371d2d726ba26b1a556f4790). Although if you have an incredibly complex piece of python code, odds are you should just use a IDE or debugger... 

Read more if you want a dramatization with the reasoning. 

# Introduction & Why
I end up using python as a prototyping language because it has dynamic typing and deep library support. In addition, Python is a popular language for statistical libraries, so I also end up working with numpy arrays frequently.

A part of the writing process is annoying because I need to frequently debug complicated processes (and I'm not in a situation to use a python debugger or IDE). Often times I fall into a process like this...

```python
data = ... # some data matrix
print(data.shape)
result = SOME_OPERATION(data)
print(result.shape) # Something is wrong

# Change the code

data = ... # some data matrix
print(data.shape)
result = SOME_OPERATION_MODIFIED(data)
print(result.shape) # Something is still wrong

# repeat
```

or maybe something like this...
```python
very_long_training_process()
print("done training")
send_me_an_email('ted@email.com')
```

or maybe something like this...
```python
out1 = function_that_does_something(1,2,3)
out2 = function_that_does_something(0,0,0)
out3 = function_that_does_something(-1,-2,-3)
out4 = function_that_does_something('a','b','c')
print(out1, out2, out3, out4)
```
At some point I got tired of repeating these patterns, and knew there must be some *easy* way of doing these things.

### An example of a "solution"
The solution I was thinking of was something like this:

```python
def print_output(func, *args, **kwargs):
	a = func(*args, **kwargs)
	print(a)
	return a
```

except that it's so **clunky** to use. Every time I want to debug something, I need to write a handful more code to get what I want. For example:

```python
# function_that_does_something will be abbreviated to ftds
out1 = print_output(ftds, 1, 2, 3)
out2 = print_output(ftds, 0, 0, 0) 
out3 = print_output(ftds, -1, -2, -3)
out4 = print_output(ftds, 'a','b','c')
print(out1, out2, out3, out4)
```

which isn't exactly what I imagine as an **easy** way of getting what I want.

### Function Decorators & Syntactic Sugar
Fortunately, Python has a [syntactic sugar](https://en.wikipedia.org/wiki/Syntactic_sugar) to express this logic. By using `@decorate_name` at the top of a function declaration, you can express equivalent logic in an easy way `decorate_name(func)`:
```python
def capture_output(func_example):
	def wrapper():
		print(func_example())

	return wrapper

@capture_output
def return10():
	return 10

capture_output(return10)() # equivalent expression as above
``` 

### Our Toy Example:
Now instead of writing the pattern here: 
```python
out1 = function_that_does_something(1,2,3)
out2 = function_that_does_something(0,0,0)
out3 = function_that_does_something(-1,-2,-3)
out4 = function_that_does_something('a','b','c')
print(out1, out2, out3, out4)
>> ('foo', 'bar', 'baz', 'fizz')
```

we can do this:
```python
import functools

# print the output of func
def print_output(func):    
    @functools.wraps(func)
    def pout(*args, **kwargs):
        output = func(*args, **kwargs)
        print(f"{func.__name__} output = {output}")
        return output
    return pout

@print_output
def function_that_does_something(index, str1, str2):
    # do something
    
out1 = function_that_does_something(1,2,3)
out2 = function_that_does_something(0,0,0)
out3 = function_that_does_something(-1,-2,-3)
out4 = function_that_does_something('a','b','c')
>> 'function_that_does_something output = foo'
>> 'function_that_does_something output = bar'
>> 'function_that_does_something output = baz'
>> 'function_that_does_something output = fizz'
```

### Actual Examples:
If I have a model that takes a long time to train, I send a notification to my email when the function is done running like this:

```python
# Decorator that calls endExec after func is finished running
def post_exec(endExec):
    if(not callable(endExec)):
        warnings.warn("The argument is not callable")
                
    def decorator_execute(func):
        @functools.wraps(func)
        def wrapped(*args, **kwargs):
            output = func(*args, **kwargs)
            endExec()
            return output
        return wrapped
    return decorator_execute

def send_email(username, pw, api_key, msg):
    sess = authenticate(username, pw, api_key)
    sess.send_email(msg)
    
@dec.post_exec(lambda : send_email('username', 'password', 'api_key', 'done'))
def train():
    # Do something for a long time

train() # will send an email when done 

```
# Conclusion
Feel free to use these if you think they can benefit you. [Github gist is right here.](https://gist.github.com/tedkim97/32e4ce8d371d2d726ba26b1a556f4790) Maybe at a comment if this helped.