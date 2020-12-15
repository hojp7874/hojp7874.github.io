---
title: "4. 추정"
date: 2020-12-15T19:47:33+09:00
hero: 
description:
menu:
  sidebar:
    name: 4. 추정
    identifier: 4. 추정
    parent: 통계학
    weight: 10
draft: false
---

## 모평균의 추정
### 점추정이란?
특정 지점에서의 평균 추정값이다.
표본평균이 점 추정값이 된다.
아래와 같이 간단하게 구할 수 있다.
```python
import numpy as np
samples = [9, 4, 0, 8, 1, 3, 7, 8, 4, 2]
print(np.mean(samples))
```
### 구간추정
특정 %만큼 신뢰할 수 있는 구간을 파악할 때 활용한다.

다음과 같이 모평균의 __100(1 - a)% 신뢰구간__을 구할 수 있다.
![](https://images.velog.io/images/hojp7874/post/c1df2f8d-e06c-4c53-ba20-3f8b4d4d9e34/image.png)
n이 충분히 큰 중심극한분포에서는 다음과 같이 구할 수 있다.
![](https://images.velog.io/images/hojp7874/post/a3fc9798-fb86-437a-9e31-e00ec782676c/image.png)

예제를 풀어보자
- __x=173.6, s=3.6, n=36__인 분포에서 __95% 신뢰구간__을 찾아라.
![](https://images.velog.io/images/hojp7874/post/101885a3-96da-4f71-9906-95416995aa95/image.png)

## 모비율의 추정
### 점추정
__모비율 p의 점추정량__은 아래와 같이 간단하게 구할 수 있다.
__확률변수 X__: n개의 표본에서 특정 속성을 갖는 표본의 개수
![](https://images.velog.io/images/hojp7874/post/377e4e16-7ba8-4769-a843-6e92817c62da/image.png)

### 구간추정
__n이 충분히 클 때__ 사용할 수 있다.
![](https://images.velog.io/images/hojp7874/post/86ae693e-33c4-4644-89b3-7cd923b2c1d5/image.png)
이 때, 확률변수X는 __표준정규분포__와 근사하다.
![](https://images.velog.io/images/hojp7874/post/65846c4e-bdaa-4ddf-8e1d-6db30acb63d4/image.png)
__1 - α__는 표준정규분포에서 양 옆 α/2를 잘라낸 부분과 같다.
![](https://images.velog.io/images/hojp7874/post/fe04d905-9d4d-4ec5-b2d3-b382caa77fa0/image.png)
이를 __Z__를 없애 새롭게 정리하면,
![](https://images.velog.io/images/hojp7874/post/be252219-6c56-4d08-a639-cc9c3a1b0014/image.png)
따라서 __100(1 - α)% 신뢰구간__은 다음과 같다.
![](https://images.velog.io/images/hojp7874/post/971117ea-4d4b-4273-9417-f18f936d834a/image.png)

예제를 풀어보자
- __x=48, n=150__인 분포에서 __95% 신뢰구간__을 찾아라.
![](https://images.velog.io/images/hojp7874/post/85ccd35b-c9ae-4e31-99c2-26a41ed14cdc/image.png)