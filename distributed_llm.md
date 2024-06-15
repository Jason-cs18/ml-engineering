# Overview
Large-language model (LLM) inference are well studied for homogeneous compute clusters. However, these methods are often hard to work on heterogeneous devices. In this paper, we provide an comprehensive study on serving LLMs on heterogeneous devices. We first review requrired backgrounds of LLMs and optimization techniques. Then, we analyze the challenges in serving LLMs on heterogeneous devices. Finally, we propose some promising directions to address challenges.

## Background

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


## Future Directions
- xxx 
- xxx

## Conclusion

## References
1. [Github repo] vLLM https://github.com/vllm-project/vllm
2. [Github repo] TensorRT-LLM https://github.com/NVIDIA/TensorRT-LLM
3. [Github repo] llama.cpp https://github.com/ggerganov/llama.cpp
4. [Github repo] text-generation-inference https://github.com/huggingface/text-generation-inference