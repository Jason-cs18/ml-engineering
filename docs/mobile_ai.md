---
title: Mobile AI
layout: default
parent: Engineer
nav_order: 14
---
In this tutoral, we will guide you deploy a vision transformer model on Huawei Mate 30 via [MNN](https://www.mnn.zone/m/0.3/). MNN is developed by Alibaba and it supports on-device training/inference.

![](https://github.com/alibaba/MNN/raw/master/doc/architecture.png)

![alt text](image-11.png)

_image source: [MNN Github](https://github.com/alibaba/MNN)_

## Table of Contents
- [Table of Contents](#table-of-contents)
- [Deployment Pipeline](#deployment-pipeline)
- [Installation](#installation)
- [Model Preparation](#model-preparation)
- [Deployment](#deployment)
- [Benchmark Results](#benchmark-results)
- [References](#references)

## Deployment Pipeline
![alt text](image-12.png)

_image source: [MNN Github](https://github.com/alibaba/MNN)_

Deploy a pre-trained PyTorch model to mobile employs the following steps:

1. Convert PyTorch model to ONNX format
2. Convert ONNX model to MNN format
3. Benchmark MNN model on mobile devices

## Installation
1. Download [Android NDK](https://developer.android.com/ndk/downloads) and set environment variable via `export ANDROID_NDK=/path/to/ndk`.
2. Install [adb](https://www.xda-developers.com/install-adb-windows-macos-linux/) (android testing tool)
3. Build MNN Android library
   ```bash
   git clone https://github.com/alibaba/MNN.git
   cd MNN/project/android
   # modify MNN/CMakeLists.txt
   # set MNN_BUILD_CONVERTER to ON
   # set MNN_OPENCL to ON (mobile GPU)
   # set MNN_NNAPI to ON (mobile NPU)
   mkdir build_64 && cd build_64 && ../build_64.sh
   ```

## Model Preparation
```python
import onnx
import torch
import numpy as np
import torchvision.models as models
model = models.vit_b_16(pretrained=True)
model.eval()
batch_size = 1
x = torch.randn(batch_size, 3, 224, 224)

torch.onnx.export(model, x, "vit.onnx", input_names=['input'], output_names=['output'], verbose=False)
```

## Deployment

1. Convert ONNX model to MNN format
   ```bash
   cd MNN/project/android/build_64
   ./MNNConvert -f ONNX --modelFile /path/to/vit.onnx --MNNModel vit.mnn --bizCode biz
   ```
2. Upload MNN model and MNN library to connected mobile device
   ```bash
   # make sure adb is installed and mobile is connected
   cd MNN/project/android/build_64
   # upload MNN binary
   ../updateTest.sh
   # upload MNN model 
   adb push /path/to/vit.mnn /data/local/tmp/MNN/vit.mnn 
   adb shell
   cd /data/local/tmp/MNN && export LD_LIBRARY_PATH=.:$LD_LIBRARY_PATH
   ```
3. Run benchmark on the connected mobile device
   ```bash
   # using mobile CPU
   ./MNNV2Basic.out vit.mnn 10 0 0 4 1x3x224x224
   # using mobile GPU
   ./MNNV2Basic.out vit.mnn 10 0 3 4 1x3x224x224
   ```

## Benchmark Results

Vision Transformer 

|Model|#Param (M)|#GFLOPs|Model Size (MB)|
|:---:|:---:|:---:|:---:|
|vit_b_16|86.56|17.58|346.50|

Profiling results on Huawei Mate 30

|Device|CPU (ms)|GPU (ms)|
|:---:|:---:|:---:|
|Laptop (i7+1070)|236.41|5.46|
|Huawei Meta 30|530.25|339.51|


## References
1. [MNN Documentation](https://mnn-docs.readthedocs.io/en/latest/)
2. [PyTorch Profiler](https://github.com/Jason-cs18/deep-learning-profiler)