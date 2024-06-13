---
title: Inference Engine
layout: default
parent: Engineer
nav_order: 3
---
PyTorch provides a flexible and simple component for AI researchers and engineers to build and train deep learning models. However, its dynamic nature makes it challenging to optimize in production. Thus, a native PyTorch model is slow and inefficient.

To get a low-latency inference, we usually use inference engines, such as TorchScript, ONNX and TensorRT. They enable low-cost inference with graph optimization and optimized operaters.

In this blog, we will guide you accelerate [DETR-ResNet50](https://huggingface.co/facebook/detr-resnet-50) with TorchScript, ONNX and TensorRT engines. In the end, we will introduce a little bit about TVM. 


```
- Model: DETR-ResNet50
- Experiment environments
  - OS: Ubuntu 18.04
  - GPU: GeForce GTX 1070
```

## Table of contents
- [Table of contents](#table-of-contents)
- [Use TorchScript](#use-torchscript)
  - [1. Prepare PyTorch model](#1-prepare-pytorch-model)
  - [2. Convert PyTorch model to TorchScript](#2-convert-pytorch-model-to-torchscript)
  - [3. Test TorchScript model](#3-test-torchscript-model)
- [Use ONNX](#use-onnx)
  - [1. Prepare PyTorch model (skip)](#1-prepare-pytorch-model-skip)
  - [2. Convert PyTorch model to TorchScript](#2-convert-pytorch-model-to-torchscript-1)
  - [3. Test TorchScript model](#3-test-torchscript-model-1)
- [Use TensorRT](#use-tensorrt)
- [Use TVM](#use-tvm)
- [Conclusion](#conclusion)
- [References](#references)

## Use TorchScript

### 1. Prepare PyTorch model
```python
from transformers import DetrImageProcessor, DetrForObjectDetection
import torch
import time
from loguru import logger
from PIL import Image

img_url = "demo.jpg" # replace with your image url
image = Image.open(img_url)
batch_size = 1

logger.debug(f"start profile detr with batch_size={batch_size}")

# you can specify the revision tag if you don't want the timm dependency
processor = DetrImageProcessor.from_pretrained("detr", revision="no_timm")
model = DetrForObjectDetection.from_pretrained("detr", revision="no_timm").to("cuda")
gpu_memory_model = torch.cuda.memory_allocated("cuda")
logger.debug(f"load model done! model size: {gpu_memory_model/1024/1024:.2f}MB")

# preprocess
image_list = []
for i in range(batch_size):
    image_list.append(image)
    
inputs = processor(images=image_list, return_tensors="pt").to("cuda")
gpu_memory_inputs = torch.cuda.memory_allocated("cuda") - gpu_memory_model
logger.debug(f"preprocess done! data size: {gpu_memory_inputs/1024/1024:.2f}MB")

# inference
warm_up = 3
for i in range(warm_up):
    with torch.no_grad():
        outputs = model(**inputs)

start = time.time()
for i in range(5):
    with torch.no_grad():
        outputs = model(**inputs)

end = time.time()
logger.debug(f"inference done! time cost: {(end-start)/5:.2f}s")
```

> model size: `162.32` MB

|Batch Size|1|4|8|16|
|:---:|:---:|:---:|:---:|:---:|
|Throughput (fps)|11.22|12.02|11.87|12.08|


### 2. Convert PyTorch model to TorchScript

### 3. Test TorchScript model

## Use ONNX

### 1. Prepare PyTorch model (skip)

### 2. Convert PyTorch model to TorchScript

### 3. Test TorchScript model

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
