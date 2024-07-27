# Forward propagation of neural networks

1. [Artificial Neurons](#1-artificial-neurons) <br>
    1-1. [Parametric Functions](#1-1-parametric-functions) <br>
    1-2. [Dataset 표기](#1-2-dataset-표기) <br>
    1-3. [Minibatch in Artificial Neurons](#1-3-minibatch-in-artificial-neurons) <br>
    1-4. [Affine Function 구현](#1-4-affine-function-구현) <br />
    1-5. [Activate Function 구현](#1-5-activate-function-구현) <br />
2. [Dense Layers](#2-dense-layers) <br />
    2-1. [Dense Layer 란](#2-1-dense-layer-란) <br />
    2-2. [Generalized Dense Layer](#2-2-generalized-dense-layer) <br />
    2-3. [Dense Layer 구현](#2-3-dense-layer-구현) <br />
    2-4. [Cacaded Dense Layers](#2-4-cacaded-dense-layers) <br />
    2-5. [Sequential Method, Model-subclassing](#2-5-sequential-method-model-subclassing) <br />
3. [Sigmoid and Softmax](#3-sigmoid-and-softmax) <br />
    3-1. [Odds](#3-1-odds) <br />
    3-2. [Logit](#3-2-logit) <br />
    3-3. [Sigmoid](#3-3-sigmoid) <br />
    3-4. [Softmax](#3-4-softmax) <br />
    3-5. [Binary Classifiers 구현](#3-5-binary-classifiers-구현) <br />
    3-6. [Multiclass Classifiers 구현](#3-6-multiclass-classifiers-구현) <br />
4. [Loss Functions](#4-loss-functions) <br />
    4-1. [Mean Squared Error (MSE)](#4-1-mean-squared-error-mse) <br />
    4-2. [Binary Cross Entropy (BCE)](#4-2-binary-cross-entropy-bce) <br />
    4-3. [Categorical Cross Entropy (CCE)](#4-3-categorical-cross-entropy-cce) <br />
5. [Conv Layers](#5-conv-layers) <br />
    5-1. [Image Tensors](#5-1-image-tensors) <br />
    5-2. [Window Extraction](#5-2-window-extraction) <br />
    5-3. [Computations of Conv Layer](#5-3-computations-of-conv-layer) <br />
    5-4. [n-Channel Input](#5-4-n-channel-input) <br />

## 1. Artificial Neurons

### 1-1. Parametric Functions

함수 $f(x;\theta)$ 가 존재할 때, 함수가 가지고 있는 $\theta$ 값이 존재하여 입력값 $x$에 영향을 줌 <br />
함수 $f(x;\theta)$ 와 $f(x;\theta')$ 에 대해 $\theta \neq \theta'$ 면 입력값 $x$에 대해 출력값 $z$가 다르게 나옴

$$
 x \in R \rightarrow f(x;\theta) \rightarrow z \in R
$$

### 1-2. Dataset 표기

일반적으로 $\vec{x}$를 나타낼 때, 열벡터로 나타내는데 딥러닝에서는 트랜스포즈를 해준 행벡터로 나타냄
(데이터 읽기가 더 쉬움)

$$
 X^\top(\in R^{N\times k}) = 
 \begin{Bmatrix}
    (\vec{x}^{(1)})^{\top} \\ 
    (\vec{x}^{(2)})^\top \\ 
    \cdots \\ 
    (\vec{x}^{(N)})^\top \\ 
 \end{Bmatrix} = 
 \begin{Bmatrix} 
    x_1^{(1)}&x_2^{(1)}&\cdots&x_k^{(1)} \\
    x_1^{(2)}&x_2^{(2)}&\cdots&x_k^{(2)} \\ 
    \cdots&\cdots&\ddots&\vdots \\ 
    x_1^{(N)}&x_2^{(N)}&\cdots&x_k^{(N)} \\ 
 \end{Bmatrix}
$$

### 1-3. Minibatch in Artificial Neurons

Input -> Affine Function -> Activate Function -> Output

$$
X^T = 
 \begin{Bmatrix} 
    \leftarrow&\overrightarrow{x^{(1)}}^{{(T)}}&\rightarrow \\
    \leftarrow&\overrightarrow{x^{(2)}}^{{(T)}}&\rightarrow \\
    &\vdots& \\
    \leftarrow&\overrightarrow{x^{(N)}}^{{(T)}}&\rightarrow \\
 \end{Bmatrix}  \rightarrow 
 f(X^T;\overrightarrow{w}, b) =
 \begin{Bmatrix} 
    \leftarrow&\overrightarrow{x^{(1)}}^{{(T)}}\cdot\overrightarrow{w} + b&\rightarrow \\
    \leftarrow&\overrightarrow{x^{(2)}}^{{(T)}}\cdot\overrightarrow{w} + b&\rightarrow \\
    &\vdots& \\
    \leftarrow&\overrightarrow{x^{(N)}}^{{(T)}}\cdot\overrightarrow{w} + b&\rightarrow \\
 \end{Bmatrix}  \rightarrow 
Activation Function(a) =
 \begin{Bmatrix} 
    \leftarrow&a(\overrightarrow{x^{(1)}}^{{(T)}}\cdot\overrightarrow{w} + b)&\rightarrow \\
    \leftarrow&a(\overrightarrow{x^{(2)}}^{{(T)}}\cdot\overrightarrow{w} + b)&\rightarrow \\
    &\vdots& \\
    \leftarrow&a(\overrightarrow{x^{(N)}}^{{(T)}}\cdot\overrightarrow{w} + b)&\rightarrow \\
 \end{Bmatrix}
$$

### 1-4. Affine Function 구현

위 [1-3](#1-3-minibatch-in-artificial-neurons)의 minibatch 내용에서 먼저 Affine Function을 구현해보는데 해당 함수는 **입력값들을 선형으로 변환하는 역할**을 하며 tensorflow의 Dense를 통해 구현하는 것과 해당 Dense에서 사용한 $\overrightarrow{w}, b$ 값들을 추출하여 직접 계산을 통해 구현하는 방법으로 진행

```python
import tensorflow as tf

from tensorflow.keras.layers import Dense
x = tf.random.uniform(shape=(1,10), minval=0, maxval=10)

print('---- x 값 ----')
print(x.numpy())

dense = Dense(units=1)

y_tf = dense(x) # Dense 함수를 통한 수행

W,B = dense.get_weights() # Dense에서 사용한 가중치, 편향값 가져오기

print('---- W(가중치), B(편향) 값 ----')
print("W : {}\n{}\n".format(x.shape, W))
print("B : {}\n{}\n".format(B.shape, B))

y_man = tf.linalg.matmul(x, W)+B # 행렬곱 + 편향

print('---- tensorflow 통한 결과 ----')
print("y(Tensorflow):{}\n{}\n)".format(y_tf.shape, y_tf.numpy()))

print('---- 직접 계산 통한 결과 ----')
print("y(직접계산): {}\n{}\n".format(y_man.shape, y_man.numpy()))
```

```javascript
// 출력값
---- x 값 ----
[[8.024992   3.983494   5.5374994  3.7628865  8.116709   3.0038893
  2.4645126  1.1512434  7.598428   0.44297814]]
---- W(가중치), B(편향) 값 ----
W : (1, 10)
[[-0.01062649]
 [ 0.4972629 ]
 [ 0.25603515]
 [ 0.299658  ]
 [-0.11099654]
 [ 0.2427963 ]
 [ 0.7105146 ]
 [ 0.1731705 ]
 [ 0.71067625]
 [ 0.04027861]]

B : (1,)
[0.]

---- tensorflow 통한 결과 ----
y(Tensorflow):(1, 1)
[[11.637645]]
)
---- 직접 계산 통한 결과 ----
y(직접계산): (1, 1)
[[11.637645]]

// tensorflow 통한 결과와 직접 계산을 통한 결과를 보면 동일한 것을 확인할 수 있음.
```

### 1-5. Activate Function 구현

다음으로는 Affine Function을 통해 나온 선형 값을 비선형으로 만들기 위해 Activate Function을 구현함. 대부분의 해결이 필요한 문제는 선형이 아닌 비선형으로 Activate Function은 **선형을 비선형으로 바꿔주는 활성화 함수**임. 해당 예제에서는 많이 사용되는 sigmoid, tanh, relu를 사용

```python
import tensorflow as tf

from tensorflow.keras.layers import Dense
from tensorflow.math import exp, maximum

x = tf.random.uniform(shape=(1,10), minval=0, maxval=10)

# sigmoid, tanh, relu Dense 생성
dense_sigmoid = Dense(units = 1 , activation = 'sigmoid')
dense_tanh = Dense(units = 1 , activation = 'tanh')
dense_relu = Dense(units = 1 , activation = 'relu')

# forward propagation
y_sigmoid = dense_sigmoid(x)
y_tanh = dense_tanh(x)
y_relu = dense_relu(x)

# 아래는 직접계산과 tensorflow 통한 결과를 출력

# sigmoid는 0과 1사이의 값으로 변환
W,B = dense_sigmoid.get_weights()
z = tf.linalg.matmul(x, W) + B
a = 1 / (1 + exp(-z))

print("sigmoid(Tensorflow)= {}\n{}".format(y_sigmoid.shape, y_sigmoid.numpy()))
print("sigmoid(manual)= {}\n{}".format(a.shape, a.numpy()))

# tanh은 -1 과 1 사이의 값으로 변환
W,B = dense_tanh.get_weights()
z = tf.linalg.matmul(x, W) + B
a = (exp(z) - exp(-z)) / (exp(z) + exp(-z))

print("tanh(Tensorflow)= {}\n{}".format(y_tanh.shape, y_tanh.numpy()))
print("tanh(manual)= {}\n{}".format(a.shape, a.numpy()))

# relu는 0보다 작은 값은 0으로 만들어 0 >= 의 값으로 변환
W,B = dense_relu.get_weights()
z = tf.linalg.matmul(x, W) + B
a = maximum(z, 0)

print("relu(Tensorflow)= {}\n{}".format(y_relu.shape, y_relu.numpy()))
print("relu(manual)= {}\n{}".format(a.shape, a.numpy()))
```

```javascript
// 출력값
sigmoid(Tensorflow)= (1, 1)
[[0.9971153]]
sigmoid(manual)= (1, 1)
[[0.99711525]]
tanh(Tensorflow)= (1, 1)
[[-0.99985605]]
tanh(manual)= (1, 1)
[[-0.9998561]]
relu(Tensorflow)= (1, 1)
[[23.227852]]
relu(manual)= (1, 1)
[[23.227852]]
```

## 2. Dense Layers

### 2-1. Dense Layer 란

앞서 [1-3](#1-3-minibatch-in-artificial-neurons)에서 보였던 artificial neurons 여러개를 하나의 층에 모아 모든 입력 노드가 모든 neurons 노드에 연결되어 출력값을 내보내는 Fully Connected Layer 를 말한다.

<p align="center">
    <img width="400" alt="Dense Layer" src="https://github.com/jongeunShin95/TIL/assets/20867824/682783fc-84ae-42ae-b730-4df0d76b2a45">
    <p align="center"><I>Dense Layer</I></p>
</p>


### 2-2. Generalized Dense Layer

Dense Layer 에 대해 일반화를 해보면 다음과 같다.

<p align="center">
    <img width="450" alt="Generalized Dense Layer" src="https://github.com/jongeunShin95/TIL/assets/20867824/b329c10b-d105-4a6a-abb7-12c7fd5fd01f">
    <p align="center"><I>Generalized Dense Layer</I></p>
</p>

### 2-3. Dense Layer 구현

하나의 Dense Layer를 구현해본다. tensorflow 의 Dense 를 이용하는 방법, 행렬 곱을 이용하는 방법, dot product 를 이용해 하나의 벡터를 가져와 구하는 방법으로 구현해본다.

```python
import tensorflow as tf
import numpy as np

from tensorflow.math import exp
from tensorflow.linalg import matmul

from tensorflow.keras.layers import Dense

N, n_feature = 4, 10
X = tf.random.normal(shape=(N, n_feature))

n_neuron = 3
dense = Dense(units=n_neuron, activation='sigmoid')
Y_tf = dense(X)

W, B = dense.get_weights()
print("Y(Tensorflow): \n", Y_tf.numpy())

# 행렬 곱
Z = matmul(X, W) + B 
Y_man_matmul = 1/(1 + exp(-Z))
print("Y(행렬 곱): \n", Y_man_matmul.numpy())

# dot products
Y_man_vec = np.zeros(shape=(N, n_neuron))
for x_idx in range(N):
  x = X[x_idx] # 입력 값 하나를 들고옴

  for nu_idx in range(n_neuron):
    w, b = W[:, nu_idx], B[nu_idx] # nu_idx 번째 laye의 weight, bias 를 들고옴

    z = tf.reduce_sum(x * w) + b # Affine Function
    a = 1/(1 + np.exp(-z)) # Activate Function
    Y_man_vec[x_idx, nu_idx] = a # 첫 번재 출력값에 넣는다

print("Y(dot products): \n", Y_man_vec)
```

```javascript
// 출력
Y(Tensorflow): 
 [[0.6248131  0.28246373 0.11592089]
 [0.5112781  0.7338823  0.1971219 ]
 [0.84512705 0.870962   0.48430094]
 [0.6721053  0.56761324 0.18266279]]
Y(행렬 곱): 
 [[0.6248131  0.28246373 0.11592088]
 [0.51127803 0.7338823  0.1971219 ]
 [0.8451271  0.870962   0.48430097]
 [0.6721053  0.5676133  0.18266279]]
Y(dot products): 
 [[0.62481306 0.28246372 0.11592089]
 [0.51127802 0.7338823  0.19712189]
 [0.84512704 0.87096207 0.48430094]
 [0.67210529 0.56761326 0.18266279]]
```

### 2-4. Cacaded Dense Layers

이번에는 하나의 layer 가 아닌 여러 layer를 거쳐 출력값을 내는 것을 구현해본다. 각 layer의 벡터 수는 list를 통해 담을 것이며 직접 계산하는 방법은 각 layer의 weight, bias를 가져와 위의 Dense Layer 구현처럼 해주면 되기에 여기서는 tensorflow 를 이용하여 전체 layer 를 거치는 구현만 해본다.

```python
import tensorflow as tf

from tensorflow.keras.layers import Dense

N, n_feature = 4, 10

X = tf.random.normal(shape=(N, n_feature))

n_neurons = [10, 20, 30, 40]

dense_layers = list()

for n_neuron in n_neurons:
  dense = Dense(units=n_neuron, activation='sigmoid')
  dense_layers.append(dense)
print("Input: ”", X.shape) # 입력값 shape

for dense_idx, dense in enumerate(dense_layers):
  X = dense(X)
  print("After dense layer", dense_idx + 1)
  print(X.shape, '\n') # 확인해보면 입력 데이터의 수는 변하지 않고 특징의 수만 각 layer의 뉴런수에 맞추어 바뀐다

print(X)
```

```javascript
// 출력
Input: ” (4, 10)
After dense layer 1
(4, 10) 

After dense layer 2
(4, 20) 

After dense layer 3
(4, 30) 

After dense layer 4
(4, 40) 

tf.Tensor(
[[0.36952984 0.5738927  0.5958621  0.43816036 0.6132642  0.61638796
  0.4616614  0.3413767  0.4997409  0.3554447  0.64101875 0.59697604
  0.52230895 0.6227869  0.53060853 0.5190789  0.4516735  0.53102255
  0.35540646 0.4640545  0.6340574  0.535441   0.5529383  0.29057053
  0.41894445 0.47920665 0.6271921  0.6710545  0.43481573 0.5106191
  0.26321265 0.6047313  0.40307227 0.42829984 0.57516927 0.26252675
  0.5370714  0.6098057  0.46224457 0.7280118 ]
 [0.3703943  0.5745397  0.5963264  0.44085315 0.61256796 0.61649734
  0.4524212  0.35261068 0.49870422 0.3521004  0.6348808  0.58728033
  0.51829404 0.61566067 0.53116477 0.526372   0.46271402 0.52544487
  0.35780486 0.46801403 0.63321584 0.5334377  0.5476298  0.28990865
  0.41993418 0.48588988 0.6273501  0.67072004 0.43820715 0.5146493
  0.2649437  0.5999989  0.40433994 0.43493846 0.5781825  0.26609692
  0.54273194 0.60785884 0.46184507 0.7247313 ]
 [0.3703816  0.572974   0.5963867  0.4386883  0.6129524  0.61773103
  0.45582485 0.34526333 0.49888924 0.3510286  0.63896066 0.59379804
  0.5205332  0.61936975 0.5300334  0.52029365 0.4563395  0.5288603
  0.3562708  0.46551642 0.63535154 0.5350623  0.55156195 0.28753614
  0.41808382 0.48226836 0.6265653  0.671988   0.4378254  0.5106438
  0.2629788  0.60354316 0.40182024 0.43036518 0.5786785  0.2628922
  0.5398106  0.6094911  0.46220508 0.72687393]
 [0.36978105 0.5712423  0.5972227  0.43890902 0.61231387 0.616915
  0.4571995  0.34394863 0.49901158 0.35438323 0.6394533  0.5959328
  0.52230424 0.6220205  0.5294079  0.51882875 0.45487574 0.5297611
  0.35462156 0.4657013  0.6341016  0.53713655 0.55160046 0.2888496
  0.4190202  0.4819768  0.62753814 0.6712784  0.43784094 0.5100543
  0.26448822 0.6052451  0.40227118 0.42977884 0.577452   0.26325452
  0.54002184 0.6088079  0.46245918 0.7265016 ]], shape=(4, 40), dtype=float32)
```

### 2-5. Sequential Method, Model-subclassing

위에 처럼, 여러 Dense Layer 를 거칠 때 list 를 통한 방법도 있지만 tensorflow 의 Sequential 함수를 이용하는 방법과 Class 모델을 만드는 방법도 있다.

```python
# Sequential Method
import tensorflow as tf

from tensorflow.keras.layers import Dense
from tensorflow.keras.models import Sequential

X = tf.random.normal(shape=(4, 10))

model = Sequential()
model.add(Dense(units=10, activation='sigmoid'))
model.add(Dense(units=20, activation='sigmoid'))
```

```python
# Model-subclassing
import tensorflow as tf

from tensorflow.keras.layers import Dense
from tensorflow.keras.models import Model

class TestModel(Model):
  def __init__(self):
    super(TestModel, self).__init__()

    self.dense1 = Dense(units=10, activation='sigmoid')
    self.dense2 = Dense(units=20, activation='sigmoid')

  def call(self, x):
    x = self.dense1(x)
    x = self.dense2(x)
    return x

model = TestModel()
```

## 3. Sigmoid and Softmax

이번에는 Dense Layer 에서 activation 함수로 사용했던 sigmoid 와 softmax 를 간단히 알아보려고 한다. 이를 위해, 먼저 odds, logit 을 정리하고 어떻게 sigmoid 와 softmax 가 나왔는지 본다.

### 3-1. Odds

Odds 의 경우 성공할 확률이 실패할 확률보다 얼마나 더 잘 일어날지를 수치화 시킨 함수이다. 예를 들어, p(이길 확률) = 0.6 이면 1 - p(지는 확률) = 0.4 가 된다. 이를 odds 식인 $o=\frac{p}{1-p}$ 에 대입해보면 1.5 가 나오며 이는 이길 확률이 더 크다는 의미가 된다. 다만 해당 함수는 아래 사진과 같이 일어나지 않는 확률이 큰 경우에는 0에 수렴하게 되지만, 일어날 확률이 큰 경우 무한으로 발산한다는 점이 있다.

<p align="center">
    <img width="400" alt="Odds" src="https://github.com/jongeunShin95/TIL/assets/20867824/cb74de22-a699-4504-bbc6-fb3b8956c007">
    <p align="center"><I>Odds</I></p>
</p>

### 3-2. Logit

위의 Odds 의 경우 좌우대칭이지가 않다. 그래서 Odds 에 Log 를 씌우게 되면 $l=log(\frac{p}{1-p})$ 과 같이 되고 해당 함수는 0.5를 기준으로 좌우대칭이 된다. 또한, 확률이 입력되었을 때 logit 의 값은 $-\infty < l < \infty$ 로 정의할 수 있다.

<p align="center">
    <img width="400" alt="Logit" src="https://github.com/jongeunShin95/TIL/assets/20867824/19a7ec26-6f39-4fd9-b0a1-be83c4dd04b0">
    <p align="center"><I>Logit</I></p>
</p>

### 3-3. Sigmoid

그렇다면 이 Sigmoid 는 Logit 함수의 역함수를 나타낸다. 즉, Logit 값이 들어왔을 때 이를 확률로 변환해주는 함수를 말하며 값은 $0 \le p \le 1$ 의 값을 가지게 되며 무한대의 입력값을 받아 0과 1사이의 확률값으로 변환해주는 함수이다. 위에서 affine function -> activation function 을 통과하는 과정에서 affine function 의 값은 무한대로 나오고 이를 activation function (sigmoid) 에 넣어 0과 1사이의 값인 확률값으로 내보내주게 되는 것이다.

> $$P = \frac{1}{1+e^{-l}}$$

### 3-4. Softmax

Sigmoid 함수의 경우에는 각 클래스에 대한 확률값을 나타내기에 각 클래스의 확률값을 더하게 되면 1보다 큰 경우가 발생한다. Softmax 의 경우 전체 클래스에 대한 확률값을 나타내기 때문에 각 클래스의 확률값을 모두 더하면 1이 된다. Sigmoid 의 경우 주로 이진분류에 사용을 하며, Softmax 의 경우 다중분류에 사용을 한다.

> $$P_{j} = \frac{e^{l_{j}}}{\sum_{k=1}^{K}[e^{l_{k}}]}$$


### 3-5. Binary Classifiers 구현

간단하게 마지막에 Sigmoid activation 함수를 하나 넣게 되면 Sigmoid 를 통한 이진분류를 할 수 있다.

```python
import tensorflow as tf
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense

model = Sequential()
model.add(Dense(units=10, activation='relu'))
model.add(Dense(units=5, activation='relu'))
model.add(Dense(units=1, activation='sigmoid' ))
```

### 3-6. Multiclass Classifiers 구현

이번에는 softmax 를 이용한 다중 분류를 구현해 본다.

```python
import tensorflow as tf

from tensorflow.keras.models import Model
from tensorflow.keras.layers import Dense


class SoftmaxModel(Model):
  def __init__(self):
    super(SoftmaxModel, self).__init__()

    self.dense1 = Dense(units=8, activation='relu')
    self.dense2 = Dense(units=5, activation='relu')
    self.dense3 = Dense(units=3, activation='softmax')
    
  def call(self, x):
    print("X: {}\n{}\n".format(x.shape, x.numpy()))
    
    x = self.dense1(x)
    print("A1: {}\n{}\n".format(x.shape, x.numpy()))
    
    x = self.dense2(x)
    print("A2: {}\n{}\n".format(x.shape, x.numpy()))
    
    
    x = self.dense3(x)
    print("Y: {}\n{}\n".format(x.shape, x.numpy()))
    print("Sum of vectors: {}\n".format(tf.reduce_sum(x, axis=1)))
    return x


model = SoftmaxModel()

X = tf.random.uniform(shape=(8, 5), minval=-10, maxval=10)
Y = model(X)
```

```javascript
// 출력값
X: (8, 5)
[[-2.8362179   5.6602716  -9.916644    8.858097   -5.2923465 ]
 [ 5.535366   -2.0934129  -6.090827   -5.2633977   4.1897373 ]
 [ 7.628126    8.5497055  -4.8740053   2.4602299  -0.06941986]
 [-0.8205795  -6.026585   -0.9291153  -9.927177   -0.72073174]
 [-7.5788546  -3.7129474  -2.198174   -7.299173    0.15242767]
 [-9.620283    2.6950645   8.890074    6.219599   -2.510941  ]
 [-4.1099358  -2.7705216   8.075283    6.033863   -2.3529673 ]
 [-2.0200324   9.458847    7.9519463   1.3033628   1.0691214 ]]

A1: (8, 8)
[[0.00000000e+00 2.23685169e+00 4.98971790e-01 1.11412115e+01
  1.41843569e+00 1.92859143e-01 0.00000000e+00 7.65051651e+00]
 [0.00000000e+00 0.00000000e+00 7.77379274e+00 0.00000000e+00
  0.00000000e+00 0.00000000e+00 3.56715870e+00 9.68558192e-01]
 [0.00000000e+00 5.68919373e+00 3.85545683e+00 0.00000000e+00
  0.00000000e+00 2.87591648e+00 0.00000000e+00 9.70282459e+00]
 [1.46606863e+00 0.00000000e+00 0.00000000e+00 0.00000000e+00
  0.00000000e+00 0.00000000e+00 2.99898314e+00 0.00000000e+00]
 [4.06624079e+00 0.00000000e+00 0.00000000e+00 2.43914247e+00
  0.00000000e+00 0.00000000e+00 0.00000000e+00 0.00000000e+00]
 [1.11813192e+01 1.76375210e+00 0.00000000e+00 3.05586720e+00
  5.90002298e+00 7.74090481e+00 0.00000000e+00 0.00000000e+00]
 [6.03417587e+00 4.46024799e+00 0.00000000e+00 9.88518715e-01
  6.53423262e+00 3.15890813e+00 5.13729453e-03 0.00000000e+00]
 [7.90750599e+00 2.92930913e+00 0.00000000e+00 0.00000000e+00
  8.74949574e-01 1.05232296e+01 0.00000000e+00 6.87680542e-01]]

A2: (8, 5)
[[1.8592076  7.8753667  0.         0.         0.19611472]
 [1.9734353  0.         0.         4.685965   0.        ]
 [1.8446039  2.7184658  0.         0.         0.        ]
 [0.         0.         0.9891865  0.9065626  0.        ]
 [0.         0.         3.0557928  0.         0.9466025 ]
 [0.         1.1571572  9.9529915  0.         0.        ]
 [3.1585197  1.5295764  2.888393   0.         0.        ]
 [0.         0.         6.7289057  0.         0.        ]]

Y: (8, 3)
[[6.2023551e-04 3.8593782e-05 9.9934119e-01]
 [5.9176213e-03 9.2895132e-01 6.5131098e-02]
 [4.6847496e-02 3.8546607e-02 9.1460598e-01]
 [1.4255357e-01 4.5782888e-01 3.9961749e-01]
 [3.4730490e-02 3.3530048e-01 6.2996906e-01]
 [2.8019311e-04 1.4033772e-02 9.8568600e-01]
 [8.7457104e-03 1.2541927e-01 8.6583495e-01]
 [6.5258066e-03 1.4011060e-01 8.5336363e-01]]

// 최종 결과의 최종 확률 합은 다 1이 되는 것을 볼 수 있다.
Sum of vectors: [1.         1.         1.0000001  0.99999994 1.         0.99999994
 0.99999994 1.        ]

```

## 4. Loss Functions

딥러닝 모델의 경우 X(입력값)이 들어가게 되면 Y(출력값)이 나오게 되며, 이 출력값이 얼마나 정답을 잘 맞추는지가 중요하다. 이때, 정답을 얼마나 맞추냐를 예측하는 평가가 loss function 이며 해당 값을 줄이는 것을 목표로 모델을 학습하게 된다. 보통 Regression 문제의 경우 MSE 를 통해 평가를 하고 Binary Classifiers 의 경우 Binary Cross Entropy, Multiclass Classifiers 의 경우 Categorical Cross Entropy 를 사용한다.

### 4-1. Mean Squared Error (MSE)

먼저 Regression function 에서 일반적으로 사용하는 MSE 의 경우 간단한 손실 함수로 예측값과 실제값의 차이를 제곱한 결과에 대해 평균을 내는 값을 말한다.

$$
 MSE = \frac{1}{N}\sum_{i=1}^{N}(y^i - \hat{y}^i)^2
$$

<br />

간단하게 MSE 를 코드를 통해 실습해본다.

```python
import tensorflow as tf

from tensorflow.keras.losses import MeanSquaredError

loss_object = MeanSquaredError() # MSE 함수

# (32, 1) 의 pred 와 정답셋을 구성한다.
batch_size = 32
predictions = tf.random.normal(shape=(batch_size, 1))
labels = tf.random.normal(shape=(batch_size, 1))

mse = loss_object(labels, predictions) # tensorflow 제공 함수 사용
mse_manual = tf.reduce_mean(tf.math.pow(labels - predictions, 2)) # MSE 식을 이용한 계산

print("MSE(Tensorflow): ", mse.numpy())
print("MSE(Manual): ", mse_manual.numpy())

# 출력값
# MSE(Tensorflow):  2.966487
# MSE(Manual):  2.966487
```

### 4-2. Binary Cross Entropy (BCE)

BCE 의 경우에는 Binary Classifiers 에서 사용되는 손실함수이다. 해당 함수는 $y$값이 1일 때 $ylog(\hat{y})$ 의 식을 적용하고 $y$ 값이 0일 때는 $(1-y)log(1-\hat{y})$ 의 식을 적용시켜 손실함수를 구한다.

$$
  BCE = -[ylog(\hat{y}) + (1-y)log(1-\hat{y})]
$$

<br />

<p align="center">
    <img height="550" width="400" alt="bce_1" src="https://github.com/jongeunShin95/TIL/assets/20867824/880c47c5-52c0-4070-b2cc-36da22e6cf5f">
    <p align="center"><I>bce_1</I></p>
</p>

<br />

<p align="center">
    <img height="550" width="400" alt="bce_0" src="https://github.com/jongeunShin95/TIL/assets/20867824/5cad8e6f-3e07-4e97-b3b7-b1fbff3a4677">
    <p align="center"><I>bce_0</I></p>
</p>

<br />

간단하게 BCE 를 코드를 통해 실습해본다.

```python
import tensorflow as tf

from tensorflow.keras.losses import BinaryCrossentropy

batch_size = 32
n_class = 2 # 0, 1 두 개의 클래스

# 예측값과 정답값을 셋팅해준다.
predictions = tf.random.uniform(shape=(batch_size,1),
                                minval=0, maxval=1,
                                dtype=tf.float32)
labels = tf.random.uniform(shape=(batch_size, 1),
                            minval=0, maxval=n_class,
                            dtype=tf.int32)

# ternsorflow 함수 활용
loss_object = BinaryCrossentropy()
loss = loss_object(labels, predictions)

# 직접 계산
bce_man = -(labels*tf.math.log(predictions) + (1 - labels)*tf.math.log(1 - predictions))
bce_man = tf.reduce_mean(bce_man)

print("BCE(Tenaorflow): ", loss.numpy())
print("BCE(Manual): ", bce_man.numpy())

# 출력값
# BCE(Tenaorflow):  1.0893092
# BCE(Manual):  1.0893099
```

### 4-3. Categorical Cross Entropy (CCE)

Categorical Cross Entropy 의 경우에는 Multi-class Classification 에서 사용된다. 그래서 보통 Softmax 뒤에 이 Loss 함수를 이용한다. 그리고 정답값인 $y$값이 1일 때 $\hat{y}$ 이 살아있어 식이 적용된다.

$$
  CCE = -\sum_{t=1}^{K}y_ilog(\hat{y_i})
$$

<br />

간단하게 CCE 를 코드를 통해 실습해본다.

```python
import tensorflow as tf

from tensorflow.keras.losses import CategoricalCrossentropy

batch_size, n_class = 16, 5

predictions = tf.random.uniform(shape=(batch_size, n_class),
                                minval=0, maxval=1,
                                dtype=tf.float32)

# softmax 의 경우 모든 확률 값을 더하면 1이 되는데 동일하게 하기 위해 각 확률의 합을 나누어 준다.
pred_sum = tf.reshape(tf.reduce_sum(predictions, axis=1), (-1, 1))
predictions = predictions/pred_sum

labels = tf.random.uniform(shape=(batch_size, ),
                          minval=0, maxval=n_class,
                          dtype=tf.int32)

labels = tf.one_hot(labels, n_class)

# tensorflow 에서 제공하는 함수 활용
loss_object = CategoricalCrossentropy()
loss = loss_object(labels, predictions)

print("CCE(Tensorfiow): ", loss.numpy())

# 직접 계산
cce_man = tf.reduce_mean(tf.reduce_sum(-labels*tf.math.log(predictions), axis=1))
print("CCE(Manual): ", cce_man.numpy())

# 출력값
# CCE(Tensorfiow):  2.015347
# CCE(Manual):  2.015347
```


## 5. Conv Layers

Conv Layer 는 Convolutional Layer 의 줄임말로 CNN(Convolutional Neural Network) 에서 사용되는 입력데이터의 특징을 추출하는 layer 이다. CNN 은 주로 이미지를 분석하는데 많이 쓰이기에 해당 layer 에서는 주로 입력데이터가 이미지들이 된다. 또한 Convolution 은 cross-correlation 의 형태로 이루어진 것으로 서로의 상관관계를 구하는 것이다.

### 5-1. Image Tensors

흑백 이미지를 나타내는 Tensor ($H$ - 이지미의 세로, $W$ - 이미지의 가로)

$$X \in R^{n_{H}\times n_{W}}$$

컬러 이미지를 나타내는 Tensor (RGB 구성으로 3개의 채널로 구성됨)

$$X \in R^{n_{H} \times n_{W} \times n_{C}}$$

컬러 Tensor 가 여러개 (컬리 이미지가 여러개)

$$X \in R^{N \times n_{H} \times n_{W} \times n_{C}}$$

### 5-2. Window Extraction

CNN 에서 필터를 통해 데이터의 특징을 추출하게 되는데 이때, 전체 데이터에 대해 필터를 적용하는 것이 아닌 특정 윈도우 사이즈 만큼 데이터를 추출하여 각 윈도우마다 필터를 적용하여 계산하게 된다. 이번에는 윈도우 사이즈에 따라 윈도우를 구하는 방법을 살펴본다.

**Window(1D)**

아래는 벡터에 대해 윈도우 사이즈가 3일 때 나오는 윈도우이다.

<p align="center">
    <img width="400" alt="Window(1D)" src="https://github.com/user-attachments/assets/4d278d2f-c21e-436a-8aea-f77b6d46bcd1">
    <p align="center"><I>Window(1D)</I></p>
</p>

<br />

**Window(2D)**

아래는 2차 행렬에 대해 윈도우 사이즈가 (2, 2)일 때 나오는 윈도우이다.

<p align="center">
    <img width="400" alt="Window(2D)" src="https://github.com/user-attachments/assets/486385c7-f274-416f-9176-df91b8c201e9">
    <p align="center"><I>Window(2D)</I></p>
</p>

<br />

### 5-3. Computations of Conv Layer

윈도우를 구했으니 이번에는 특정 필터가 있을 때 이 윈도우를 통해 계산한 맵을 구하는 것을 살펴본다.

<p align="center">
    <img width="400" alt="Computations" src="https://github.com/user-attachments/assets/2bcbcf3c-a2f8-40fc-96cc-533f1fa01228">
    <p align="center"><I>Computations</I></p>
</p>

<br />

### 5-4. n-Channel Input

마지막으로 여러 채널을 가진 입력값을 여러 개의 필터를 거치는 것을 보면 다음과 같다.

<p align="center">
    <img width="400" alt="n-Channel Input" src="https://github.com/user-attachments/assets/e8003985-e1bb-44e6-9c0c-10fed0dcbd15">
    <p align="center"><I>n-Channel Input</I></p>
</p>

<br />