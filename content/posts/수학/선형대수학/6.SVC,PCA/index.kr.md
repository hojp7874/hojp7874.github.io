---
title: "6. SVC, PCA"
date: 2020-12-15T19:45:47+09:00
hero: 
description:
menu:
  sidebar:
    name: 6. SVC, PCA
    identifier: 6. SVC, PCA
    parent: 선형대수학
    weight: 10
draft: false
---

## 특이값 분해(SVC, Singular Value Decomposition)
![](https://images.velog.io/images/hojp7874/post/5967da75-8b6f-4420-b29b-314774d1a751/image.png)
- U: m × m 회전행렬 (정규직교행렬)
- D: m × n 확대축소 (확대축소 크기에 따른 정렬 형태)
- V: n × n 회전행렬 (정규직교행렬)

### SVD의 과정
![](https://images.velog.io/images/hojp7874/post/d6cb019c-b9a2-42c5-ad79-32c74c9cc59c/image.png)
벡터__x__에  __A__를 연산하는 과정을 특이값분해를 통해 분석해봅시다.
연산은 __V -> D -> U__의 순서로 진행됩니다.
![](https://images.velog.io/images/hojp7874/post/a5fd47a1-7936-498f-8417-c49d6997a5c0/image.png)
1. __V__는 시계방향으로 45도 회전하는 함수이다.
2. __D__는 x방향으로 4배, y방향으로 1/sqrt(2)배만큼 확대축소하는 함수이다.
3. __U__는 y축을 기준으로 45도 회전하는 함수이다.

### SVD의 근사
만약 SVD 계산을 단순화 하고싶다면 근사치를 계산할수도 있습니다.
바로 __D__에서 작게 증폭되는 값을 버리는 방법으로요!
![](https://images.velog.io/images/hojp7874/post/d6cb019c-b9a2-42c5-ad79-32c74c9cc59c/image.png)
위 식에서 __D__에 4만 남겨두게 되면, __U__는 1열만 필요하고, __V__는 1행만 필요합니다.
![](https://images.velog.io/images/hojp7874/post/99b33ceb-c4ad-445b-b64a-d105e54411e3/image.png)
결과적으로 위와같은 식이 되어 계산이 매우 편해집니다!
이 연산의 기하학적 의미를 알아보면,
![](https://images.velog.io/images/hojp7874/post/85d51442-d969-447c-b2f3-f74738af8b91/image.png)
1. y축으로 증폭된 값을 버리고, x축으로의 수선의 발(곱셈 연산)을 가지고 갑니다.
2. x축으로만 4배 증폭합니다.
3. y축을 기준으로 회전하는것처럼 보이지만, 사실 곱셈연산

## 주성분분석(PCA, principal Component Analysis)
데이터의 __공분산행렬__을 찾기위한 직교분해입니다.
참고로, 데이터의 중심 __m__ 과 공분산행렬 __C__는 아래와 같이 구합니다.
![](https://images.velog.io/images/hojp7874/post/415cba49-c6d2-4cc6-a007-01fc336491cf/image.png)
PCA는 아래와 같이 나타냅니다.
![](https://images.velog.io/images/hojp7874/post/9f0157b5-834c-4e66-b14f-2c6f654c12c9/image.png)
여기서 각각의 의미를 살펴보면,
- __W__의 각 열벡터들은 분포들의 주성분 방향과 같습니다.
- __D__의 각 요소들은, 그 주성분의 크기와 같습니다.
- 여기서 __주성분__은 아래의 그림에 보이는 화살표입니다.
![](https://images.velog.io/images/hojp7874/post/2dd135e3-36fa-4b77-a250-92d28f22b2e7/image.png)
주성분은 원래 아래의 좌측 값입니다.
이 값을 구하는 방법은 모든 행렬값을 더해서 그 갯수로 나눈 값입니다.
그런데 PCA분석을 이용해서 주성분을 분석할 수 있습니다.
![](https://images.velog.io/images/hojp7874/post/8e7c5ba2-328c-4d39-9741-6de4befafd9d/image.png)
<주성분 구하는법>
![](https://images.velog.io/images/hojp7874/post/56e1d522-fff8-4a6d-a610-fd5ea0b57649/image.png)