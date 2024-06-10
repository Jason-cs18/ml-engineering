---
title: Inference Server
layout: default
parent: Engineer
nav_order: 4
---

In the last [blog](https://jason-cs18.github.io/ml-engineering/inference_engine.html), we discuss how to achieve low-latency inference with inference engines. But in practice, we often need to handle concurrency and improve throughput of ML services. 

In this blog, we guide you how to deploy the pre-trained or optimized model on high-throughput inference server.

## Table of contents
- [Table of contents](#table-of-contents)
- [Use Ray Serve to deploy PyTorch model](#use-ray-serve-to-deploy-pytorch-model)
- [Use Ray Serve to deploy TorchScript/ONNX/TensorRT](#use-ray-serve-to-deploy-torchscriptonnxtensorrt)
- [Use Ray Serve and NVIDIA Triton Together](#use-ray-serve-and-nvidia-triton-together)
- [Conclusion](#conclusion)
- [References](#references)

## Use Ray Serve to deploy PyTorch model

## Use Ray Serve to deploy TorchScript/ONNX/TensorRT

## Use Ray Serve and NVIDIA Triton Together

## Conclusion
In this blog, we have learned ...

We recommend ...

---

## References
1. [Deploying HuggingFace models on Triton](https://github.com/triton-inference-server/tutorials/blob/main/HuggingFace/README.md#deploying-on-the-python-backend)
2. [Conceptual Guides for NVIDIA Triton](https://github.com/triton-inference-server/tutorials/tree/main/Conceptual_Guide)
3. [Ray Serve Documentation](https://docs.ray.io/en/latest/serve/index.html)
4. [Serving models with Triton Server in Ray Serve](https://docs.ray.io/en/latest/serve/tutorials/triton-server-integration.html)