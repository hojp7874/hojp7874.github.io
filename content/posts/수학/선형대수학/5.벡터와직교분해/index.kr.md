---
title: "5. 벡터와 직교분해"
date: 2020-12-15T19:45:37+09:00
hero: 
description:
menu:
  sidebar:
    name: 5. 벡터와 직교분해
    identifier: 5. 벡터와 직교분해
    parent: 선형대수학
    weight: 10
draft: false
---

## 투영(projection)
![](https://images.velog.io/images/hojp7874/post/ac7f278c-4291-4934-8af8-c35baf3a799f/image.png)
벡터 __a__에 벡터 __u__를 투영시키면, 벡터 __w1__이 됩니다.
![](https://images.velog.io/images/hojp7874/post/2c393838-84bf-4575-9b6c-831d02dc31b7/image.png)
__w2__는 __u__를 __a__에 투영하고 남은 벡터라고 하여 __보완벡터(complement vector)__라고 합니다.

## 직교행렬
__직교행렬__은 행렬의 열벡터들이 서로 직각을 이루고 있는 행렬을 말합니다.
![](https://images.velog.io/images/hojp7874/post/adeb673c-2e3d-4d7d-8915-cb6734624378/image.png)

여기에 각 열벡터들의 길이가 1일 때, __정규직교행렬__이라고 말합니다.
![](https://images.velog.io/images/hojp7874/post/8bb3fe18-76b5-4ceb-a003-0d3bfa6a86f7/image.png)

## 어떤 장점이 있나요?
### 직교행렬의 장점.
만약 행렬 __A__가 직교행렬이라면 __Ax__ = __b__의 해를 구할 때 역행렬을 구할 필요가 없습니다.
앞서 말했던 __투영__을 이용할 수 있기 때문이죠.
![](https://images.velog.io/images/hojp7874/post/9e1bb20e-0a3a-4265-9457-6903a3c18b91/image.png)
위와같은 선형시스템을 풀어봅시다.
![](https://images.velog.io/images/hojp7874/post/72c44ef6-fd89-4b77-b33b-6ed8b475c80d/image.png)

![](https://images.velog.io/images/hojp7874/post/22db8c15-9a0b-4aa2-bead-74cc11b2f91b/image.png)
알아야 하는 것은 위 그린에서의 빨간 화살표와 주황 화살표의 갯수(실수) 입니다.
빨간 화상표의 갯수를 구하면, __u__를 투영한 값이 몇__a__인지 알면 됩니다.
![](https://images.velog.io/images/hojp7874/post/2c393838-84bf-4575-9b6c-831d02dc31b7/image.png)
위 식에 대입하면, projau = (24-4/16+4)__a__ == 1__a__

같은 방법으로 주황 화살표의 갯수를 구하면,
projbu = (6+4/1+4)__b__ = 2__b__
따라서, x1은 2, x2는 1으로 해를 구할 수 있습니다.

또한 이 과정은 x1을 구하는 과정과 x2를 구하는 과정이 서로 독립적이라는 장점이 있습니다.
그래서 x의 계산이 병렬처리가 가능하여 빠른 계산이 가능합니다.

### 정규직교행렬의 장점
직교행렬의 장점을 모두 가지고있고, 추가적으로 계산이 더 편하다는 장점이 있습니다.
![](https://images.velog.io/images/hojp7874/post/2c393838-84bf-4575-9b6c-831d02dc31b7/image.png)
이 식에서 분모의 ||a||가 1이므로 __u·a__로 계산할 수 있습니다.

## 활용(QR분해)
직교행렬은 굉장히 많은 장점을 가지고 있습니다. 그러나 우리에게 주어지는 __A__는 직교행렬이 아닐 가능성이 크죠...
그래서 __A__를 직교행렬로 바꿔주는 작업(__QR분해__)을 합니다!
![](https://images.velog.io/images/hojp7874/post/83ee6ac0-36c2-43ed-9f87-7394ca6f442a/image.png)
__A__를 직교행렬인 __Q__로 바꾸고 남은 값들은 __R__행렬로 만들어집니다.
신기하게도 __R__행렬은 상삼각행렬로 만들어지기 때문에 후방대치법을 사용해 쉽게 값을 구할 수 있겠네요.
__QR__분해를 하는 과정은 그람-슈미트 과정이라는걸 한다는데... 나중에 공부해봐야겠네요.

### vs LU분해
- LU분해: 선형시스템을 풀 때 병렬처리를 할 수 없습니다.
- QR분해: __Q__행렬이 꽉차있는 형태이기 때문에 메모리 사용량이 많습니다.