---
title: Inference Engine
layout: default
parent: Engineer
nav_order: 3
---
PyTorch provides a flexible and simple component for AI researchers and engineers to build and train their models. However, its dynamic nature makes it challenging to deploy in production. Specifically, PyTorch models often need a large memory footprint and hava a slow inference speed.

To push its limit on inference, we usually convert them to a static graph and using inference engines to optimize the graph, such as ONNX, TensorRT, and TensorFlow Lite.

```
- Model: DETR-ResNet50
- Experiment environments
  - OS: Ubuntu 18.04
  - GPU: Tesla V100 32G
```

## Table of contents
- [Table of contents](#table-of-contents)
- [Use TorchScript](#use-torchscript)
- [Use ONNX](#use-onnx)
- [Use TensorRT](#use-tensorrt)
- [Use TVM](#use-tvm)
- [Conclusion](#conclusion)
- [References](#references)

## Use TorchScript

## Use ONNX

## Use TensorRT

## Use TVM

## Conclusion
In this blog, we have learned to accelerate PyTorch models with different inference engines. Unlike PyTorch, ONNX and TensorRT are static graphs and can optimize the execution graph with performant operators and graph optimization. TVM is a flexible and efficient compiler framework. It can be used to optimize the execution graph with custom operators and graph optimization. But it also needs a lot of work to implement the operators and graph optimization.

Thus, we recommend to use ONNX or TensorRT for inference acceleration on NVIDIA GPUs.

---

## References
1. [torchscript-to-tvm](https://github.com/masahi/torchscript-to-tvm/blob/master/detr/detr_test.py)
2. [OpenMMlab导出DETR模型并用onnxruntime推理](https://blog.csdn.net/taifyang/article/details/136127159)
3. [OpenMMlab导出yolox模型并用onnxruntime和tensorrt推理](https://blog.csdn.net/taifyang/article/details/134368390)
