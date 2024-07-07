---
title: Inference Engine
layout: default
parent: Engineer
nav_order: 3
---
PyTorch provides a flexible and simple component for AI researchers and engineers to build and train deep learning models. However, its dynamic nature makes it challenging to optimize in production. Thus, a native PyTorch model is slow and inefficient.

To get a low-latency inference, we usually use inference engines, such as TorchScript, ONNX and TensorRT. They enable low-cost inference with graph optimization and optimized operaters.

In this blog, we will guide you accelerate [DETR-ResNet50](https://huggingface.co/facebook/detr-resnet-50) with ONNX and TensorRT engines. In the end, we will introduce a little bit about TVM. 


```
- Model: DETR-ResNet50
- Experiment environments
  - OS: Ubuntu 18.04
  - GPU: NVIDIA Tesla V100
```

## Table of contents
- [Table of contents](#table-of-contents)
- [Use PyTorch](#use-pytorch)
- [Use ONNX](#use-onnx)
  - [Export model to onnx](#export-model-to-onnx)
  - [Inference with ONNX](#inference-with-onnx)
- [Use TensorRT](#use-tensorrt)
- [Use TVM (TBD)](#use-tvm-tbd)
- [Conclusion](#conclusion)
- [References](#references)

## Use PyTorch
As presented in the [previous blog](https://jason-cs18.github.io/ml-engineering/model_selection.html), we can easily load and infer with transformers library. If you do not familiar with huggingface, you can refer to the [official tutorial](https://huggingface.co/docs/transformers/model_doc/detr).

After measurement, the inference cost on 1 NVIDIA Tesla V100 is
```markdown
# batch-size = 1
- Latency (ms): 79.2
- GPU memory (MB): 187.7
```

## Use ONNX
### Export model to onnx
Prior to exporting, make sure you have download models from [Huggingface](https://huggingface.co/facebook/detr-resnet-50). Then, you can use the built-in tools to export the model to ONNX format.

```bash
optimum-cli export onnx --model /mnt/code/model_zoo/detr-resnet-50 /mnt/code/model_zoo/detr-resnet-50-onnx --task object-detection --device cuda
```
### Inference with ONNX
After exporting, we can use the ONNX runtime to load model and infer. The latency is
```markdown
# batch-size = 1
- Latency (ms): 30.84
```
```python
from time import time

import torch
import onnxruntime
from PIL import Image
from transformers import AutoImageProcessor, DetrForObjectDetection
from rich.console import Console

console = Console()

img_path = "../test_data/000000039769.jpg"
model_path = "/mnt/code/model_zoo/detr-resnet-50-onnx"
model_onnx_path = "/mnt/code/model_zoo/detr-resnet-50-onnx/model.onnx"
device = "cuda" if torch.cuda.is_available() else "cpu"

image = Image.open(img_path)

console.log("Load pre-trained DETR (ONNX)")
image_processor = AutoImageProcessor.from_pretrained(model_path)

session_options = onnxruntime.SessionOptions()
providers = ["CPUExecutionProvider"]
if device == 'cuda':
    providers = ["CUDAExecutionProvider", "CPUExecutionProvider"]
    
session = onnxruntime.InferenceSession(model_onnx_path, sess_options=session_options, providers=providers)	

console.log("preprocessing")
inputs = image_processor(images=image, return_tensors="pt")
# print(inputs.data)

console.log("inference")
warmup_times = 2
for i in range(warmup_times):
    pred = session.run(None, {'pixel_values': inputs.data['pixel_values'].numpy()})

measure_times = 5
start = time()
for i in range(measure_times):
    pred = session.run(None, {'pixel_values': inputs.data['pixel_values'].numpy()})
    
end = time()
console.log(f"Latency: {round((end - start) / measure_times * 1000, 2)} ms")
```

## Use TensorRT
<!-- Converting the ONNX model to TensorRT model is a little bit more complicated. Thus, we use the offical example of [mmdetection-to-tensorrt](https://github.com/grimoire/mmdetection-to-tensorrt). -->



## Use TVM (TBD)

## Conclusion
In this blog, we have learned to accelerate PyTorch models with different inference engines. Unlike PyTorch, ONNX and TensorRT are static graphs and can optimize the execution graph with performant operators and graph optimization. TVM is a flexible and efficient compiler framework. It can be used to optimize the execution graph with custom operators and graph optimization. But it also needs a lot of work to implement the operators and graph optimization.

Thus, we recommend to use ONNX or TensorRT for inference acceleration on NVIDIA GPUs.

---

## References
1. [torchscript-to-tvm](https://github.com/masahi/torchscript-to-tvm/blob/master/detr/detr_test.py)
2. [OpenMMlab导出DETR模型并用onnxruntime推理](https://blog.csdn.net/taifyang/article/details/136127159)
3. [OpenMMlab导出yolox模型并用onnxruntime和tensorrt推理](https://blog.csdn.net/taifyang/article/details/134368390)
