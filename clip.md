---
title: CLIP and its variants
date: 2023-06-09
layout: default
parent: DL Basics
nav_order: 2
---

As mentioned in [last blog](https://jason-cs18.github.io/ml-engineering/transformers.html), scaling model size and data size is a key to improve performance of transformer-based models. However, collecting large-scale data is not always an easy task. In this blog, I will introduce some multimodal learning methods, which expand the training data to other modals.

## Table of contents
- [Table of contents](#table-of-contents)
- [Multimodal Learning](#multimodal-learning)
- [CLIP](#clip)
- [SAM](#sam)
- [BLIP and BLIP-2](#blip-and-blip-2)
- [ImageBind](#imagebind)
- [LanguageBind](#languagebind)
- [LLaVA](#llava)
- [Conclusion](#conclusion)
- [References](#references)

## Multimodal Learning
![](https://linzhiqiu.github.io/papers/cross_modal/images/motivation_new.png)

_image source: [Multimodality Helps Unimodality](https://linzhiqiu.github.io/papers/cross_modal/)_

Owing to dataset bias, a majority of unimodal models find it difficult to generalize effectively on new tasks or domains. Take, for instance, image classification models that are trained on ImageNet and struggle to classify blurred images. However, if we obtain the audio corresponding to the blurred image, we can potentially predict the class of the blurred image with greater ease (such as a car). As depicted in the figure presented above, we are able to utilize additional text or audio information to enhance the discriminatory capacity. 

![](https://linzhiqiu.github.io/papers/cross_modal/images/methodology.png)

_image source: [Multimodality Helps Unimodality](https://linzhiqiu.github.io/papers/cross_modal/)_

To employ multimodal information, contrastive learning and transformer architecture are always a good choice. With contrasive learning, we can learn a representation that is invariant to the modality and align multiple modals. Transformers are able to compress multimodal information into a same semantic representation.

## CLIP

## SAM

## BLIP and BLIP-2

## ImageBind

## LanguageBind

## LLaVA

## Conclusion

----
## References
1. [ICML'21] [Learning Transferable Visual Models From Natural Language Supervision](http://proceedings.mlr.press/v139/radford21a)
2. [ICCV'23] [Segment Anything](https://arxiv.org/abs/2304.02643)
3. [ICML'22] [BLIP: Bootstrapping Language-Image Pre-training for Unified Vision-Language Understanding and Generation](https://arxiv.org/abs/2201.12086)
4. [ICML'23] [BLIP-2: Bootstrapping Language-Image Pre-training with Frozen Image Encoders and Large Language Models](https://arxiv.org/abs/2301.12597)
5. [CVPR'23] [ImageBind: One Embedding Space To Bind Them All](https://arxiv.org/abs/2305.05665)
6. [ICLR'24] [LanguageBind: Extending Video-Language Pretraining to N-modality by Language-based Semantic Alignment](https://github.com/PKU-YuanGroup/LanguageBind)
7. [NeurIPS'23] [Visual Instruction Tuning](https://llava-vl.github.io/)
8. [CVPR'24] [Improved Baselines with Visual Instruction Tuning](https://arxiv.org/abs/2310.03744)
9. [CVPR'23] [Multimodality Helps Unimodality:
Cross-Modal Few-Shot Learning with Multimodal Models](https://linzhiqiu.github.io/papers/cross_modal/) 