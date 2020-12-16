---
title: "6. 교차 엔트로피"
date: 2020-12-15T19:47:54+09:00
hero: 
description:
menu:
  sidebar:
    name: 6. 교차 엔트로피
    identifier: 6. 교차 엔트로피
    parent: 통계학
    weight: 10
draft: false
---

## 엔트로피
### 자기정보
확률이 낮은 사건일수록 값어치있는 정보(내포하고 있는 정보가 많다)라는 의미이다.

![](https://images.velog.io/images/hojp7874/post/cc634476-5527-43f6-ae67-2bb52845f28c/image.png)

여기서 정보의 단위 __b__ 는



![](https://images.velog.io/images/hojp7874/post/6435ca9f-396c-4aab-b0de-7606b7dc843f/image.png)

등이 있고, 일반적으로 __2__ 를 많이 사용한다.



자기정보는 아래의 특성이 있다.



![](https://images.velog.io/images/hojp7874/post/c53a4662-c5da-4754-88de-0f8833ab7c18/image.png)

### 엔트로피란?
자기정보의 평균이다.

![](https://images.velog.io/images/hojp7874/post/37ee7a84-51e0-4af6-9cb1-32f8f425a752/image.png)



K개의 사건이 일어났을 때 엔트로피는 아래와 같은 특성이 있다.



![](https://images.velog.io/images/hojp7874/post/301cc275-64ac-4888-9933-72cdaec451bb/image.png)



__엔트로피는 다음과 같이 활용할 수 있다.__

![](https://images.velog.io/images/hojp7874/post/a23943b2-ec25-4bad-9df4-1f6580c5158a/image.png)

"ABCD"라는 데이터를 전송할 때,

일반적으로 전송하면 평균 __2bit__가 필요하다.

엔트로피를 이용해 압축하면, 평균 __ΣP(X)i(X) = 7/4bit__가 필요하다.



## 교차 엔트로피
### 교차엔트로피란?
__P__ 에 대한 확률분포 __Q__ 가 있을 때, __Q__ 가 __P__ 와 얼마나 비슷한지를 나타내는 의미가 있다.

위의 엔트로피 활용의 예 에서 __P__ 의 확률분포 __Q__ 가 있을 때, __Q__ 의 엔트로피를 구하면,

![](https://images.velog.io/images/hojp7874/post/6fd23d81-b6ab-407a-8b50-c14f54ebe03a/image.png)

__P__ 일때가 최적해이므로, __H(P, Q) >= H(P)__ 이다.

__Q__ 가 __P__ 와 비슷할수록 __H(P, Q)__ 는 __H(P)__ 에 가까워진다.

### 손실함수
__교차엔트로피__ 는 기계학습에서 __손실함수__ 로 많이 사용된다.
특히, __분류문제__ 에서의 손실함수로 사용되는데

> _분류문제란?_
- _주어진 대상이 A인지 아닌지를 판단하는 문제_
- _주어진 대상이 A B C 중 무엇인지 판단하는 문제_

기계학습에서는 주어진 대상이 각 그룹에 속할 확률을 제공한다.

예: __[0.8, 0.2]__ A일확률 80%, 아닐확률 20%

이 값이 정답인 __[1.0, 0.0]__ 과 얼마나 다른지 측정이 필요하다.

손실함수에는 여러 종류가 있는데, 그 중 대표적인 제곱합과 비교를 하면,

| 제곱합                                                       | 교차엔트로피                                                 |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ![](https://images.velog.io/images/hojp7874/post/5c185335-f397-4426-affb-58e8e8742540/image.png) | ![](https://images.velog.io/images/hojp7874/post/81b305ac-b5b6-418c-8918-2caa215bb240/image.png) |
| 확률이 다를수록 커짐                                         | 확률이 다를수록 커짐                                         |
|학습속도 느림|학습속도 빠름
||분류문제에서 주로 이용|

#### 교차 엔트로피를 사용해서 손실함수를 구해보자.



__p = [1, 0]__이 정답일 때,

![](https://images.velog.io/images/hojp7874/post/d4aab01d-43c9-4f94-a6e5-c181ab0b7c8e/image.png)



- __[0.8, 0.2]__ 일때 0.3219
- __[0.5, 0.5]__ 일때 1
- __[0.2, 0.8]__ 일때 2.32

이렇게 정답과 다를수록 손실계수가 커진다.

이를 python으로 계산하면,
```python
import numpy as np

def crossentropy(P, Q):
  return sum([-P[i]*np.log2(Q[i]) for i in range(len(P))])

[crossentropy([1, 0], [1-i/10, i/10]) for i in range(1, 10)]

->
[0.15200309344504997,
 0.3219280948873623,
 0.5145731728297583,
 0.7369655941662062,
 1.0,
 1.3219280948873622,
 1.7369655941662059,
 2.3219280948873626,
 3.3219280948873626]
```
이렇게 구할 수 있다.