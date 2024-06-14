---
title: AI Accelerator
layout: default
nav_order: 6
---
AI tasks are computationally intensive, and the use of GPUs is essential for accelerating deep learning jobs. In our daily work, we often notice that GPUs are under-utilized and perform poorly. Thus, it is a good idea to understand the basic usage of GPUs and how to optimize deep learning jobs for better performance. In this article, we will dive into GPU architecture and introduce basic usage of CUDA programing which is a parallel computing platform and application programming interface developed by NVIDIA.

## GPU Architecture
> related materials:
> 1. [How GPU Computing Works, NVIDIA GTC 2021](https://www.nvidia.com/en-us/on-demand/session/gtcspring21-s31151/)
> 2. [Programming Heterogeneous Computing Systems with GPUs and other Accelerators (Spring 2023), ETH Zurich](https://safari.ethz.ch/projects_and_seminars/spring2023/doku.php?id=heterogeneous_systems)

GPUs are designed to perform highly parallel computations, making them well-suited for tasks such as deep learning and computer graphics. They are made up of a large number of processing cores, each of which can execute a single instruction at a time. These cores are connected by a high-speed interconnect, allowing them to communicate with each other and coordinate their work.

## CUDA Programming
> related materials:
> 1. [Lecture 11: TinyEngine and Parallel Processing](https://www.dropbox.com/scl/fi/42b23nby5k5d09bpwd1cx/lec11.pdf?rlkey=e2ce7bs8ssgtb82isxgv4y7ij&e=1&dl=0)
> 2. [Lab05: Optimize LLM on Edge Devices](https://docs.google.com/document/d/13IaTfPKjp0KiSBEhPdX9IxgXMIAZfiFjor37OWQJhMM/edit)

## CUDA + PyTorch
> related materials:
> 1. [CUDA semantic](https://pytorch.org/docs/stable/notes/cuda.html#cuda-memory-management)
> 2. [Custom C++ and CUDA Extensions](https://pytorch.org/tutorials/advanced/cpp_extension.html?highlight=cuda)

## DL Profiling
> related materials:
> 1. [PyTorch Profiler](https://pytorch.org/tutorials/recipes/recipes/profiler_recipe.html?highlight=profil)
> 2. [Profiling your PyTorch Module](https://pytorch.org/tutorials/beginner/profiler.html?highlight=profiler)
> 3. [PyTorch Profiler With TensorBoard](https://pytorch.org/tutorials/intermediate/tensorboard_profiler_tutorial.html?highlight=profile)
> 4. [Deep Learning Profiler](https://docs.nvidia.com/deeplearning/frameworks/dlprof-user-guide/index.html)

## Conclusion

## References