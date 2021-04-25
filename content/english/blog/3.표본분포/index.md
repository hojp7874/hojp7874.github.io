---
title: "3. 표본 분포"
date: 2020-12-15T19:47:23+09:00
author: Jinpyo Hong
image : "images/blog/default.jpg"
bg_image: "images/laptop-coffee.jpg"
categories: ["None"]
tags: ["None"]
description: 
draft: false
type: "post"
---

## 표본 분포란?
__통계량__ 의 확률분포이다.

__통계량:__ 표본평균, 표본분산 등

- __모집단이 정규분포__ 를 이룰 때 표본평균은 아래와 같이 구한다. (표본 분포도 정규분포이다.)

  ![](https://images.velog.io/images/hojp7874/post/1ed28cbd-f91c-4c17-b31e-b56b8409cf1f/image.png)


- python으로 구하는 법은

  ![](https://images.velog.io/images/hojp7874/post/9aca14cb-0180-4cac-a0e4-c294abf6eb48/image.png)
```python
import numpy as np
import matplotlib as plt
import random

# 평균 10, 분산 3, n의 갯수 10
xbars = [np.mean(np.random.normal(loc=10, scale=3, size=10)) for i in range(10000)]
h=plt.pyplot.hist(xbars, range=(5,15), bins=30)
```
![](https://images.velog.io/images/hojp7874/post/11804df7-36cb-4d81-9f02-c3a3134941c9/image.png)

## 중심극한정리
모집단이 __표본분포가 아닐 때__ 사용할 수 있는 표본 분포
- 표본 평균은 아래와 같이 구한다.

  ![](https://images.velog.io/images/hojp7874/post/cec84e59-3d61-4eae-b42b-7a10a021390f/image.png)

  n이 충분히 커지면(30개 이상), __정규분포__를 따른다.

### python으로 구하는 법
__0 ~ 10에서 n개 뽑아 평균을 낸 것의 분포__
```python
import random
import numpy as np
import matplotlib as plt

xbars = [np.mean(np.random.rand(n) * 10) for i in range(10000)]
h=plt.pyplot.hist(xbars, range=(0,10), bins=100)
```
- n=1

  ![](https://images.velog.io/images/hojp7874/post/0a8fdd78-6cfe-41c8-8390-b58bb71ea968/image.png)

- n=2

  ![](https://images.velog.io/images/hojp7874/post/3ede61bf-4a6a-4d19-af13-498c64f93a97/image.png)

- n=3

  ![](https://images.velog.io/images/hojp7874/post/bb8c6fc2-ae47-4b3d-bb7c-767f229bbffb/image.png)

- n=30

  ![](https://images.velog.io/images/hojp7874/post/ffadf99f-8537-49e9-bff3-afea62c4d6c9/image.png)

  n이 커질수록 정규분포와 가까워진다.

__지수분포__ 에서 똑같이 구해보면,
```python
import random
import numpy as np
import matplotlib as plt
n=1
xbars = [np.mean(np.random.exponential(scale=3, size=n)) for i in range(10000)]
print(np.mean(xbars), np.var(xbars))
h=plt.pyplot.hist(xbars, range=(0,10), bins=100)
```
- n=1

  ![](https://images.velog.io/images/hojp7874/post/8ede7ea7-caea-42aa-bf82-9281ede4eb0e/image.png)

- n=3

  ![](https://images.velog.io/images/hojp7874/post/5b29f0ea-8aaf-4c7e-ae5f-2b8aa2dde383/image.png)

- n=30

  ![](https://images.velog.io/images/hojp7874/post/a6e391f4-0684-4bc4-a3e0-838bc143efd3/image.png)

  n이 커질수록 정규분포와 가까워진다.

