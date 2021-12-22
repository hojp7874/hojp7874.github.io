> 간혹 우리는 switch/case 구문 또는 do/while 구문을 사용하고 싶습니다.
> 그러나 Python에서는 이런 문법을 지원하지 않습니다.
> 이를 흉내내기 위해 파이썬 3.8부터는 왈러스 연산자 (`:=`) 를 만들었습니다.


## 왈러스 연산자? `:=`
대입식 이라고도 불리는 왈러스 연산자는 __as 구문__과 쓰임이 유사합니다.

간단히 예제 코드를 살펴봅시다.

```python
# 해당 조건에 맞으면 True를 반환하는 코드
def condition():
	pass

# <if len(filter(condition, dataset)) as cnt:> 와 유사
if cnt := len(filter(condition, dataset)):
	print(f"해당 조건에 맞는 검색결과가 {cnt}건 있습니다.")
else:
	print("해당 조건에 맞는 검색결과가 없습니다.")
```

안타깝게도 if 문 또는 while 문에서는 as를 사용할 수 없기 때문에 왈러스 연산자를 이용해야 합니다.

왈러스 연산자가 사용된 `if cnt := len(filter(condition, dataset))` 행의 로직은 아래와 같습니다.

1. `cnt`에 `len(filter(condition, dataset))`을 대입한다.
2. `cnt`가 `True`면 if문을 실행한다.

이를 잘 활용하면 `switch/case` 구문과 `do/while` 구문을 파이썬에서도 흉내낼 수 있습니다.

## switch/case 흉내내기

예제를 만들어봅시다.

> 철수는 스포츠경기 관람 좋아합니다.
> 
> 축구, 야구, 농구 순으로 좋아합니다.
> 
> 선호하는 스포츠 경기 수를 조회하는 로직을 만들려고 합니다.
>
> 
> 만약 축구경기가 열려있다면 이를 조회하고,
> 
> 축구경기가 없다면 야구경기를,
> 
> 야구경기가 없다면 농구경기를 보여주는 로직을 구현합니다.


```python
cnt = len(filter(<축구 경기>, <모든 경기>))
if cnt:
	print(f'축구 경기가 {cnt}개 있습니다.')
else:
	cnt = len(filter(<야구 경기>, <모든 경기>))
	if cnt:
		print(f'야구 경기가 {cnt}개 있습니다.')
	else:
		cnt = len(filter(<농구 경기>, <모든 경기>))
		if cnt:
			print(f'농구 경기가 {cnt}개 있습니다.')
		else:
			print('선호하는 경기가 없습니다.')
```

if, else문의 반복으로 가독성이 현저히 저하됩니다.

왈러스 연산자를 이용하면 이 코드를 획기적으로 정리할 수 있습니다.

```python
if cnt := len(filter(<축구 경기>, <전체 경기>)):
	print(f'축구 경기가 {cnt}개 있습니다.')
elif cnt := len(filter(<야구 경기>, <전체 경기>)):
	print(f'야구 경기가 {cnt}개 있습니다.')
elif cnt := len(filter(<농구 경기>, <전체 경기>)):
	print(f'농구 경기가 {cnt}개 있습니다.')
else:
	print('선호하는 경기가 없습니다.')
```

## do/while 흉내내기

예제를 만들어봅시다.

> 철수는 야구경기가 진행되는 도중에 경기장에 도착했습니다.
> 
> 그리고 잠시 뒤에 있는 농구 경기도 보고싶었습니다.
> 
> 그래서 야구경기를 home팀이 10점을 따는 라운드 까지만 보려고 합니다.
> 
> 철수가 마지막으로 본 라운드를 출력하는 로직을 만들어봅시다.


```python
rnd = get_round()
home_score = get_home_score()
while home_score < 10 or rnd <= 9:
	home_score = get_home_score()
	rnd += 1
print(f'HOME팀의 점수: {home_score}점')
```

`home_score = get_home_score()` 구문이 불필요하게 중복됩니다.

왈러스 연산자를 이용해 중복을 줄여봅시다.

```python
rnd = get_round()
while home_score := get_home_score() < 10:
	rnd += 1
print(f'HOME팀의 점수: {home_score}점')
```

