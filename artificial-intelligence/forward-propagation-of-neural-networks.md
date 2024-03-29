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
   2-5. [Sequential Method, Model-subclassing](#2-5-sequential-method-model-subclassing)

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