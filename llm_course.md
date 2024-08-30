---
title: Efficient LLMSys
layout: default
nav_order: 5
has_children: true
---
To push the last mile of LLM deplyment, a lot of works focus on training and serving LLMs within low cost. In this blog, we summarize the most important works in efficient LLMs.

## Table of contents
- [Table of contents](#table-of-contents)
- [Background](#background)
  - [Transformer Math](#transformer-math)
    - [Inference](#inference)
    - [Training](#training)
- [Survey](#survey)
- [Efficient LLM Serving](#efficient-llm-serving)
  - [Serving LLMs over Heterogeneous Environment](#serving-llms-over-heterogeneous-environment)
  - [Datasets](#datasets)
- [Efficient LLM Training](#efficient-llm-training)
- [Conclusion](#conclusion)
- [Reference](#reference)

## Background
To catch up the latest progress of efficient LLMs, I highly recommend [TinyML and Efficient Deep Learning Computing](https://hanlab.mit.edu/courses/2023-fall-65940) from MIT Song Lab and [AI-Systems (LLM Edition)](https://learning-systems.notion.site/AI-Systems-LLM-Edition-294-162-Fall-2023-661887583bd340fa851e6a8da8e29abb) from UC Berkeley.

### Transformer Math
Before we dive into the efficient LLMs, let's review some math behind transformers. It refers to two popular tech blogs ([Transformer Math 101](https://blog.eleuther.ai/transformer-math/) and [Transformer Inference Arithmetic](https://kipp.ly/transformer-inference-arithmetic/)).

#### Inference

#### Training


## Survey
- [arXiv 2023.02] [Full Stack Optimization of Transformer Inference: a Survey](https://arxiv.org/abs/2302.14017), UC Berkeley
  - A new full-stack optimization hardware for transformer models.
  - Arithmetic intensity is a key metric to evaluate the efficiency of the DNN models.
- [arXiv 2023.12] [Towards Efficient Generative Large Language Model Serving: A Survey from Algorithms to Systems](https://arxiv.org/pdf/2312.15234), Carnegie Mellon University
- [TMLR 2024] [Efficient Large Language Models: A Survey](https://github.com/AIoT-MLSys-Lab/Efficient-LLMs-Survey), The Ohio State University
- [arXiv 2024.04] [A Survey on Efficient Inference for Large Language Models](https://arxiv.org/abs/2404.14294), Infinigence-AI and Tsinghua University

## Efficient LLM Serving
LLM pipeline (stages), cost estimation, metrics.

### Serving LLMs over Heterogeneous Environment

_As AWS reported, 90% of the machine learning demand in the cloud is for inference._

_Key challenges: KV-cache management for long-texts and sparse computation of MoE architectures_

- [ASPLOS 2023] [STI: Turbocharge NLP Inference at the Edge via Elastic
Pipelining](https://arxiv.org/pdf/2207.05022), University of Virginia.

- [ACL 2023 Demo] [Petals: Collaborative Inference and Fine-tuning of Large Models](https://aclanthology.org/2023.acl-demo.54/), HSE University.
  - An implementation of naive model pipeline parallelisms across heterogeneous devices. 

- [arXiv 2024.04] [M´elange: Cost Efficient Large Language Model Serving by
Exploiting GPU Heterogeneity](https://arxiv.org/pdf/2404.14527), UC Berkeley.
  - A cost-efficient GPU allocation stategy for LLM serving (model request size, request rate, and latency service-level objective).

- [MLSys 2024] [HeteGen: Heterogeneous Parallel Inference for Large Language Models on Resource-Constrained Devices](https://arxiv.org/abs/2403.01164), National University of Singapore.
  - A I/O aware parallel strategy for on-device LLM serving.

- [ICML 2024] [HEXGEN: Generative Inference of Large Language Model over Heterogeneous Environment](https://arxiv.org/pdf/2311.11514), The Hong Kong University of Science and Technology and ETH Zurich.
    - Without a careful model partition strategy, naive pipeline and tensor parallelism lead to out-of-memory errors.
    - An [implementation](https://github.com/Relaxed-System-Lab/HexGen) that accommodates tensor model parallelism and pipeline parallelism. 
- [ICML 2023] [The case for 4-bit precision: k-bit Inference Scaling Laws](https://arxiv.org/abs/2212.09720), University of Washington.
  - LLM inference is a memory-bound task and 4-bit quantization is always a best choice for LLM compression. 


### Datasets
1. [AzurePublicDataset](https://github.com/Azure/AzurePublicDataset): Microsoft Azure Traces  

## Efficient LLM Training
The standard LLM training phrase contains three stages: pre-train, fine-tune, and alignment.

- Pre-train aims to learn a common knowledge from a large-scale corpus (~20P tokens, P is the number of parameters).
- Fine-tune targets to improve task-related capability on instruction dataset, such as machine translation (~1%-5% pre-train data).
- Alignment aims to tune the trained model with human perference. It makes LLMs output more responsible and consistent answers.


## Conclusion

---

## Reference
1. [Github Repo] [HetServe-LLMs: A Overview of Efficiently Serving Large Language Models across Edge Devices](https://github.com/Jason-cs18/HetServe-LLMs), New York University and Shandong University.
2. [Github Repo] [Awesome LLM Systems Papers](https://github.com/AmberLJC/LLMSys-PaperList), University of Michigan.
3. [ISCA 2024] [Splitwise: Efficient generative LLM inference using phase splitting](https://www.microsoft.com/en-us/research/publication/splitwise-efficient-generative-llm-inference-using-phase-splitting/), University of Washington.
4. [SIGCOMM 2024] [CacheGen: KV Cache Compression and Streaming for Fast Large Language Model Serving](https://arxiv.org/abs/2310.07240), University of Chicago
5. [arXiv 2024.05] [CacheBlend: Fast Large Language Model Serving with Cached Knowledge Fusion](https://arxiv.org/abs/2405.16444), University of Chicago