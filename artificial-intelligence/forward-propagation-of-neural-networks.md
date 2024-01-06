# Forward propagation of neural networks

1. [Artificial Neurons](#1-artificial-neurons) <br>
    1-1. [Parametric Functions](#1-1-parametric-functions) <br>
    1-2. [Dataset 표기](#1-2-dataset-표기) <br>
    1-3. [Minibatch in Artificial Neurons](#1-3-minibatch-in-artificial-neurons) <br>

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

위 [1-3](#1-3-minibatch-in-artificial-neurons)의 minibatch 내용에서 먼저 Affine Function을 구현해보는데 tensorflow의 Dense를 통해 구현하는 것과 해당 Dense에서 사용한 $ \overrightarrow{w}, b $ 값들을 추출하여 직접 계산을 통해 구현하는 방법으로 진행

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