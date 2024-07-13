---
title: Monitor
layout: default
parent: Engineer
nav_order: 7
---
_open sentence_

## Table of contents
- [Table of contents](#table-of-contents)
- [Profile time \& memory for Python scripts](#profile-time--memory-for-python-scripts)
- [Monitor GPU with nvitop](#monitor-gpu-with-nvitop)
- [Use PyTorch Profiler to profile PyTorch scripts](#use-pytorch-profiler-to-profile-pytorch-scripts)
- [Monitor DETR with Ray Dashboard](#monitor-detr-with-ray-dashboard)
  - [Add accuracy metrics to Ray Dashboard](#add-accuracy-metrics-to-ray-dashboard)
- [Conclusion](#conclusion)

## Profile time & memory for Python scripts

```python
# pip install line_profiler
# LINE_PROFILE=1 python test_profile.py
# pip install memory-profiler
# python test_profile.py

"""
test_profile.py
"""
import math
from line_profiler import profile
# from memory_profiler import profile

@profile
def test_profile(x: int) -> int:
    """test function for profile"""
    sum = 0
    for i in range(x):
        sum += 1
    
    return sum


def main():
    test_profile(1000)
    

if __name__ == "__main__":
    main()
```



## Monitor GPU with nvitop
```bash
pip install -U nvitop
nvitop # start monitoring
```

## Use PyTorch Profiler to profile PyTorch scripts

## Monitor DETR with Ray Dashboard

### Add accuracy metrics to Ray Dashboard

## Conclusion
In this blog, we have learned ...

We recommend ...