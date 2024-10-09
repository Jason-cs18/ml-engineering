---
title: Web Development
layout: default
parent: Engineer
nav_order: 8
---
In practical projects, we often need to divid frontend and backend development. In the example below, we teach you how to implement backend logic for web-based AI applications.

## Table of contents
- [Table of contents](#table-of-contents)
- [Implement backend logic with FastAPI](#implement-backend-logic-with-fastapi)
  - [Connect to Database](#connect-to-database)
  - [Add AI endpoints to FastAPI](#add-ai-endpoints-to-fastapi)
  - [Mount Gradio to FastAPI](#mount-gradio-to-fastapi)
- [Conclusion](#conclusion)
- [Reference](#reference)


## Implement backend logic with FastAPI
FastAPI is a high-performance Python web framework. It is built on top of [Starlette](https://www.starlette.io/) and [Pydantic](https://pydantic-docs.helpmanual.io/), which are both fast and easy to use.

```python
from typing import Union

from fastapi import FastAPI

app = FastAPI()


@app.get("/")
def read_root():
    return {"Hello": "World"}


@app.get("/items/{item_id}")
def read_item(item_id: int, q: Union[str, None] = None):
    return {"item_id": item_id, "q": q}
```

### Connect to Database
Because database logic is an standard workflow for FastAPI, I recommend to checkout the official [FastAPI documentation](https://fastapi.tiangolo.com/tutorial/sql-databases/).

### Add AI endpoints to FastAPI
Usually, we just move demo script to FastAPI.

```python
from fastapi import FastAPI, Request
from pydantic import BaseModel

# input/outputs for API
class InputData(BaseModel):
    input_image: str
    
class OutputData(BaseModel):
    output: list

# AI model
class DETR:
    def __init__(self):
        self.model = ... # load your model here
    
    def predict(self, input_data):
        return self.model.predict(input_data)

# FastAPI app
app = FastAPI()
detr = DETR()

@app.post("/detr")
def detr_predict(input_data: InputData):
    return OutputData(output=detr.predict(input_data.input_image))

```

### Mount Gradio to FastAPI
Gradio apps (interactive webUI) support to mount to FastAPI.

```python
from fastapi import FastAPI
import gradio as gr
app = FastAPI()
@app.get("/")
def read_main():
    return {"message": "This is your main app"}
io = gr.Interface(lambda x: "Hello, " + x + "!", "textbox", "textbox")
app = gr.mount_gradio_app(app, io, path="/gradio")
```

## Conclusion
In this blog, we have learned to build a complete web-based AI application (backend and frontend). We recommend to customize this template for your own project.

---
## Reference
1. [FastAPI](https://fastapi.tiangolo.com/)
2. [mount_gradio_app](https://www.gradio.app/docs/gradio/mount_gradio_app)