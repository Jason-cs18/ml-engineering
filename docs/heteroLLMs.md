# Overview
Large-language models (LLMs) have revoluted the AI community. They have been used in various applications, from language understanding to code completion. The awesome performance is always accompanied by the large model size and large-scale training data. These high resource demanding make LLMs hard to deploy in production. Now, server-based serving is a popular choice for LLM inference. However, it is hard to meet all application scenarios (such as privacy-preserving applications). Thus, a lot of efforts are devoted to improve the LLM serving on heterogeneous devices. They are mainly divided into two categories: efficient LLMs and parallel computation. The first approach seek to reduce demaning resource via model compression techniques and the second methods try to improve execution efficiency with popular parallel computing mechanisms (e.g., pipeline parallelism, tensor parallelism, offloading).

## Backgrounds of LLMs

### LLM and Scaling Laws
- Decode-only architecture and autogressive generation strategy.
- Emergent capabilities of LLMs and scaling laws

### Efficient LLMs
- Model compression (quantization, distillation)
- Efficient attntion mechanism 
- Efficient decoding mechanism

In sum, efficient LLMs are mainly focused on reducing the model size and the inference latency. But they have to trade-off the accuracy and the efficiency. Even on some extremely-large models (e.g., xxx 170B), optimized LLMs are still hard to loaded by the consumer-grade devices.

- pros: low-memory footprint, low-latency, low-cost
- cons: accuracy drop

### Distrubuted LLM Serving
- Model pipeline parallelism
- Model tensor parallelism

In sum, existing parallel computing mechanisms have been applied and verified in homogeneous environment. But deploy them to heterogeneous devices is still challenging due to ...

## Serving LLMs over Heterogeneous Devices
To serve LLMs in a low-latency and low-cost manner, some recent works about heterogeneity-aware parallel computing are proposed.

**what they have done? cons and pros**


## Software Framework
In this section, we review existing popular serving frameworks for LLM inference.

|Serving Framework|Target|Memory Optimization|Parallel Computation|Heterogeneous|Programming Language|
|:---:|:---:|:---:|:---:|:---:|:---:|
|vLLM [[1]](#references)|Throughput|✔|TP||Python|
|TensorRT-LLM [[2]](#references)|Latency|✔|TP, PP|✔|C++|
|llama.cpp [[3]](#references)|Latency||TP, PP|✔|C++|
|text-generation-inference [[4]](#references)|Latency|✔|TP|✔|Python|
|Petals [[4]](#references)|Throughput||PP|✔|Python|
|LMDeploy [[5]](#references)|Throughput|✔|TP|✔|Python, C++|


## Future Directions

## References
1. [ACL 2024 Demo] [LinguaLinked: A Distributed Large Language Model Inference System for Mobile Devices](https://arxiv.org/pdf/2312.00388), University of California Irvine
2. [ICML 2024] [HEXGEN: Generative Inference of Large Language Model over Heterogeneous Environment](https://arxiv.org/pdf/2311.11514), The Hong Kong University of Science and Technology and ETH Zurich
3. [MLSys 2024] [HeteGen: Heterogeneous Parallel Inference for Large Language Models on Resource-Constrained Devices](https://arxiv.org/abs/2403.01164), National University of Singapore
<!-- ## Background

### LLM
- What is LLM?
  - transformer architecture
  - encoder-only and decoder-only
  - autogressive generation strategy
- Why LLM serving is hard?
  - extremely-large model size
  - high bandwidth requirement
  - low latency requirement
- How to improve LLM serving?
  - fast generation strategy
  - efficient LLMs
  - parallel computation

### Optimization Techniques
- Efficient LLMs
  - More efficient attention (selective, sliding + dilated, global, hash-based)
  - Mixture-of-experts (MoE, decouple model capacity and computation cost)
  - Model compression (pruning and knowledge distillation) 
- Fast generation strategy
  - Non-autogressive decoding (fast but low accuracy)
  - Speculative decoding (high accuracy but incurs memory overheads)
  - Cascade inference (fast but requires an accurate dispatching mechanism)
- Parallel computation
  - Model pipeline parallelism (split model into multiple stages)
  - Model tensor parallelism (split attention/FFN into multiple tensors)

## Challenges
- Serving LLMs on low memory/bandwidth devicesS
- Serving sparse-activated mixture-of-experts (MoE) with model parallelism 
- Memory management of KV-cache on heterogeneous devices

## Software Frameworks
> Model pipeline parallelism (PP)
> Model tensor parallelism (TP)
> Memory optimization denotes using PagedAttention

|Serving Framework|Target|Memory Optimization|Parallel Computation|Heterogeneous|Programming Language|
|:---:|:---:|:---:|:---:|:---:|:---:|
|vLLM [[1]](#references)|Throughput|✔|TP||Python|
|TensorRT-LLM [[2]](#references)|Latency|✔|TP, PP|✔|C++|
|llama.cpp [[3]](#references)|Latency||TP, PP|✔|C++|
|text-generation-inference [[4]](#references)|Latency|✔|TP|✔|Python|
|LMDeploy [[5]](#references)|Throughput|✔|TP|✔|Python, C++|

## Future Directions
- xxx 
- xxx

## Conclusion -->

<!-- ## References
1. [Github repo] vLLM https://github.com/vllm-project/vllm
2. [Github repo] TensorRT-LLM https://github.com/NVIDIA/TensorRT-LLM
3. [Github repo] llama.cpp https://github.com/ggerganov/llama.cpp
4. [Github repo] text-generation-inference https://github.com/huggingface/text-generation-inference
5. [Github repo] LMDeploy https://github.com/InternLM/lmdeploy -->