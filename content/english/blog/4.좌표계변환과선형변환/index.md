---
title: "4. 좌표계변환과 선형변환"
date: 2020-12-15T19:43:25+09:00
author: Jinpyo Hong
image : "images/blog/default.jpg"
bg_image: "images/laptop-coffee.jpg"
categories: ["None"]
tags: ["None"]
description: 
draft: false
type: "post"
---

## 좌표계변환이란?
선형시스템 __Ax__=__b__ 가 있을 때, 행렬 __A__ 를 좌표계로 보고 벡터 __x__ 와 __b__ 를 좌표값으로 보는 방법입니다.

예를들어,

![](https://images.velog.io/images/hojp7874/post/4b9d1c67-8ee0-4153-90a5-2d9955a126fc/image.png)

위와 같은 식에서 __b__ 는 앞에 항등행렬을 곱한것과 같습니다.

![](https://images.velog.io/images/hojp7874/post/07b3d015-49d6-4ee8-a23a-3fcf6f6a1788/image.png)

그리고 항등행렬을 좌표계로 보면, xy좌표계와 같습니다. 따라서

![](https://images.velog.io/images/hojp7874/post/96d0f305-e77c-44ec-9781-e2afc223263c/image.png)

이런 벡터모양을 가지게 됩니다.

이번에는 __Ax__ 에서 __A__ 를 좌표계로 보면,

![](https://images.velog.io/images/hojp7874/post/53c38a34-23d2-49be-a3ed-c4eeeb93409f/image.png)

위의 그림의 초록색 좌표계로 볼 수 있습니다.

행렬을 열벡터로 만들면, [1 2]와 [-1 2]가 나타나는데,

이 벡터들을 축으로 좌표계를 만드는 겁니다.

그러면 위 그림의 빨간 벡터는 (2, 1)의 좌표값을 가지게 됩니다.

## 선형변환이란?
어떤 벡터를 다른 벡터로 변환해주는 함수를 변환이라고 합니다.

이때, 입력값과 출력값이 모두 선형인 기준을 만족시키면, 이 함수(변환)을 선형변환이라고 합니다.

어딘가 익숙하지않나요?

네 맞아요. 행렬은 선형변환이랍니다.

예를들어 2차원 벡터를 입력으로 받아서 반시계만큼 θ만큼 회전하는 행렬을 만들어봅시다.

![](https://images.velog.io/images/hojp7874/post/2d2040f6-dc4b-433e-8500-70fdb99ae345/image.png)

만드는 법은 간단합니다. (1, 0)과 (0, 1) 벡터를 행렬로 만들고,

![](https://images.velog.io/images/hojp7874/post/905186cd-ec06-47dd-a21e-a3b11a902a2b/image.png)

그리고 변환한 후의 좌표로 갈아끼웁니다.

![](https://images.velog.io/images/hojp7874/post/b71d6b5f-232d-4b17-8dc8-e6a4c8d40142/image.png)

완성됐습니다.