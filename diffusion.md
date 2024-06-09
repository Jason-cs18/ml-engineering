---
title: Diffusion
layout: default
parent: DL Basics
nav_order: 3
---

Unlike text, image and video data are often high-dimension and contain the inner spatial relationship. Thus, applying LLMs to image/video data is not straightforward.

In this blog, we will introduce the diffusion model and its variants, which have been widely used in image/video generation.

## Image Synthesis


## Video Synthesis


## Text to 3D
Although 2D synthesis has gained significant progress, the view consistency still remains a challenge. To bridge this gap, some works have proposed to generate 3D objects from text descriptions directly.

Unlike 2D synthesis, 3D synthesis is a challenging task due to

- Limited high quality 3D training data
- Hard to align different modal in 3D space



## Conclusion

---

## References
1. [Blog] [What are Diffusion Models?](https://lilianweng.github.io/posts/2021-07-11-diffusion-models/), Lilian Weng
2. [Blog] [Diffusion Models for Video Generation](https://lilianweng.github.io/posts/2024-04-12-diffusion-video/), Lilian Weng
3. [ICML'21] [Zero-Shot Text-to-Image Generation](https://proceedings.mlr.press/v139/ramesh21a.html?ref=journey-matters), OpenAI
4. [CVPR'22] [High-Resolution Image Synthesis with Latent Diffusion Models](https://arxiv.org/abs/2112.10752), Heidelberg University
5. [arXiv 2024.03] [Scaling Rectified Flow Transformers for High-Resolution Image Synthesis](https://arxiv.org/pdf/2403.03206), Stability AI
6. [arXiv 2023.11] [Stable Video Diffusion](https://static1.squarespace.com/static/6213c340453c3f502425776e/t/655ce779b9d47d342a93c890/1700587395994/stable_video_diffusion.pdf), Stability AI
7. [arXiv 2024.03] [Fast High-Resolution Image Synthesis with Latent Adversarial Diffusion Distillation](https://arxiv.org/abs/2403.12015), Stability AI
8. [ICCV'23] [Scalable Diffusion Models with Transformers](https://arxiv.org/abs/2212.09748), UC Berkeley
9. [arXiv 2023.11] [RichDreamer: A Generalizable Normal-Depth Diffusion Model for Detail Richness in Text-to-3D](https://github.com/modelscope/richdreamer), Alibaba
10. [arXiv 2024.03] [DreamReward: Text-to-3D Generation with Human Preference](https://arxiv.org/abs/2403.14613), Tsinghua University
11. [CVPR'24 Highlight] [HumanGaussian: Text-Driven 3D Human Generation with Gaussian Splatting](https://github.com/alvinliu0/HumanGaussian?tab=readme-ov-file), The Chinese University of Hong Kong
12. [NeurIPS'23] [DreamHuman: Animatable 3D Avatars from Text](https://openreview.net/pdf?id=rheCTpRrxI), Google Research