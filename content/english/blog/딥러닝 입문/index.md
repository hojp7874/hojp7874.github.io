---
title: "딥러닝 입문"
date: 2020-12-15T19:56:53+09:00
author: Jinpyo Hong
image : "images/blog/default.jpg"
bg_image: "images/laptop-coffee.jpg"
categories: ["None"]
tags: ["None"]
description: 
draft: false
type: "post"
---

# Deep Learning이란?

`AI`는 인간의 일을 대신 할 수 있는 모든 기계를 말한다. ex) 전기밥솥, 세탁기 등

기계가 일을 할 수 있도록 하는 방법에는 크게 두 가지가 있다.
- 인간이 직접 알고리즘을 구현하여  입력하는 방법
- 수많은 데이터를 입력하여 알고리즘을 찾아내는 방법

![](https://images.velog.io/images/hojp7874/post/1dd05ec9-455b-4f84-8057-f5747bd42bee/image.png)

두 번째 방법이 바로 `Machine Learning`이다.

## Machine Learning

Machin Learning을 학습법을 기준으로 분류하면, `지도학습`과 `비지도학습`, `강화학습`으로 구분된다.

![](https://images.velog.io/images/hojp7874/post/a5edf82a-efd5-4ad4-9d17-074e8603d020/image.png)

### 지도학습

지도학습은 Data와 Answers를 모두 입력하여 사용한다.

예를들어, 강아지와 고양이를 분류하는 방법을 학습시킬때 모든 data에는 강아지 또는 고양이라는 정답을 함께 입력할 수 있다.

### 비지도학습

비지도학습은 Data만 입력하고 Answers는 입력하지 않는다.

예를들어, 외계인이 강아지와 고양이의 data를 얻었는데 둘의 차이점을 모른다고 가정하자. 이때 인공지능은 둘의 차이를 스스로 기준을 세워 분류할 수 있다.

하지만, 강아지와 고양이를 색깔이나 크기를 기준으로 잘못 분류할 수도 있다. 이럴땐 사용자가 어느정도의 전처리를 하는게 도움이 된다.

![](https://images.velog.io/images/hojp7874/post/6558a605-731e-472f-94c9-f4f79aad75e7/image.png)

### 강화학습

강화학습은 보상이 최대가 되는 방법으로 학습하는 방법이다.

위의 그림은 강화학습을 설명하는데, Agent가 어떤 action을 하면, 환경이 반응을 하여 state를 변화시켜 reward를 준다. 이때 Agent는 reward가 최대가 되는 action을 취하도록 스스로 값을 조정하는것이 강화학습이다.

주어지는 data가 없이도 학습을 할 수 있다는 것이 가장 큰 장점이다. 기본적으로 data는 비싸고, 또 잘못된 data로 오류를 학습할 일도 없기 때문에 정교한 학습이 가능하다.



`Deep Learning`을 흔히 강화학습에 속한다고 생각하지만 그렇지 않다. Deep Learning을 분류하기 위해서는 Machine Learning을 구현방법으로 분류해야 한다.

Machine Learning을 구현방법을 기준으로 분류하면, 아래와 같이 분류가 가능하다.
- Linear Rehression
- Regression Trees
- SVM(Support Vector Machine)
- Decision Tree
- Neural Network
- ...

## Deep Learning

Deep learning은 `Neural Network`의 일부이다.

### Neural Network

Neural Network란 인간의 뇌 신경망을 모방한 방법으로, 각 신경망은 외부의 입력을 받아 계산을 하여 값을 출력한다.

![](https://images.velog.io/images/hojp7874/post/867e6692-090e-467b-9eef-204770a5a3d8/image.png)

위 그림에서 x는 입력값의 양이고, w는 비중이다.

만약 덜 익은 귤을 분류하는 인공지능을 학습시킬때, x1은 크기, x2는 색깔로 설정할 수 있다.

그리고 w1는 크기가 덜 익은 정도에 미치는 영향, w2는 색깔이 덜 익은 정도에 미치는 영향이고 둘 다 변수로 둔다.

이제 학습을 시켜보자. 처음에 입력된 w1, w2로 1차 그래프를 만들 수 있다. (w1\*x1 + w2\*x2 = f)

![](https://images.velog.io/images/hojp7874/post/0d1d0c3b-cdb2-4ea4-86e0-9f9b1c31efdd/image.png)

이 그래프는 귤을 제대로 나누지 못했다. 잘못 분류된 귤들의 갯수는 오차가 되고, 오차를 줄이는 방향으로 w를 조정하여 다시 테스트한다.

![](https://images.velog.io/images/hojp7874/post/1afcf2c3-3b20-4b01-891f-df8b3106dd86/image.png)

테스트를 반복하다보면 어느새 적절한 w1과 w2값을 찾게 된다.

![](https://images.velog.io/images/hojp7874/post/e33d54ea-f344-4763-83d9-76978df7f508/image.png)

> [위의 테스트가 가능한 링크입니다.](https://playground.tensorflow.org/#activation=linear&regularization=L1&batchSize=1&dataset=gauss&regDataset=reg-plane&learningRate=0.3&regularizationRate=0&noise=25&networkShape=1,2&seed=0.70492&showTestData=false&discretize=true&percTrainData=10&x=true&y=true&xTimesY=false&xSquared=false&ySquared=false&cosX=false&sinX=false&cosY=false&sinY=false&collectStats=false&problem=classification&initZero=false&hideText=false)


이렇게 출력된 값은 다른 신경망의 입력값이 되어 마치 인간의 뇌와 같이 연쇄적으로 처리를 한다.

![](https://images.velog.io/images/hojp7874/post/79ef7c14-8c80-41dc-b742-dfc8cec706a2/image.png)

input과 output 사이에 있는 인공신경망 층을 hidden layer라고 하는데, 이 층의 갯수가 늘어날수록, 인공지능은 더 정교한 학습이 가능하고, 더 복잡해진다.

그리고 이 인공지능층이 여러개인 인공지능을 Deep Learning이라고 한다.