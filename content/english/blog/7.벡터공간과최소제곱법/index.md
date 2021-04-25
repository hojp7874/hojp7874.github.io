---
title: "7. 벡터공간과 최소제곱법"
date: 2020-12-15T19:45:59+09:00
author: Jinpyo Hong
image : "images/blog/default.jpg"
bg_image: "images/laptop-coffee.jpg"
categories: ["None"]
tags: ["None"]
description: 
draft: false
type: "post"
---

## 벡터공간이란?
어떤 공간 안에서 아무리 벡터들을 더하거나 스칼라곱을 해도 벗어날 수 없는 공간을 말합니다.

![](https://images.velog.io/images/hojp7874/post/03f15684-8a66-440e-b219-5236a18c2208/image.png)

### 열공간의 의미
행렬 __A__ 의 모든 열벡터로 만들어진 벡터공간을 열벡터라 합니다.

열벡터들로 가능한 모든 선형조합을 집합으로 모으면 구할 수 있습니다.

![](https://images.velog.io/images/hojp7874/post/33833ca8-007f-4731-9869-ecc00987a4b2/image.png)

만약 선형시스템 __Ax__ = __b__ 가 해가 없으면, 아래와 같은 형국이 만들어집니다.

![](https://images.velog.io/images/hojp7874/post/1aac8a7e-938b-42bc-b6ac-36c52215e593/image.png)

이렇게 선형시스템이 해가 있는지 없는지를 구할 수 있습니다.



## 최소제곱법이란?
만약 __Ax__ = __b__ 가 해가 없다고 해도 가까운 해라도 구하고싶다!! 라고 할 때 사용할 수 있는 방법입니다.

![](https://images.velog.io/images/hojp7874/post/12cd37bd-974a-4cbe-82ef-41ce961f1717/image.png)

![](https://images.velog.io/images/hojp7874/post/bdc90fbd-4dcd-4f43-9f0b-0c7e76a1d9e0/image.png)

원래목표 __b__ 와 새로운목표 __b'__ 의 차이 __(b-b')__ 의 __제곱__ 을 최소화하는 의미라서 이름이 __최소제곱법__ 입니다.



### 구하는 방법


__Ax__ = __b__ 의 양변에 __AT__ 를 곱하면 됩니다.

![](https://images.velog.io/images/hojp7874/post/64ef9082-7c71-4c2f-97ed-67f5546affb6/image.png)

### 활용
__선형회귀__ 문제에 응용할 수 있습니다.

![](https://images.velog.io/images/hojp7874/post/00720b8c-9e83-4655-8cbd-397b0f3e900a/image.png)

위의 예시에서는 네 지점을 모두 지나는 직선은 없습니다.

아래는 __y = mx + b__ 에 (x, y)를 대입했을 때의 값들을 행렬식으로 만든 것입니다.

ex) (-3)m + (1)b = (-1)

![](https://images.velog.io/images/hojp7874/post/9165f5b2-1a02-488d-ba70-bbe0ac88364d/image.png)

그래서 최소제곱법을 사용하면,

![](https://images.velog.io/images/hojp7874/post/953d18b4-3195-4944-b886-8d0117c36645/image.png)

![](https://images.velog.io/images/hojp7874/post/b5bd0d44-3396-4993-8fb5-606405298bc4/image.png)

m = 4/5, b = 1 이므로

y = 4/5x + 1의 직선이 그려집니다.