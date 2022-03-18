---
title: Python | Keras
date: 2020-05-10 10:29:53
tags: [Python]
categories: Python
mathjax: true
math: true
hide: true
notshow: true
---

<center></center>
<!--more-->

# keras
Keras 是一个用 Python 编写的高级神经网络 API，它能够以 TensorFlow, CNTK, 或者 Theano 作为后端运行。

## 安装
- 使用PyPI安装
 ```shell
 ## Anaconda Prompt
 pip install keras
 ```
- 使用GitHub源码安装
 ```shell
 git clone https://github.com/keras-team/keras.git
 cd keras
 python setup.py install
 ```

## 后端

- Keras不处理如张量乘积、卷积等低级操作
- 依赖于一个专门的、优化的张量操作库来完成这个操作，它可以作为 Keras 的「后端引擎」
- Keras 有三个后端实现可用: 
 - TensorFlow 后端
 - Theano 后端
 - CNTK 后端

配置见：[Keras后端配置](https://keras.io/zh/backend/)

## Sequential顺序模型
- 由多个网络层线性堆叠

```Python
from keras.models import Sequential

model = Sequential()
```

### 堆叠
可以使用`.add()`来堆叠模型：
```Python
from keras.models import Sequential
from keras.layers import Dense

model.add(Dense(units=64, activation='relu', input_dim=100))
model.add(Dense(units=10, activation='softmax'))
```

模型需要知道它所期望的输入的尺寸——因此，模型的第一层[^1]需要接收关于其输入尺寸的信息。
[^1]: 且只有第一层需要，下面的层可以自动地推断尺寸

输入尺寸的方法：
- 传递`input_shape`参数给第一层
 - `input_shape`是一个表示尺寸的元组[^2]
 - `input_shape`中不包含数据的batch大小
- 某些2D层（如`Dense`）支持通过参数`input_dim`指定输入尺寸；某些3D时序层支持`input_dim`和`input_length`参数
- 如果需要为输入指定一个固定的`batch`大小，可以传递参数`batch_size`给一个层

 > 如：同时将`batch_size=32`和`input_shape=(6,8)`传递给一个层，则每一批输入的尺寸为`(32, 6, 8)`

[^2]: 一个由整数或`None`组成的元组，其中`None`表示可能为任何正整数。

```Python
model = Sequential()
model.add(Dense(32, input_shape=(784,)))
## 等价于
model = Sequential()
model.add(Dense(32, input_dim=784))
```

### 编译
构建模型之后，可以使用`.compile()`来配置学习过程。
- 优化器`optimizer`：可以是现有优化器的字符串标识符，也可以是Optimizer类的实例。
 - 现有优化器的字符串标识符：`rmsprop`、`adagrad`
 - Optimizer类的实例：[SGD](https://keras.io/api/optimizers/sgd)、[RMSprop](https://keras.io/api/optimizers/rmsprop)、[Adam](https://keras.io/api/optimizers/adam)、[Adadelta](https://keras.io/api/optimizers/adadelta)、[Adagrad](https://keras.io/api/optimizers/adagrad)、[Adamax](https://keras.io/api/optimizers/adamax)、[Nadam](https://keras.io/api/optimizers/Nadam)、[Ftrl](https://keras.io/api/optimizers/ftrl)
- 损失函数`loss`：模型试图最小化的目标函数；可以是现有损失函数的字符串标识符，也可以是一个目标函数。
 - 现有损失函数的字符串标识符：`categorical_crossentropy`、`mse`
 - 目标函数：见[losses](https://keras.io/api/losses/)
- 评估标准`metrics`：可以是现有的标准的字符串标识符，也可以是自定义的评估标准函数。
 - `metrics=['accuracy']`


```Python
model.compile(loss='categorical_crossentropy',
              optimizer='sgd',
              metrics=['accuracy'])
## 自定义评估标准函数
import keras.backend as K  ## 加载后端

def mean_pred(y_true, y_pred):
    return K.mean(y_pred)

model.compile(optimizer='rmsprop',
              loss='binary_crossentropy',
              metrics=['accuracy', mean_pred])
```

- 多分类问题
 ```Python
 model.compile(optimizer='rmsprop',
              loss='categorical_crossentropy',
              metrics=['accuracy'])
 ```
- 二分类问题
 ```Python
 model.compile(optimizer='rmsprop',
              loss='binary_crossentropy',
              metrics=['accuracy'])
 ```
- 回归问题
 ```Python
 model.compile(optimizer='rmsprop',
              loss='mse')
 ```



### 训练

然后在训练集数据上进行迭代(**训练**)：
```Python
model.fit(x_train, y_train, epochs=5, batch_size=32)
```

**评估**模型性能:
```Python
loss_and_metrics = model.evaluate(x_test, y_test, batch_size=128)
```

进行**预测**：
```Python
pred_classes = model.predict(x_test, batch_size=128)
```

## utils
### to_categorical()
进行One-Hot Encoding

```Python
from keras.utils import to_categorical
data = [1, 3, 2, 0, 3, 2, 2, 1, 0, 1]
tc = to_categorical(data)
tc
#array([[0., 1., 0., 0.],
#       [0., 0., 0., 1.],
#       [0., 0., 1., 0.],
#       [1., 0., 0., 0.],
#       [0., 0., 0., 1.],
#       [0., 0., 1., 0.],
#       [0., 0., 1., 0.],
#       [0., 1., 0., 0.],
#       [1., 0., 0., 0.],
#       [0., 1., 0., 0.]], dtype=float32)
```
共有{0,1,2,3}四个类别，所以是四维变量。

```Python

```


```Python

```


```Python

```

# 参考资料
- [Keras中文文档](https://keras.io/zh/)