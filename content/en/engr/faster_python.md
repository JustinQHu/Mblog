---
title: "Faster Python: Python 3.11 Release and Future Roadmap"
date: 2022-12-03T19:59:44-05:00
categories: ['engineering', '2022']
tags: ['engineering', 'python', 'Cpython','programming', 'python3.11']
draft: false
---

Python is a popular programming language, especially in scripting,  web development,  data analytics, and 
ML/AI domains. 

According to [TIOBE Programming Language Index](https://www.tiobe.com/tiobe-index/), Python has become the most popular programming since 2022, exceeding good old C:
![programming language ranking](/engr/programming_language_ranking.png "programming language ranking")

Python has gained popularity because of its flat learning curve (compared to other languages such as C/C++), human-readable
code, and thriving ecosystems. However, Developers have always complained about python not being fast or even slower than other
interpreting languages like JS or Lua.


## Python 3.11

[Python 3.11](https://docs.python.org/3/whatsnew/3.11.html) has been released today (Dec 3, 2022), bringing enormous performance improvement:
>Python 3.11 is between 10-60% faster than Python 3.10. On average, 
> we measured a 1.25x speedup on the standard benchmark suite

In short, Python 3.11 introduces improvements in the CPython compiler and interpreter, so it's faster. 

If you want to learn more details, there is a fascinating interview done by [Guido van Rossum](https://en.wikipedia.org/wiki/Guido_van_Rossum) and 
[Lex Fridman](https://en.wikipedia.org/wiki/Lex_Fridman), discussing **why Python 3.11 is so fast**:

{{< youtube TLhRuZ9cJWc >}}

## High-Performance Python Roadmap

There is more good news for the python community. 
Python 3.11 is a significant improvement, and it's not the whole picture but rather an important milestone. 

There is a [roadmap](https://github.com/markshannon/faster-cpython/blob/master/plan.md) for evolving towards high-performance python:

>Stage 1 -- Python 3.10
The key improvement for 3.10 will be an adaptive, specializing interpreter. The interpreter will adapt to types and values during execution, exploiting type stability in the program, without needing runtime code generation.

>Stage 2 -- Python 3.11
This stage will make many improvements to the runtime and key objects. Stage two will be characterized by lots of "tweaks", rather than any "headline" improvement. The planned improvements include:
Improved performance for integers of less than one machine word.
Improved peformance for binary operators.
Faster calls and returns, through better handling of frames.
Better object memory layout and reduced memory management overhead.
Zero overhead exception handling.
Further enhancements to the interpreter
Other small enhancements.

>Stage 3 -- Python 3.12 (requires runtime code generation)
Simple "JIT" compiler for small regions. Compile small regions of specialized code, using a relatively simple, fast compiler.

>Stage 4 -- Python 3.13 (requires runtime code generation)
Extend regions for compilation. Enhance compiler to generate superior machine code.


## Conclusion

As a Python user, I am happy for the Python community with the release of Python 3.11. 
Python will become faster and better as a JIT compiler will be introduced in Python3.12 and Python3.13.