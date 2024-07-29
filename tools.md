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
In Python, a popular way to package a project is to use the `pyinstaller` tool. It is a standalone package that can be used to package a Python project into a single executable file.
- [PyInstaller Manual](https://pyinstaller.org/en/stable/)