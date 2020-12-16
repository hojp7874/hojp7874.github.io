---
title: "5. 검정"
date: 2020-12-15T19:47:41+09:00
hero: 
description:
menu:
  sidebar:
    name: 5. 검정
    identifier: 5. 검정
    parent: 통계학
    weight: 10
draft: false
---

## 통계적 가설검정
__가설검정이란?__
>_만약 학생수가 1000명인 고등학교에서 30명의 랜덤하게 학생을 뽑아 키를 쟀더니 평균 172cm였다.
>그러면 전체 학생들의 키 평균이 170cm 이상이라고 할 수 있을까?_

에 대한 확률을 검정하는 것이다.
### 가설검정의 원리
모평균과 표본평균이 같을 가능성을 놓고, 특정 확률 이상이면 참이라고 한다.

![](https://images.velog.io/images/hojp7874/post/a1a63829-2616-446c-9ac5-ba7b969e0912/image.png)

__귀무가설__: __전체 학생들의 키 평균 = 선택된 학생의 키 평균__ 일 확률

__대립가설__: __전체 학생들의 키 평균 > 선택된 학생의 키 평균__ 일 확률

### 가설검정의 절차
1. __H0, H1__ 을 설정한다.

2. 유의수준 __α__ (확률이 낮다는 기준점)를 설정한다.

3. __검정통계량__ 을 계산한다.

4. __기각역__ 또는 __임계값__ 계산
	
5. __기각역이란?__

  ![](https://images.velog.io/images/hojp7874/post/b04fb776-bbca-49c2-82d3-894688d07f5e/image.png)

6. 주어진 데이터로부터 __유의성 판정__

  ![](https://images.velog.io/images/hojp7874/post/7cfb5da5-3b51-4b3a-80c5-ac672942b44e/image.png)
## 모평균의 검정
### 대립가설
> _어떤 농장에서 계란의 평균 무게는 10.5g이었다.
> 새로운 사료를 도입하고 생산된 계란 30개의 평균 무게는 11.4g이다.
> 새로운 사료가 평균적으로 더 무거운 계란을 생산했다고 할 수 있는가?_

![](https://images.velog.io/images/hojp7874/post/fbe40528-90bc-4c76-885a-3320d479c7d4/image.png)

### 검정통계량
모집단이 정규 모집단이거나, n >= 30이라면 정규 분포를 이용할 수 있다.

![](https://images.velog.io/images/hojp7874/post/4e47fd69-be09-4d7b-9fbe-01acc1eabcac/image.png)

이외의 경우도 구할 수는 있지만 어려워서 패쓰!

### 기각역
만약 가설이 기각역 범주 내에 있다면, 귀무가설은 기각할 수 있다.

![](https://images.velog.io/images/hojp7874/post/db14569e-99cd-4fb8-80ba-6e54c5d53b2f/image.png)