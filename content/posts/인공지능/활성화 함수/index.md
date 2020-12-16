---
title: "활성화 함수"
date: 2020-12-15T19:57:09+09:00
hero: 
description:
menu:
  sidebar:
    name: 활성화 함수
    identifier: 활성화 함수
    parent: 인공지능
    weight: 10
draft: false
---

![](https://images.velog.io/images/hojp7874/post/c5df2d33-8ab2-483f-a24c-5c541eada090/image.png)

딥러닝 네트워크에서 각 노드들은 입력값을 받으면 위 그림처럼 특정 함수를 거친 후 다음 레이어로 전달한다.

이 때 사용하는 함수가 활성화 함수이다.

활성화 함수는 선형 함수일 시 레이어를 여러 겹 쌓는 것의 의미가 상실되기 때문에 일반적으로 비선형 함수이다.

각 함수마다 가지고있는 장 단점이 있으며, 이를 잘 선택해야 한다.

### 1. Sigmoid

![](https://images.velog.io/images/hojp7874/post/4264dbf7-0cdc-4b09-84e2-05f6b24f8a7e/image.png)

Logistic 함수라고도 불리는 이 함수는 한때 가장 많이 사용되어오던 함수이다.

멀티퍼셉트론에서 비선형의 값을 얻기 위해 사용되었다.

미분 경과가 간결하고 사용하기 쉬운 장점이 있다.

하지만 최근에는 아래의 이유로 잘 사용되지 않는다.

- w로 편미분 값과 output이 항상 양수이다.

  ![](https://images.velog.io/images/hojp7874/post/4cdaeff3-7215-464d-b410-0495970d1e66/image.png)
  미분을 하면, ∂L/∂w=∂L/∂a*x 이다. x와 미분값의 부호는 항상 동일하기 때문에 4사분면 방향으로 이동할 수 없다.

  따라서 위의 그림처럼 지그재그 형태로 이동해야 하는데, 여기서 효율이 감소하게 된다.

- Vanishing Gradient:
  이 함수의 값은 0과 1 사이이다.

  그래서 레이어가 8겹 9겹으로 점점 깊어질수록 그 값은 결국 0에 수렴하게 된다.

  따라서 최초 입력 값이 최종 결과 값에 별로 영향을 미치지 않게된다.

  이를 `Vanishing Gradient` 현상이라고 한다.

### 2. tanh 함수
![](https://images.velog.io/images/hojp7874/post/84281f62-6c68-4c50-ada9-1323c8a1a74b/image.png)

함수의 중심을 0으로 설정해 `Sigmoid` 함수의 `zigzag`문제를 보완했다.

하지만 `Vanishing Gradient`의 문제는 여전히 남아있다.


### 3. ReLU(Rectified Linear Unit) 함수
![](https://images.velog.io/images/hojp7874/post/3729f2d7-92a8-49d5-a57e-9012ef4581ef/image.png)

이 함수는 `Sigmoid`와 `tanh` 함수가 가지는 `Vanishing Gradient` 문제를 해결할 수 있다.

- sigmoid, tanh 함수와 비교시 학습이 훨씬 빠르다.
- 연산 비용이 작고, 구현이 매우 간단하다.
- 사용하기 쉽기 때문에 많이 사용된다.
- 하지만, x<0인 값들에 대해서는 기울기가 0이기 때문에 뉴런이 죽을 수 있는 `Dying ReLU` 문제가 존재한다.

### 4. Leaky ReLU

![](https://images.velog.io/images/hojp7874/post/c6dd822b-e8d3-4f81-b62a-d3387b79f303/image.png)

`Dying ReLU` 문제를 보완하기 위해 고안된 함수이다.

다른 특징은 ReLU와 동일하다.

### 5. Maxout 함수

![](https://images.velog.io/images/hojp7874/post/17d9a186-46e2-4f19-84ec-f39b8ac5493f/image.png)

여러개의 선형 함수 중 최댓값을 출력한 함수이다.

ReLU의 장점을 모두 가지고, `Dying ReLU`의 문제를 해결했다.

하지만, 계산해야 하는 양이 많고 복잡해진다는 단점이 있다.

### 6. ELU(Exponential Linear Unit) 함수

![](https://images.velog.io/images/hojp7874/post/1bf9a925-cfea-4973-ac5d-36f7d48ea33b/image.png)

ReLU의 장점을 모두 가지고, `Dying ReLU`의 문제를 해결했다.

x<0에서 지수함수를 계산하는 비용이 발생한다.