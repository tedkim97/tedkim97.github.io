---
layout: post
title: Critique of "The Fastest Fibonacci Sequence"
subtitle: (but really I'm just critiquing the Binet Formula)
published: true
enable_latex: true
permalink: fib_critique
frontpage: false
technical: true
funstuff: true
tags:
  - programming
  - python
  - c
  - optimization
  - math
  - fibonacci
concepts:
  - fibonacci
  - approximations
  - math
---

[I read this interesting post about the new "fastest way" to compute the N-th term of the Fibonacci sequence](https://medium.com/cantors-paradise/fastest-fibonacci-9273e2a1805d). Overall, the post was a good read and had clever optimizations; however, I feel that the author makes an unfair claim for "The Fastest Way to Compute the Fibonacci Sequence". Even claiming that its faster than the ["fast doubling" method for computing the Fibonacci sequence](https://www.nayuki.io/page/fast-fibonacci-algorithms) for large N.

# A Summary of the approaches

### Medium article
The core of the approach is to use the Binet Formula to calculate the terms of Fibonacci sequence ([link to proof](http://mathonline.wikidot.com/a-closed-form-of-the-fibonacci-sequence)) and apply some conversions using the binomial theorem and log properties. If you want to see exactly how the approach is defined, [I encourage you to read the article!](https://medium.com/cantors-paradise/fastest-fibonacci-9273e2a1805d).

##### The Binet Formula:
$$ 
fib(n) = \frac{1}{\sqrt{5}}\left[\left(\frac{1+\sqrt{5}}{2}\right)^{n} -\left(\frac{1-\sqrt{5}}{2}\right)^{n}\right] 
$$

##### The End Code (Binomial Theorem & Logs)

```python
import numpy as np

def log_binom(n, ks):
    r = np.arange(n) + 1
    r = np.log(r)
    s = np.sum(r)
    r = np.cumsum(r)
    z = np.zeros(r.shape[0] + 1)
    z[1:] = r
    z1 = z[::-1]
    z = np.add(z, z1)
    z = np.subtract(s, z)
    return z[ks]

# renamed to fib_medium because it was on a medium article
def fib_medium(n):
    n += 1
    ks = np.arange(np.ceil(n / 2)).astype(np.uint32)
    coeffs = log_binom(n,  2 * ks + 1).astype(np.float64)
    terms = np.multiply(np.log(np.sqrt(5)), 2*ks+1)
    res = np.add(coeffs, terms)
    res = np.subtract(res, np.log(2)*(n-1) + np.log(np.sqrt(5)))
    m = np.max(res)
    res = np.subtract(res, m)
    res = np.exp(res)
    res = np.sum(res)
    return res, m
```

### Fast Double Matrix Exponentiation
[If you want a more complete breakdown of this method, I recommend you read the Project Nayuki post describing and proving this fast exponentiation algorithm!](https://www.nayuki.io/page/fast-fibonacci-algorithms) 

Anyways - the idea behind the fast exponentiation method is that $fib(n)$ - $F(n)$ for short - can be computed through identities derived from matrix multiplication. More specifically: 

$$ \left[ \begin{matrix} 1 & 1 \\ 1 & 0 \end{matrix} \right]^{n} = \left[ \begin{matrix} F(n+1) & F(n) \\ F(n) & F(n-1) \end{matrix} \right] $$

Given (1), we can derive the $F(2n)$ by squaring the Fibonacci matrix.  

$$ \left[ \begin{matrix} 1 & 1 \\ 1 & 0 \end{matrix} \right]^{2n} = \left[ \begin{matrix} F(2n+1) & F(2n) \\ F(2n) & F(2n-1)\end{matrix} \right] \\ $$

We can also use Equation (1) to derive an alternative expression for the square of our matrix:

$$ \left( \left[ \begin{matrix} 1 & 1 \\ 1 & 0 \end{matrix} \right]^n \right)^2 = \left[ \begin{matrix} F(n+1) & F(n) \\ F(n) & F(n-1) \end{matrix} \right]^2 \\ = \left[ \begin{matrix} F(n+1)^2+F(n)^2 & F(n+1) \cdot F(n)+F(n) \cdot F(n-1) \\ F(n) \cdot F(n+1)+F(n-1) \cdot F(n) & F(n)^2+F(n-1)^2 \end{matrix} \right]
$$

With our two matrices, we observe that $F(2n)$ and $F(2n+1)$ can be written in terms of $F(n)$ and $F(n+1)$.

$$ 
F(2n+1) = F(n+1)^2+F(n)^2 \\
F(2n) = F(n) \cdot [2F(n+1) - F(n)]
$$

Just for clarification, $F(2n)$ *seems* like it relies on $F(n-1)$, but we can substitute an identity from the Fibonacci sequence ($F(n-1) = F(n+1) - F(n)$) to get rid of it.

$$ 
F(2n) = F(n) \cdot F(n+1) + F(n-1) \cdot F(n) \\
= F(n) \cdot F(n+1) + F(n) \cdot [F(n+1) - F(n)] \\
= F(n) \cdot [2F(n+1) - F(n)]\\
$$

This technique makes computing large N of the Fibonacci sequence really fast! For instance, calculating $ Fib(10000) $ requires us to compute $ Fib(5001), Fib(5000)$, which requires us to compute $Fib(2501), Fib(2500)$ until we reach $Fib(0), Fib(1)$. At most we make $\left \lfloor \log_{2} N \right \rfloor + 1$ calculations (opposed to the $N$ calculations we would need for the traditional approach). 
	
# First Critique: Approximate vs Exact Solutions 
People can compute the $N$th term of the Fibonacci sequence with a closed form expression, using the Binet formula. However, most implementations of the Binet formula give an **approximation** of the Fibonacci sequence for large N's. Capel's method relies on storing fibonacci numbers as an exponentiation of two `float`'s like $ f_{1} \times e^{f_{2}} $. 

This is a great optimization and a nifty way of storing large numbers in less space, but [floating point arithmetic](https://en.wikipedia.org/wiki/Floating-point_arithmetic) doesn't provide an infinite amount of precision[^1]. As a result, Capel's method for computing the sequence becomes inaccurate from the actual sequence for around the `67th` term of the Fibonacci sequence. 

[^1]: To be fair, calculating Fibonacci numbers with unsigned integers will cause errors, as the numbers of the Fibonacci sequence cause the values to overflow for most languages

```python
from functools import lru_cache

@lru_cache
# Note: python doesn't support tail call optimization
# Correct O(n) fibonacci sequence 
def fib_tail_recur(n, a0, a1):
    if(n == 0):
        return a0
    return fib_tail_recur(n-1, a1, a1+a0)

MIN = 0
MAX = 70

# fib_medium outputs are off by 1, and also doesn't compute Fib(0) 
float_form = [fib_medium(n) for n in range(MIN, MAX-1)]
expanded_form = np.array([int(np.round(res * np.exp(m))) for (res, m) in float_form])
expanded_form.insert(0,0)

tail_recur = np.array([fib_tail_recur(n, 0, 1) for n in range(MIN, MAX)])

np.all(expanded_form == tail_recur)
>> False

expanded_form == tail_recur
>> array([ True, ..., True,  True,  True,  True, False, False, False])
```

The proposed method is fast, but being faster (and incorrect) isn't exactly a fair comparison. To be clear, I think approximations are important and necessary; however, this one seems to make a large trade in accuracy for a small increase in performance. 

# Second Critique: An Easy Performance Boost (see Erratum)
Running the snippet provided by the article on my local machine shows that the modified Binet formula is 21% faster.

```python
import timeit
import functools

n = 2000000

t_fib_medium = timeit.Timer(functools.partial(fib_medium, n)) 
print(f'performance fib_medium(): {t_fib_medium.timeit(10)}')

# f(ast) d(ouble)
# fibonacci_fde = fibonacci f(ast) d(ouble) e(xponentiation)
t_fib_fd = timeit.Timer(functools.partial(fibonacci_fde, n)) 
print(f'performance fibonacci_fde(): {t_fib.timeit(10)}')
>> performance performance fib_medium(): 3.6354350000000295
>> performance fibonacci_fde(): 4.60260459999995
```

However, if we add memoization with [`functools.lru_cache()`](https://docs.python.org/3/library/functools.html) decorator to the fast double exponentiation algorithm, the algorithm becomes 87% faster than the new "fastest Fibonacci sequence". 

```python
import timeit
import functools
def fibonacci_fde(n):
	return _fib(n)[0]

@functools.lru_cache()
def _fib(n):
	# Licensed code that you can find here: 
	# https://www.nayuki.io/res/fast-fibonacci-algorithms/fastfibonacci.py
    if n == 0:
        return (0, 1)
    else:
        a, b = _fib(n // 2)
        c = a * (b * 2 - a)
        d = a * a + b * b
        if n % 2 == 0:
            return (c, d)
        else:
            return (d, c + d)

n = 2000000

t_fib_medium = timeit.Timer(functools.partial(fib_medium, n)) 
print(f'performance fib_medium(): {t_fib_medium.timeit(10)}')

t_fib_fd = timeit.Timer(functools.partial(fibonacci_fde, n)) 
print(f'performance fibonacci_fde(): {t_fib_fd.timeit(10)}')
>> performance performance fib_medium(): 3.6504553000000897
>> performance fibonacci_fde(): 0.4611188000000084
```

## Erratum:
After posting this, I've realized that `lru_cache` was caching results from `_fib` between different runs - making the algorithm seem much faster than it is during timing. To make testing fair, I've added the function decorator to both functions, and the Binomial Exponent-Reduced Approximation approach is still faster. It is worth noting that converting this exponent-reduced returns an overflow error for $ N > 1500 $.

# Third Critique: Code Results in Errors for moderate sized N
My third critique is that converting our reduced values into a full Fibonacci number results in errors (limitations of numpy floats). Using `math.exp` also provides an error limit. As a result, we can't even use this implementation to retrieve the actuals terms of the Fibonacci past \~1500. 

```python
res, m = fib_medium(1500)

int(np.round(res * np.exp(m))) 
>> RuntimeWarning: overflow encountered in exp

int(np.round(res * math.exp(m))) # returns error
>> OverflowError: math range error

```

# Conclusion
Overall I like the Medium post about a new strategy for computing the Fibonacci sequence - there was cool math and clever optimizations. However, the solution only provides an*approximation* that barely beats the Fast Doubling method, and isn't as usable. 



