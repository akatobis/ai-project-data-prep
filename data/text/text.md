### Issue type

Bug

### Have you reproduced the bug with TensorFlow Nightly?

Yes

### Source

source

### TensorFlow version

2.20.0

### Custom code

Yes

### OS platform and distribution

_No response_

### Mobile device

_No response_

### Python version

3.10

### Bazel version

_No response_

### GCC/compiler version

_No response_

### CUDA/cuDNN version

CUDA 12.5.1, cuDNN 9

### GPU model and memory

NVIDIA RTX A4000, 16 GB

### Current behavior?

Compute-Sanitizer reports an out of bounds Write on D2S_NCHW_LOOP. The op returns without an error.

### Standalone code to reproduce the issue

```shell
import tensorflow as tf

with tf.device("/GPU:0"):
    x = tf.ones([181, 364, 181, 181], dtype=tf.float16)
    y = tf.raw_ops.DepthToSpace(input=x, block_size=2, data_format="NCHW")
```

### Relevant log output

```shell
========= Invalid __global__ write of size 2 bytes
=========     at void tensorflow::<unnamed>::D2S_NCHW_LOOP<Eigen::half, (int)2>(int, const T1 *, int, int, int, int, T1 *)+0x620
=========     by thread (416,0,0) in block (28,0,0)
=========     Address 0x7fa1fc058a90 is out of bounds
=========     and is 4294604144 bytes before the nearest allocation at 0x7fa2fc000000 of size 4316851448 bytes
```