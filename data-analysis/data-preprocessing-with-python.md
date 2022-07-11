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
    df = pd.DataFrame({'한굴': ['가', '나', '다'], '영어': ['a', 'b', 'c'], '숫자': [1, 2, 3]})
    # list를 통한 생성
    df = pd.DataFrame([['가', '나', '다'], ['a', 'b', 'c'], [1, 2, 3]])
```
<p align="center">
    <img src="https://user-images.githubusercontent.com/20867824/178308879-71bb8385-c85f-491b-80ab-89cd26281a0d.png" width="300" height="250" />
    dictionary 를 통한 생성
</p>

<br>

#### Nan 을 포함한 데이터 프레임 생성

```python
    import pandas as pd
    import numpy as np

    df = pd.DataFrame({'한굴': ['가', '나', '다'], '영어': ['a', np.NaN, 'c'], '숫자': [1, 2, 3]})
```
<p align="center">
    <img src="https://user-images.githubusercontent.com/20867824/178311094-e3b49d17-947f-4ccc-a4a0-fb41debf7b76.png" width="300" height="250" />
    Nan 포함
</p>


