---
title: DETR Train
layout: default
parent: Engineer
nav_order: 5
---
Although pre-trained models perform well on open benchmarks, they are hard to obtain decent performance on domain-specific tasks because of domain gaps. For example, an pre-trained DETR usually gets low accuracy on low-light images. To bridge this gap, we have to re-train or fine-tune it with our collected new data.

In this blog, we will guide you through the process of fine-tuning pre-trained DETR on [CPPE-5 dataset](https://huggingface.co/datasets/cppe-5) efficiently with Ray Train. Full code can be found [here]().

## Table of contents
- [Table of contents](#table-of-contents)
- [Why choose HuggingFace \& Ray Train?](#why-choose-huggingface--ray-train)
- [Train DETR with HuggingFace \& Ray Train (TBD)](#train-detr-with-huggingface--ray-train-tbd)
- [Conclusion](#conclusion)
- [Reference](#reference)

## Why choose HuggingFace & Ray Train?
HuggingFace offers many ready-to-use open-source models and also has a bunch of useful tools for training and evaluating models. So, we can train our models easily with HuggingFace.

Ray Train is a distributed training library that works with HuggingFace. Just like Ray Serve, it provides many high-level APIs and hides the complexity of distributed training and resource management.


## Train DETR with HuggingFace & Ray Train (TBD)

<!-- ## Train DETR with PyTorch-Lightning & Ray Train -->

## Conclusion
In this blog, we have learned how to train DETR on our own data. This need is very common in real-world applications. Thus, we recommend you try it out!

## Reference
1. [Train DETR with your own data](https://huggingface.co/docs/transformers/v4.28.0/tasks/object_detection), HuggingFace
2. [Get Started with Distributed Training using Hugging Face Transformers](https://docs.ray.io/en/latest/train/getting-started-transformers.html), Ray



