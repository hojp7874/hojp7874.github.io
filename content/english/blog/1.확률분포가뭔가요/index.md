---
title: "1. 확률분포가 뭔가요?"
date: 2020-12-15T19:46:55+09:00
author: Jinpyo Hong
image : "images/blog/default.jpg"
bg_image: "images/laptop-coffee.jpg"
categories: ["None"]
tags: ["None"]
description: 
draft: false
type: "post"
---

​	

## 확률변수가 뭔가요?

동전 10개를 던지는 실험에서 동전 앞면이 나온 수를 뜻한다.

보통 **X**나 **Y**같은 대문자로 표시한다.

다음과 같이 분류한다.

- **이산확률변수**: 셀 수 있는 경우 (동전 10개 던질 때 앞면이 나온 수, 4개)
- **연속확률변수**: 셀 수 없는 경우 (어느 학교에서의 남학생의 수, 171.23cm)


​	

## 확률분포가 뭔가요?

확률변수가 가질 수 있는 값에 대해 확률을 대응시켜주는 관계이다.
> ex)주사위 2개를 던지는 실험에서 특정 수가 나올 확률
> P(X = 2) = 1/36
> P(X = 3) = 1/18
> ...
> P(X = 12) = 1/36

확률분포를 표시하는 방법: 표, 그래프, 함수 등...

확률 변수 X도 평균과 분산을 가진다. 모집단의 평균, 분산과 같다.

​	

## 확률변수의 병균과 분산을 구하는 법:
#### 평균:

![](https://images.velog.io/images/hojp7874/post/6ef66605-6213-40d9-9b6e-03e914ea3daa/image.png)

#### 분산 (표준편차의 제곱):

![](https://images.velog.io/images/hojp7874/post/333a862d-2310-4aa5-9cdd-914e6b1ec0bc/image.png)

**분산의 간편식:**

![](https://images.velog.io/images/hojp7874/post/3f717be7-d0b5-4cf4-b043-a7b2ed0c67a7/image.png)

​	

## 공분산이란?

확률변수간의 상관관계성을 나타내는 지표이다.

확률변수 X와 Y의 공분산 **Cov(X, Y)** 에서 공분산이 0에 가까우면 두 확률변수 X와 Y는 독립적이라는 것을 의미한다.

![](https://images.velog.io/images/hojp7874/post/c13abf54-5b93-4f1a-aa1d-897d9d6ea4f2/image.png)

​	

#### 상관계수
공분산은 각 확률변수의 절대적인 크기에 영향을 받으므로, 단위에 의한 영향을 없앨 필요가 있다.
그래서 나온것이 상관계수!

![](https://images.velog.io/images/hojp7874/post/1ab42e94-570f-4eaa-bb28-1b73461d406d/image.png)

​	

공분산을 확률변수들의 분산으로 나눈 값이다.