---
title: Docker
layout: default
parent: Engineer
nav_order: 9
---
_open sentence_

## Build a docker image for your AI services

## Run a docker container with local database

## Use docker-compose to run/manage multiple AI services

## Conclusion
In this blog, we have learned to containize our AI services with docker and docker-compose. In practice, many applications/serives has their own dependencies (e.g., python-3.10 or tensorflow-2.1.0). Thus, using docker container to isolate these environments is recommend. If your application contains many AI services, using docker-compose is a good choice to run/manage/stop them.