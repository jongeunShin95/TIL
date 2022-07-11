# Data Preprocessing With Python (작성중)

> 실제 데이터를 통한 전처리가 아닌 그냥 pandas 사용법을 익히는 정도
>
> 기본적인 pandas 공부 후 실제 데이터를 통한 전처리 작업 해보기

<br>

## 1. 데이터 프레임 생성

#### dict, list 를 통한 생성

```python
    import pandas as pd

    # dictionary를 통한 생성
    df = pd.DataFrame({'한글': ['가', '나', '다'], '영어': ['a', 'b', 'c'], '숫자': [1, 2, 3]})
    # list를 통한 생성
    df = pd.DataFrame([['가', '나', '다'], ['a', 'b', 'c'], [1, 2, 3]])
```
<p align="center">
    <img width="300" alt="dictionary를 통한 생성" src="https://user-images.githubusercontent.com/20867824/178313683-1284924c-3f28-4ea7-94ea-2a358a337885.png">
    <p align="center">dictionary를 통한 생성</p>
</p>

<br>

#### Nan 을 포함한 데이터 프레임 생성

```python
    import pandas as pd
    import numpy as np

    df = pd.DataFrame({'한굴': ['가', '나', '다'], '영어': ['a', np.NaN, 'c'], '숫자': [1, 2, 3]})
```
<p align="center">
    <img width="300" alt="Nan 포함" src="https://user-images.githubusercontent.com/20867824/178314406-76255765-794c-4ff2-815d-f929bbe9a05b.png">
    <p align="center">Nan 포함</p>
</p>


