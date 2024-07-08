---
title: Inference Server
layout: default
parent: Engineer
nav_order: 4
---

In the previous [blog](https://jason-cs18.github.io/ml-engineering/inference_engine.html), we discussed how to achieve low-latency inference using inference engines. However, in practical scenarios, we often need to handle concurrency and enhance the throughput of ML services.
In this blog, we will guide you on how to deploy the pre-trained or optimized model on a high-throughput inference server. Full code can be found in [here](https://github.com/Jason-cs18/DLTK). 

## Table of contents
- [Table of contents](#table-of-contents)
- [Why we choose Ray Serve?](#why-we-choose-ray-serve)
- [Deploy DETR on Ray Serve](#deploy-detr-on-ray-serve)
- [Conclusion](#conclusion)
- [References](#references)

<!-- ## Use Ray Serve to deploy PyTorch model -->

## Why we choose Ray Serve?
Before [Ray Serve](https://docs.ray.io/en/latest/serve/index.html), there were two common approaches for serving ML models: wrapping models within a web server and building a microservice. Nevertheless, both of these methods require significant human effort to balance the load and scale the ML services. To enable easy and automatic scaling, Ray Serve was developed. Moreover, it is compatible with numerous popular web development frameworks, such as Flask and FastAPI.

![](https://images.ctfassets.net/xjan103pcp94/2h69wovKC65iUOj3CSs7cu/8462545f5593c27d61c7151fd069a56b/17PipelineImplementation.png)

_image source: [Serving ML Models in Production: Common Patterns](https://www.anyscale.com/blog/serving-ml-models-in-production-common-patterns)_

Besides Ray Serve, there are other popular inference servers, such as 
[NVIDIA Triton](https://github.com/triton-inference-server/triton-inference-server). However, we choose Ray Serve because it is easy to use and integrate with other frameworks.

## Deploy DETR on Ray Serve
In this section, we deploy the DETR model from Huggingface on Ray Serve. The implement logic is simple and can be easily extended to other models.

```bash
# run Ray server
serve build ray_backend:detector_app -o config.yaml 
serve run config.yaml
# try the deployed model
curl -X POST -F "file=@/mnt/code/data/000000000009.jpg" http://localhost:8000/predict
# or view online docs http://localhost:8000/docs
```

```python
# ray_backend.py
import io
import warnings
warnings.filterwarnings("ignore")

import ray
import requests
from fastapi import FastAPI, File, UploadFile
from ray import serve

import torch
from PIL import Image
from transformers import pipeline

from rich.console import Console

console = Console()

model_path = "/mnt/code/model_zoo/detr-resnet-50/"
device = "cuda" if torch.cuda.is_available() else "cpu"

app = FastAPI()

@serve.deployment(num_replicas=2, ray_actor_options={"num_cpus": 0.2, "num_gpus": 1})
@serve.ingress(app)
class Detector:
    def __init__(self) -> None:
        self.pipe = pipeline("object-detection", model=model_path, device=0)
    
    @app.post("/predict")
    async def detect(self, file: UploadFile = File(...)) -> list:
        request_object_content = await file.read()
        img = Image.open(io.BytesIO(request_object_content))
        with torch.no_grad():
            outputs = self.pipe(img)
        
        return outputs
    
    
detector_app = Detector.bind()
```

## Conclusion
In this blog, we have explored how to serve pre-trained models on Ray Serve. We have only presented a simple example of serving an object detection model. However, it should be noted that Ray Serve natively supports model composition and other complex use cases. If you are interested in learning more about these features, please refer to [the Ray Serve Documentation](https://docs.ray.io/en/latest/serve/index.html) for detailed information.

---

## References
1. [Deploying HuggingFace models on Triton](https://github.com/triton-inference-server/tutorials/blob/main/HuggingFace/README.md#deploying-on-the-python-backend)
2. [Conceptual Guides for NVIDIA Triton](https://github.com/triton-inference-server/tutorials/tree/main/Conceptual_Guide)
3. [Ray Serve Documentation](https://docs.ray.io/en/latest/serve/index.html)
4. [Serving models with Triton Server in Ray Serve](https://docs.ray.io/en/latest/serve/tutorials/triton-server-integration.html)