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
	- [Packaging multiple files (TBD)](#packaging-multiple-files-tbd)
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
	@echo "make run: run news_scraper.py in background mode"

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
	($(CONDA_ACTIVATE) /mnt/data/envs/avatar_demo; nohup python -W ignore news_scraper.py > output.log &)
```

After creating the Makefile, we can run the commands by typing `make <command>` in the terminal. For instance, to run news_scraper.py in the background, we can type `make run`.

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
from rich.console import Console

console = Console()

def main():
    console.log("Hello World!")


if __name__ == '__main__':
    main()
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
Now, we can run `hello_world` app on any Linux and macOS devices.

```bash
./hello_world

# output
[16:59:39] Hello World!                                                  hello_world.py:12
```

### Packaging multiple files (TBD)

## Conclusion
In this note, we have learned how to use Makefile to automate the execution of Python scripts and how to package a Python project using `pyinstaller`. These tools are essential for managing and deploying Python projects efficiently.

## Continue reading
- [Makefiles in python projects](https://krzysztofzuraw.com/blog/2016/makefiles-in-python-projects/)
- [PyInstaller Manual](https://pyinstaller.org/en/stable/)	