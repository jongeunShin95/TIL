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