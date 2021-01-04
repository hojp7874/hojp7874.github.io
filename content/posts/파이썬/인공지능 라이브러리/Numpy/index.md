---
title: "Numpy"
date: 2020-12-18T21:47:04+09:00
hero: 
description:
menu:
  sidebar:
    name: Numpy
    identifier: Numpy
    parent: 인공지능 라이브러리
    weight: 10
draft: false
---

## 0. Numpy란?

> Vector와 Matrics를 다루기에 특화된 파이썬 라이브러리

## 1. 시작하기

터미널에서:

```shell
pip install numpy
```

shell 또는 python 파일에서:

```python
import numpy as np
```

## 2. Numpy로 연산하기

- vector와 scalar 사이의 연산이 가능하다.

  ```python
  A = np.array([1, 2, 3])
  x = 5
  print(A * x)
  
  -> [ 5 10 15]
  ```

  

- Indexing이 가능하다.

  ```python
  W = np.array([[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]])
  
  W[2, 3]
  
  -> 12
  ```

- Slicing이 가능하다.

  ```python
  W[:2, 1:3]
  ->
  
  array([[2, 3],
         [6, 7]])
  
  
  W[1:]
  
  -> 
  array([[ 5,  6,  7,  8],
         [ 9, 10, 11, 12]])
  ```

- Broadcasting이 가능하다.

  실제로는 연산이 불가능할지라도, Numpy는 스스로 피연산자의 형태를 변형하여 연산을 수행한다.

  1. __M * N, M * 1__

     ```python
     a = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
     x = np.array([0, 1, 0])
     
     x = x[:, None]
     print(a + x)
     
     ->
     [[1 2 3]
      [5 6 7]
      [7 8 9]]
     ```
  
  2. __M * N, 1 * N__
  
      ```python
     y = np.array([0, 1, -1])
   
     print(a * y)
   
     ->
     [[ 0  2 -3]
      [ 0  5 -6]
      [ 0  8 -9]]
     ```
  
      3. __M * 1, 1 * N__
  
      ```python
     # 열벡터
     t = np.array([1, 2, 3])
     t = t[:, None]
     
     #행벡터
     u = np.array([2, 0, -2])
     
     print(t + u)
     
     ->
     [[ 3  1 -1]
      [ 4  2  0]
      [ 5  3  1]]
      ```

## 3. Numpy로 선형대수 사용하기

### 영벡터(행렬)

- 원소가 모두 0인 벡터(행렬)
- `np.zeros(dim)`을 통해 생성, dim은 값, 혹은 튜플 (,)

```python
np.zeros(2)

-> array([0., 0.])
```

```python
np.zeros((3, 3, 3))

->
array([[[0., 0., 0.],
        [0., 0., 0.],
        [0., 0., 0.]],

       [[0., 0., 0.],
        [0., 0., 0.],
        [0., 0., 0.]],

       [[0., 0., 0.],
        [0., 0., 0.],
        [0., 0., 0.]]])
```



### 일벡터(행렬)

- 원소가 모두 1인 벡터(행렬)
- `np.ones(dim)`을 통해 생성, dim은 값, 튜플(,)

```python
np.ones(2)

-> array([1., 1.])
```

```python
np.ones((3, 3))

->
array([[1., 1., 1.],
       [1., 1., 1.],
       [1., 1., 1.]])
```

### 대각행렬

- Main Diagonal을 제외한 성분이 0일 행렬
- `np.diag((main_diagnals))`을 통해 생성할 수 있음.

```python
np.diag((2, 4))

->
array([[2, 0],
       [0, 4]])
```

```python
np.diag((1, 3, 5))

->
array([[1, 0, 0],
       [0, 3, 0],
       [0, 0, 5]])
```



### 항등행렬

- main diagonal == 1인 diagonal matrix(대각행렬)
- `np.eye(n, (dtype=int, uint, float, complex, ...))`을 통해 생성

```python
np.eye(2).dtype

-> dtype('float64')
```

```python
np.eye(3, dtype=int)

->
array([[1, 0, 0],
       [0, 1, 0],
       [0, 0, 1]])
```

### 행렬곱

- 행렬간에 정의되는 곱 연산(dot product)
- `np.dot()` 또는 `@`을 사용

```python
mat_1 = np.array([[1, 4], [2, 3]])
mat_2 = np.array([[7, 9], [0, 6]])
mat_1.dot(mat_2)

->
array([[ 7, 33],
       [14, 36]])
```

```python
mat_1 @ mat_2

->
array([[ 7, 33],
       [14, 36]])
```

### 트레이스

- Main diagonal의 합
- np.trace()를 사용

```python
arr = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
arr

->
array([[1, 2, 3],
       [4, 5, 6],
       [7, 8, 9]])
```

```python
np.trace(arr)

-> 15
```

```python
arr.trace()

-> 15
```

### 행렬식

- 행렬을 대표하는 값들 중 하나
- 선형변환 과정에서 Vector의 Scaling 척도
- `np.linalg.det()`으로 계산

```python
arr_2 = np.array([[2, 3], [1, 6]])
arr_2

->
array([[2, 3],
       [1, 6]])
```

```python
np.linalg.det(arr_2)

-> 9.000000000000002
```

```python
arr_3 = np.array([[1, 4, 7], [2, 5, 8], [3, 6, 9]])
arr_3

->
array([[1, 4, 7],
       [2, 5, 8],
       [3, 6, 9]])
```

```python
np.linalg.det(arr_3)

-> 0.0
```

### 역행렬

- 행렬 A에 대해 AB = BA = I을 만족하는 행렬 B = A^-1
- `np.linalg.inv()`로 계산

```python
mat = np.array([[1, 4], [2, 3]])
mat

->
array([[1, 4],
       [2, 3]])
```

```python
mat_inv = np.linalg.inv(mat)
mat_inv

->
array([[-0.6,  0.8],
       [ 0.4, -0.2]])
```

```python
mat @ mat_inv

->
array([[ 1.00000000e+00,  0.00000000e+00],
       [-1.11022302e-16,  1.00000000e+00]])
```

### 고유값과 고유벡터

- 정방행렬(nxn) A에 대해 𝐴𝑥=𝜆𝑥Ax=λx를 만족하는 상수 𝜆λ와 x를 각각 고유값과 고유벡터라 한다.
- `np.linalg.eig()`로 계산

```python
mat = np.array([[2, 0, -2], [1, 1, -2], [0, 0, 1]])
mat

->
array([[ 2,  0, -2],
       [ 1,  1, -2],
       [ 0,  0,  1]])
```

```python
np.linalg.eig(mat)

->
(array([1., 2., 1.]),
 array([[0.        , 0.70710678, 0.89442719],
        [1.        , 0.70710678, 0.        ],
        [0.        , 0.        , 0.4472136 ]]))
```

#### Validation

```python
eig_val, eig_vec = np.linalg.eig(mat)
eig_val

->
array([1., 2., 1.])


eig_vec

->
array([[0.        , 0.70710678, 0.89442719],
       [1.        , 0.70710678, 0.        ],
       [0.        , 0.        , 0.4472136 ]])
```

```python
mat @ eig_vec[:, 0]

-> array([0., 1., 0.])
```

```python
eig_val[0] * eig_vec[:, 0]

-> array([0., 1., 0.])
```