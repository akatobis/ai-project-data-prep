### Issue type

Bug

### Have you reproduced the bug with TensorFlow Nightly?

Yes

### Source

binary

### TensorFlow version

TensorFlow: 2.23.0-dev20261011

### Custom code

Yes

### OS platform and distribution

Ubuntu24.04

### Mobile device

_No response_

### Python version

3.11.15

### Bazel version

_No response_

### GCC/compiler version

_No response_

### CUDA/cuDNN version

12.0.140

### GPU model and memory

_No response_

### Current behavior?

tf.math.betainc returns NaN under XLA when either shape parameter is +Inf and x is 0 or 1, instead of preserving the expected endpoint values.

Eager / graph: [0, 0, 1, 1]

XLA: [NaN, NaN, NaN, NaN]

This reproduces on both CPU and GPU with float32 and float64.

### Standalone code to reproduce the issue

```shell
import numpy as np
import tensorflow as tf

def fn(a, b, x):
    return tf.math.betainc(a, b, x)

a = tf.constant([np.inf, 1., np.inf, 1.])
b = tf.constant([1., np.inf, 1., np.inf])
x = tf.constant([0., 0., 1., 1.])

with tf.device("/CPU:0"):
    print("Eager:", fn(a, b, x).numpy())
    print("XLA:", tf.function(fn, jit_compile=True)(a, b, x).numpy())
```

### Relevant log output

```shell
Eager: [0. 0. 1. 1.]
XLA: [nan nan nan nan]
```