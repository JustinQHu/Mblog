---
title: "FastAPI 初探"
date: 2023-12-01T16:03:50-05:00
categories: ['工程','技术'] 
tags: ['工程','技术', 'python', 'fastapi','FastAPI', 'web server']
draft: false
---

## What is FastAPI?

According to its official website,
> [FastAPI](https://fastapi.tiangolo.com/) is a modern, fast (high-performance), web framework for building APIs with
> Python 3.8+ based on standard Python type hints.

## Hello World with FastAPI

### Installation

#### Requirements

>Python3.8+

#### Install FastAPI

```shell
pip install fastapi
```

#### Install ASGI Server

```shell
pip install "uvicorn[standard]"
```

### Define a FastAPI app

Create a file *main.py* with:

```python
from typing import Union

from fastapi import FastAPI

app = FastAPI()
```

### Create API Endpoints

In *main.py* add the following 2 endpoints implementation

```python
@app.get("/")
def read_root():
    return {"Hello": "World"}


@app.get("/items/{item_id}")
def read_item(item_id: int, q: Union[str, None] = None):
    return {"item_id": item_id, "q": q}
```

### API Endpoints with Automatic Validation

final code looks like this:

```python
from typing import Union
from fastapi import FastAPI
from pydantic import BaseModel


app = FastAPI()


class Book(BaseModel):
    name: str
    price: float
    is_offer: Union[bool, None] = None


@app.get("/")
def home():
    return {"Hello world": "Dec 1st 2023"}


@app.get("/book/{book_id}")
def read_item(book_id: int, q: Union[str, None] = None):
    return {"book_id": book_id, "q": q}


@app.put("/book/{book_id}")
def update_item(book_id: int, book: Book):
    return {"book_id": book_id,  "book_name": book.name}

```

### Test

Run the server with

```shell
uvicorn main:app --reload
```

>INFO:     Will watch for changes in these directories: ['/Users/xxx/PycharmProjects/pythonProject']  
INFO:     Uvicorn running on <http://127.0.0.1:8000> (Press CTRL+C to quit)  
INFO:     Started reloader process [99265] using WatchFiles  
INFO:     Started server process [99267]  
INFO:     Waiting for application startup.  
INFO:     Application startup complete.  

Open it in the browser and test the endpoints defined above.

### API Documentation

FastAPI has interactive API docs built-in by default.

#### Interactive Docs Powered by Swagger UI

Go to */docs*,  we can the interactive API docs:

![Automatic API Documentation](/se/fastapi101/apidoc_swaggerui.png "Automatic Interactive Doc with Swagger UI")

#### Automatic API doc with ReDoc

Go to */redoc*,  we can see another automatic API documentation:

![Automatic API Documentation](/se/fastapi101/apidoc_redoc.png "Automatic Doc with Redoc")

## General Thoughts

FastAPI is generally a lightweight framework to develop APIs with Python. Some attractive advantages of FastAPI
for me compared to a comprehensive framework like [Django](https://www.djangoproject.com/):
>
>1. fast to set up;  fast to build APIs, prototypes, and POCs
>2. automatic validation with [Pydantic](https://docs.pydantic.dev/latest/)
>3. Automatic API docs

I would definitely consider FastAPI to be my first option if I want to test some ideas quick,  and honest I think
it's also quite attractive to build production systems as well, given more and more web applications are
single page applications with [React](https://react.dev/), [Vue](https://vuejs.org/), or [Angular](https://angularjs.org/)
that only requires APIs from backend. Django is a little too heavy for such case
