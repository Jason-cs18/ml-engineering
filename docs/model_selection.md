---
title: Model Selection
layout: default
parent: Engineer
nav_order: 1
---

When ML engineers start a new project, they often need to build a fast demo to present the key idea. Fortunately, many popular model zoos provide a convenient tool to complete this work.

> If you need to find a specific model zoo of vision (e.g., object detection, pose estimation, etc), I recommend you checkout the model zoo of [OpenMMLab](https://platform.openmmlab.com/modelzoo/), which consists of [MMDetection](https://github.com/open-mmlab/mmdetection), [MMSegmentation](https://github.com/open-mmlab/mmsegmentation), [MMagic](https://github.com/open-mmlab/mmagic), [MMDetection3D](https://github.com/open-mmlab/mmdetection3d), [MMOCR](https://github.com/open-mmlab/mmocr), [MMPose](https://github.com/open-mmlab/mmpose), [MMAction2](https://github.com/open-mmlab/mmaction2) popular libraries. Also, it provides a series deployment toolbox (e.g., [MMDeploy](https://github.com/open-mmlab/mmdeploy) and [MMRazor](https://github.com/open-mmlab/mmrazor)) to accelerate model inference speed. 

In this blog, we use DETR as an example to show how to use HuggingFace to build a demo. DETR is a classic object detection model built on transformer. Unlike other CNN-based models, DETR unifies the convolutional and transformer-based models into a single framework. It removes some hand-crafted components (e.g., RPN, anchor, etc) and achieves state-of-the-art performance. If you do not familiar with DETR, I recommend you to read the HuggingFace [tutorial](https://huggingface.co/docs/transformers/model_doc/detr). All code samples are available in [DLTK](https://github.com/Jason-cs18/DLTK/tree/main/mlsys_template).

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
from transformers import AutoImageProcessor, DetrForObjectDetection
import torch
from PIL import Image
from rich.console import Console

console = Console()

img_path = "../test_data/000000039769.jpg"
model_path = "/mnt/code/model_zoo/detr-resnet-50"
device = "cuda" if torch.cuda.is_available() else "cpu"

image = Image.open(img_path)

console.log("Load pre-trained DETR")
image_processor = AutoImageProcessor.from_pretrained(model_path)
model = DetrForObjectDetection.from_pretrained(model_path).to(device)

console.log("preprocessing")
inputs = image_processor(images=image, return_tensors="pt").to(device)
console.log("Inference")
outputs = model(**inputs)

target_sizes = torch.tensor([image.size[::-1]])
console.log("postprocessing")
results = image_processor.post_process_object_detection(outputs, threshold=0.9, target_sizes=target_sizes)[0]

for score, label, box in zip(results["scores"], results["labels"], results["boxes"]):
    box = [round(i, 2) for i in box.tolist()]
    print(
        f"Detected {model.config.id2label[label.item()]} with confidence "
        f"{round(score.item(), 3)} at location {box}"
    )
```

I also provide a measurement script in [DLTK](https://github.com/Jason-cs18/DLTK/tree/main/mlsys_template). You can try it locally.

```bash
# Measurement
model size: 41524768
Latency: 79.2 ms
GPU used 187.67 MB memory
```

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