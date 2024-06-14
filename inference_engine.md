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
  - GPU: NVIDIA Tesla V100
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
import time
import argparse

from rich.console import Console
from omegaconf import OmegaConf

from transformers import DetrImageProcessor, DetrForObjectDetection
import torch
from PIL import Image

console = Console()

def parse_args():
    parser = argparse.ArgumentParser(description="PyTorch Inference Script")
    parser.add_argument("--config", type=str, default="/mnt/data/production_template/DLTK/dlapp_template/configs/infer_default.yaml", help="Path to config file")
    parser.add_argument("--mode", type=str, default="image", help="test image or video")
    parser.add_argument("--batch_size", type=int, default=1, help="batch size for inference")
    return parser.parse_args()


def main():
    args = parse_args()
    console.log(f"Profiling PyTorch-DETR with batchsize {args.batch_size}")
    config = OmegaConf.load(args.config)
    # console.log(f"model: {config.model}")
    # console.log(f"data: {config.data}")
    
    device = "cuda:0" if torch.cuda.is_available() else "cpu"
    
    if args.mode == "image":
        
        console.log("Inference on image")
        image = Image.open(config.data.image_path)
        images = []
        for i in range(args.batch_size):
            images.append(image)
        
        start = time.time()
        processor = DetrImageProcessor.from_pretrained(config.model.model_path, revision="no_timm")
        model = DetrForObjectDetection.from_pretrained(config.model.model_path, revision="no_timm").to(device)
        end = time.time()
        load_time = end - start
        
        start = time.time()
        inputs = processor(images=images, return_tensors="pt").to(device)
        end = time.time()
        preprocess_time = end -start
        
        # warmup
        with torch.no_grad():
            for i in range(3):
                outputs = model(**inputs)
        
        start = time.time()
        with torch.no_grad():
            for i in range(5):
                outputs = model(**inputs)
        
        end = time.time()
        infer_time = (end - start)/(5*args.batch_size)
        
        console.log(f"Load time: {load_time}s")
        console.log(f"Preprocess time: {preprocess_time}s")
        console.log(f"Inference time: {infer_time}s")
        console.log(f"Throughput: {1/infer_time} images/s")

```

|Batch Size|1|4|8|
|:---:|:---:|:---:|:---:|
|Throughput (fps)|9.33|19.69|23.70|


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
