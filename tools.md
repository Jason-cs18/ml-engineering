---
title: Automation
layout: default
parent: Engineer
nav_order: 14
---

In Python projects, a considerable number of executable files exist that we are required to run, such as training, testing, and webui. To circumvent the need for manual execution of these files, we employ Makefile to mechanize the procedure of running them. During production delivery, it is essential to package the entire project, encompassing both the scripts and the dependencies, into a solitary executable file. This enables users to run the packaged application without having to install a Python interpreter or any associated modules.

## Table of contents
- [Table of contents](#table-of-contents)
- [Makefile](#makefile)
- [Package](#package)
  - [Installation](#installation)
  - [Packaging a single file](#packaging-a-single-file)
  - [Packaging multiple files](#packaging-multiple-files)
- [Conclusion](#conclusion)
- [Continue reading](#continue-reading)

## Makefile
Makefile is a file that encompasses a collection of instructions intended for the make program. It serves the purpose of automating the procedure related to compiling and linking programs. In the aforementioned example, we leverage Makefile to mechanize the processes of installing dependencies, inspecting logs, and running news_scraper.py.

```makefile
# Makefile
SHELL=/bin/bash

CONDA_ACTIVATE=source $$(conda info --base)/etc/profile.d/conda.sh ; conda activate ; conda activate

.PHONY: help clean delete check run

help:
	@echo "make help: print all available commands"
	@echo "make activate: activate pre-installed conda env"
	@echo "make setup: install all required packages"
	@echo "make clean: clean all generated audios (*.mp3)"
	@echo "make delete: detele existing_urls.npy"
	@echo "make find: find pid of the running process with output.log"
	@echo "make check: print the log of the last run"
	@echo "make run: run news_scraper.py in background mode (params: web_url)"

activate:
	($(CONDA_ACTIVATE) /mnt/data/envs/avatar_demo ; python -c "import numpy as np; print(np.__version__)")

setup: requirements.txt
	pip install -r requirements.txt

clean:
	rm ./web_ui/*.mp3

delete:
	rm existing_urls.npy

find:
	lsof output.log

check:
	cat output.log

run:
	($(CONDA_ACTIVATE) /mnt/data/envs/avatar_demo; nohup python -W ignore news_scraper.py --web_url ${web_url} > output.log &)
```

After creating the Makefile, we can run the commands by typing `make <command>` in the terminal. For instance, to run news_scraper.py in the background, we can type `make run web_url="www.baidu.com"`.

## Package
In Python, a common way to package a project is to use the `pyinstaller` tool. It is a standalone package that can be used to package a Python project into a single executable file.

### Installation
```bash
pip install -U pyinstaller
```

### Packaging a single file
Let us package a single Python script (`hello_world.py`).
```python
'''
@Project: Package Demo
@File: hello_world.py
@Author: Yan Lu
@Date: 2024/07/30
'''
import uvicorn
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def root():
    return {"hello": "world"}

if __name__ == '__main__':
    uvicorn.run(app, host="127.0.0.1", port=58000, reload=False)
```
After writing the script, we can package it using the following command:
```bash
pyinstaller hello_world.py
```
This will generate a `dist` folder containing the executable file (`hello_world.exe` on Windows, `hello_world` on Linux and macOS).

```bash
dist/
└── hello_world
    ├── hello_world
    └── _internal
```
Now, we can simply run `hello_world` app on any Linux and macOS devices.

```bash
./dist/hello_world/hello_world

# output
INFO:     Started server process [3669391]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     Uvicorn running on http://127.0.0.1:58000 (Press CTRL+C to quit)
INFO:     127.0.0.1:58656 - "GET / HTTP/1.1" 200 OK
```

### Packaging multiple files
In Python projects, we often need to package multiple Python files into a single executable file. Let us take a simple image classification project as an example. We have a `main.py` file that imports functions from `model.py` and starts a FastAPI server.

```bash
# File structure
.
├── config.yaml
├── image.jpg
├── main.py
├── model.py
└── resnet50 # model weights
```

```yaml
# config.yaml
host: 127.0.0.1
port: 5000
model_name: resnet50
task: image classification
model_path: /home/jason/Code/package/huggingface/resnet50
```

```python
'''
@Project: Package Demo
@File: model.py
@Author: Yan Lu
@Date: 2024/07/30
'''
import torch
from transformers import AutoImageProcessor, ResNetForImageClassification

class Classifier:
    """Classifier model"""
    def __init__(self, model_path, device="cuda:0"):
        self.device = device
        self.processor = AutoImageProcessor.from_pretrained(model_path)
        self.model = ResNetForImageClassification.from_pretrained(model_path)
        self.model = self.model.to(device)
        
    def predict(self, x):
        inputs = self.processor(x, return_tensors="pt")
        inputs = inputs.to(self.device)
        with torch.no_grad():
            logits = self.model(**inputs).logits
        
        del inputs
        torch.cuda.empty_cache()
        predicted_label = logits.argmax(-1).item()
        
        return self.model.config.id2label[predicted_label]
```

```python
'''
@Project: Package Demo
@File: main.py
@Author: Yan Lu
@Date: 2024/07/30
'''
import sys

sys.setrecursionlimit(100000) # important param for pyinstaller

import uvicorn
from PIL import Image 
from omegaconf import OmegaConf
from fastapi import FastAPI, File, UploadFile

from model import Classifier

app = FastAPI()

config = OmegaConf.load("config.yaml")

model = Classifier(config.model_path)

@app.get("/")
def root():
    return "欢迎使用图片分类服务！"


@app.post("/predict")
async def predict(file: UploadFile = File(...)):
    filename = "test.jpg"
    try:
        res = await file.read()
        with open(filename, "wb") as f:
            f.write(res)
    except Exception as e:
        print("invalid file!")
    
    image = Image.open(filename)
    result = model.predict(image)
    return result


if __name__ == '__main__':
    uvicorn.run(app, host=config.host, port=config.port, reload=False)
```

Use the following command to package the project:
```bash
pyinstaller main.py

# if you meet "A RecursionError (maximum recursion depth exceeded) occurred", please follow the instructions below:
# 1. add this line near the top:: to your program's spec file
# import sys ; sys.setrecursionlimit(sys.getrecursionlimit() * 5)
# 2. build your program by running PyInstaller with .spec file 
# pyinstaller myprog.spec
```
Like the previous example, we can simply run the application via
```bash
./dist/hello_world/hello_world

# outputs
INFO:     Started server process [3951422]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     Uvicorn running on http://127.0.0.1:5000 (Press CTRL+C to quit)
INFO:     127.0.0.1:33254 - "GET / HTTP/1.1" 200 OK
INFO:     127.0.0.1:33262 - "GET /docs HTTP/1.1" 200 OK
INFO:     127.0.0.1:33262 - "GET /openapi.json HTTP/1.1" 200 OK
INFO:     127.0.0.1:42574 - "POST /predict HTTP/1.1" 200 OK
```

## Conclusion
In this note, we have learned how to use Makefile to automate the execution of Python scripts and how to package a Python project using `pyinstaller`. These tools are essential for managing and deploying Python projects efficiently.

## Continue reading
- [Makefiles in python projects](https://krzysztofzuraw.com/blog/2016/makefiles-in-python-projects/)
- [PyInstaller Manual](https://pyinstaller.org/en/stable/)	