---
title: "2. 이항분포, 정규분포, 포아송분포, 지수분포"
date: 2020-12-15T19:47:12+09:00
hero: 
description:
menu:
  sidebar:
    name: 2. 이항분포, 정규분포, 포아송분포, 지수분포
    identifier: 2. 이항분포, 정규분포, 포아송분포, 지수분포
    parent: 통계학
    weight: 10
draft: false
---

## 이항분포란?
__이항확률분포__의 확률분포이다.
__이항확률분포란?__ n번의 베르누이 시행에서 성공의 횟수
__베르누이시행이란?__ 동전던지기처럼 결과가 두 개뿐인 실험
 ![](https://images.velog.io/images/hojp7874/post/caee1865-ccc9-4284-ac36-ee2fa9cf1375/image.png)
 - 10개의 동전을 던졌을 때 앞면이 x개 나오는 확률:
 ![](https://images.velog.io/images/hojp7874/post/afb68962-e4dc-4d0d-9e6f-c793d68fd33d/image.png)
python에서 `scipy.stats` 모듈로 구할 수 있다.
```python
from scipy import stats

# x는 입력값
stats.binom.cdf(x, n=10, p=0.5)
```

이항분포의 평균, 분산, 표준편차는 아래와 같이 구한다.
![](https://images.velog.io/images/hojp7874/post/82fd4d5c-4a7f-452c-b79a-81b418f2ec43/image.png)
python으로는,
```python
stats.binom.stats(n=3, p=0.2)

-> (array('평균'), array('분산'))
```
## 정규분포란?
![](https://images.velog.io/images/hojp7874/post/710eb78a-4833-45a0-be03-67479e907c8f/image.png)
자연에 있는 수 많은 확률을 반영하는 __확률밀도함수__
__확률밀도함수란?__ 연속확률변수의 확률분포를 나타내기 위한 함수. (그래프 아래의 면적이 확률이다.)

__표준정규분포표__
표준정규분포를 표로 만들어 놓은 것.
![](https://images.velog.io/images/hojp7874/post/f478cfb6-1851-43c4-a8a5-3882ef0e0634/image.png)
이 표를 이용하기 위해서는 __표준정규확률변수 Z__를 구해야 함.
__표준정규확률변수__
![](https://images.velog.io/images/hojp7874/post/852ee80f-6493-4f70-bfc4-34d1101963cd/image.png)
예제)
평균이 4,표준편차가 3인 확률변수에서 X가 4 이하일 확률은?
![](https://images.velog.io/images/hojp7874/post/3799aef3-0df0-4f23-9a68-7b75b644d571/image.png)
python으로 구하면,
```python
satats.norm.cdf(4, loc=4, scale=3)

-> 0.5
```
## 포아송분포란?
일정한 시간, 공간단위에서 발생하는 이벤트의 수의 확률분포
![](https://images.velog.io/images/hojp7874/post/3015736e-3570-4637-8f6c-420c96231021/image.png)

![](https://images.velog.io/images/hojp7874/post/fab24841-d9bb-4193-b4a0-59c364a115b2/image.png)

python으로 구하는 방법은,
```python
# x 이하의 확률을 말한다.
# x가 2이면 P[X<=2] 즉, P[X=0] + P[X=1] + P[X=2]를 뜻함
stats.poisson.cdf('x', mu='람다값')
```

## 지수분포란?
포아송분포에 의한 사건이 발생할 때까지 걸리는 __시간__의 확률분포
![](https://images.velog.io/images/hojp7874/post/5ad2fc97-4ed3-4bbf-ad38-35cf00c14a07/image.png)
포아송분포의 평균이 __λ__이므로, 지수분포의 평균은 __1/λ__임이 바로 드러난다.

python으로 구하는 방법은,
```python
# x시간까지 이벤트가 발생할 확률
stats.expon.cdf('x', scale=1/'람다값')

-> 0.0.7768698398515702
```