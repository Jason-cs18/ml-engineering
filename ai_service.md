---
title: Inference Microservice
layout: default
parent: Engineer
nav_order: 15
---
In previous blogs, we introduce the conponents of the AI product development pipeline. In this blog, we will teach how to put them it together and develop an end-to-end inference microservice. Unlike the experimental demos, the inference microservice is a production-ready service that can be deployed to the cloud and serve the real users.

## Inference Microservice

The inference microservice is a web service that can be deployed to the cloud. It is responsible for receiving the user's input, processing the input with the trained model, and returning the result to the user. The inference microservice is built using ONNX/TensorRT, FastAPI, and Docker-compose.

## Deployment

The deployment pipeline contains the following steps:
- implement inference pipeline (ONNX/TensorRT)
- containize the runtime (Docker)
- orchestrate the deployed services (Docker-compose/Kubernets)
- call the service (API)

### Implement the inference logic

### Write a Dockerfile

### Write a Docker-compose file

### Deploy the inference microservice

### API Call



