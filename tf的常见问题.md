#### 1. tf 2.0版本运行1.0版本的代码

```python
# 导入tf的时候这样导入
import tensorflow.compat.v1 as tf
tf.disable_v2_behavior()
```

#### 2. tf在运行时出现内存溢出的情况(out of memory)

```python
import tensorflow as tf
config = tf.ConfigProto(allow_soft_placement=True)

#最多占gpu资源的70%
gpu_options = tf.GPUOptions(per_process_gpu_memory_fraction=0.7)

#开始不会给tensorflow全部gpu资源 而是按需增加
config.gpu_options.allow_growth = True

sess = tf.Session(config=config)
```

#### 3.keras在运行时出现内存溢出的情况(CUDA_ERROE_OUT_OF_MEMORY)

```python
import keras.backend.tensorflow_backend as KTF
config = tf.ConfigProto(allow_soft_placement=True)

#最多占gpu资源的70%
#gpu_options = tf.GPUOptions(per_process_gpu_memory_fraction=0.7)

#开始不会给tensorflow全部gpu资源 而是按需增加
config.gpu_options.allow_growth = True

session = tf.Session(config=config)
KTF.set_session(session)
```

