---
title: Automation
layout: default
parent: Engineer
nav_order: 14
---

In our daily works, we have to write and run many Python scripts. To make our lives easier, we use some tools to automate the process.

## Makefile
Makefile is a file that contains a set of instructions for the make program. It is used to automate the process of compiling and linking programs. In our project, we use Makefile to automate the process of building the project and running the tests.

```makefile
SHELL=/bin/bash

CONDA_ACTIVATE=source $$(conda info --base)/etc/profile.d/conda.sh ; conda activate ; conda activate

.PHONY: help clean delete check run

help:
	@echo "make help: print all available commands"
	@echo "make activate: activate pre-installed conda env"
	@echo "make setup: install all required packages"
	@echo "make clean: clean all generated audios (*.mp3)"
	@echo "make delete: detele existing_urls.npy"
	@echo "make check: find pid of the running process with output.log"
	@echo "make run: run news_scraper.py in background mode"

activate:
	($(CONDA_ACTIVATE) /mnt/data/envs/avatar_demo ; python -c "import numpy as np; print(np.__version__)")

setup: requirements.txt
	pip install -r requirements.txt

clean:
	rm ./web_ui/*.mp3

delete:
	rm existing_urls.npy

check:
	lsof output.log

run:
	($(CONDA_ACTIVATE) /mnt/data/envs/avatar_demo; nohup python -W ignore news_scraper.py > output.log &)
```