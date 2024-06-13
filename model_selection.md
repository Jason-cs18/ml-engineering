---
title: Model Selection
layout: default
parent: Engineer
nav_order: 1
---

When ML engineers start a new project, they often need to build a fast demo to present the key idea. Fortunately, many popular model zoos provide a convenient tool to complete this work.

> If you need to find a specific model zoo of vision (e.g., object detection, pose estimation, etc), I recommend you checkout the model zoo of [OpenMMLab](https://platform.openmmlab.com/modelzoo/), which consists of [MMDetection](https://github.com/open-mmlab/mmdetection), [MMSegmentation](https://github.com/open-mmlab/mmsegmentation), [MMagic](https://github.com/open-mmlab/mmagic), [MMDetection3D](https://github.com/open-mmlab/mmdetection3d), [MMOCR](https://github.com/open-mmlab/mmocr), [MMPose](https://github.com/open-mmlab/mmpose), [MMAction2](https://github.com/open-mmlab/mmaction2) popular libraries. Also, it provides a series deployment toolbox (e.g., [MMDeploy](https://github.com/open-mmlab/mmdeploy) and [MMRazor](https://github.com/open-mmlab/mmrazor)) to accelerate model inference speed. 

## Table of contents
- [Table of contents](#table-of-contents)
- [Use HuggingFace to build a demo](#use-huggingface-to-build-a-demo)
- [Use ModelScope to build a demo](#use-modelscope-to-build-a-demo)
- [Use MMDetection to build a demo](#use-mmdetection-to-build-a-demo)
- [Conclusion](#conclusion)
- [References](#references)

## Use HuggingFace to build a demo
HuggingFace is a popular model zoo for transformer-based models. On HuggingFace, you can find a lot of pre-trained models for various tasks. For example, you can try [DETR](https://huggingface.co/facebook/detr-resnet-50) within few lines of code.

```python
from transformers import AutoImageProcessor, DetrModel
from PIL import Image
import requests

url = "http://images.cocodataset.org/val2017/000000039769.jpg"
image = Image.open(requests.get(url, stream=True).raw)
image_processor = AutoImageProcessor.from_pretrained("facebook/detr-resnet-50")
model = DetrModel.from_pretrained("facebook/detr-resnet-50")

inputs = image_processor(images=image, return_tensors="pt")
outputs = model(**inputs)
```
Besides, it provides many tools to evaluate and deploy your models. Details can be found in [Datasets](https://huggingface.co/docs/datasets/index), [Evaluate](https://huggingface.co/docs/evaluate/index), and [Accelerate](https://huggingface.co/docs/accelerate).

> If you are in China, you can access HuggingFace models and datasets via [HF Mirror](https://hf-mirror.com/). 

## Use ModelScope to build a demo
[ModelScope](https://modelscope.cn/my/overview) is a alternative model zoo created by Alibaba. The download speed is faster than HuggingFace for Chinese users.

When you want to use a pre-trained model, you can search the model zoo and try the demo as instructions.

## Use MMDetection to build a demo
[MMDetection](https://github.com/open-mmlab/mmdetection) is a comprehensive model zoo for object detection and semantic segmentation. It contains more advanced detection and segmentation models than HuggingFace and ModelScope. However, its code is a bit complex and not easy to use as HuggingFace.

## Conclusion
In this page, we have learned how to build a fast demo for image object detection with three popular model zoos (HuggingFace, ModelScope, and MMDetection). In the [next blog](https://jason-cs18.github.io/ml-engineering/web_demo.html), we will write a interactive webUI for this demo.

## References
1. [HuggingFace](https://huggingface.co/)
2. [ModelScope](https://modelscope.cn/)
3. [OpenMMLab](https://platform.openmmlab.com/modelzoo/)