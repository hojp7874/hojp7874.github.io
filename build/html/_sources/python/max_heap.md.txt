# heapq로 최대힙 사용하기

> 많은 파이썬 이용자들이 `heapq` 표준 라이브러리를 사용하지만 해당 라이브러리는 __Max Heap__ 을 구현하지 못하는 단점이 있습니다.
>
> 그러나 `heapq` 소스코드에는 __Max Heap__ 의 많은 부분이 이미 구현되어있습니다.
> 
> 본 문서에서는 `heapq`의 소스코드를 분석해 __Max Heap__ 을 이용하는 방법에 대해 알아보겠습니다.

## 소스코드 분석

[해당 링크는 파이썬 표준 라이브러리인 `heapq`의 소스코드 입니다.](https://github.com/python/cpython/blob/main/Lib/heapq.py)

`heapq` 에서 공식적으로 사용하도록 지정한 함수의 목록은 아래와 같습니다.

```python
__all__ = ['heappush', 'heappop', 'heapify', 'heapreplace', 'merge',
           'nlargest', 'nsmallest', 'heappushpop']
```

그러나 이 함수들은 __Min Heap__ 을 기준으로 설계되어있습니다.

우리가 찾아야 하는 것은 파이썬 접근제어자로 감춰져있는 아래의 함수들입니다.

- `_heapify_max`: __Max Heap__ 버전의 __heapify__ 입니다.
- `_heappop_max`: __Max Heap__ 버전의 __heappop__ 입니다.

보시다시피 이미 __heapify__ 와 __heappop__ 은 __Max Heap__ 버전이 이미 구현되어있습니다.

따라서 __Max Heap__ 버전의 __heappush__ 만 구현하여 사용하면 됩니다.

## _heappush_max 구현하기

`_heappush_max`를 구현하기 위해 `heappush` 함수를 살펴보겠습니다.

```python
def heappush(heap, item):
    """Push item onto heap, maintaining the heap invariant."""
    heap.append(item)
    _siftdown(heap, 0, len(heap)-1)
```

리스트 형태인 __heap__ 에 __item__ 을 넣고 최소힙으로 정렬하는 로직(`_siftdown`)이 동작합니다.

그리고 `_siftdown`의 __Max Heap__ 버전인 `_siftdown_max`도 이미 구현되어있습니다.

따라서 ___heappush_max__ 는 다음 코드로 손쉽게 구현할 수 있습니다.

```python
from heapq import _siftdown_max

def _heappush_max(heap, item):
    heap.append(item)
    _siftdown_max(heap, 0, len(heap)-1)
```

이어서 `_heappush_max`가 동작하는 전체적인 로직을 설명합니다.

### _siftdown_max 로직 분석

`_heappush_max`는 __heap__ 에 __item__ 을 push하고 `_siftdown_max` 함수를 실행합니다.

`_siftdown_max`는 최대힙을 정렬하는 알고리즘입니다.

소스코드는 아래와 같습니다.

```python
def _siftdown_max(heap, startpos, pos):
    'Maxheap variant of _siftdown'
    newitem = heap[pos]
    # Follow the path to the root, moving parents down until finding a place
    # newitem fits.
    while pos > startpos:
        parentpos = (pos - 1) >> 1
        parent = heap[parentpos]
        if parent < newitem:
            heap[pos] = parent
            pos = parentpos
            continue
        break
    heap[pos] = newitem
```

- 새로 추가된 인자를 `newitem`에 저장합니다. 현재 노드는 `newitem`을 가리킵니다.
- while문을 돌며 루프 한 번당 부모노드로 이동합니다.
- 만약 부모 노드가 새로운 인자보다 작으면 `부모 노드의 값`을 `현재 노드의 값`에 저장합니다.
- 그러나 부모 노드가 새로운 인자보다 크면 루프를 종료합니다.
- 루프가 종료된 순간 현재 노드에 `newitem`을 저장합니다.

## 장단점

일반적으로 최대힙을 구현할 때 가장 많이 사용되는 방법은 인자를 반수로 치환하는 방법과

또는 튜플로 치환하여 0번째 인자로 반수를 넣는 방법입니다.

예시)

```python
import heapq

max_heap = [...]

# 인자를 반수로 치환하는 방법
heapq.heapify(list(map(lambda x: -x, max_heap)))
heapq.heappush(-item, max_heap)
item = -heapq.heappop(max_heap)

# 튜플로 치환하여 0번째 인자로 반수를 넣는 방법
heapq.heapify(list(map(lambda x: (-x, x), max_heap)))
heapq.heappush((-item, item), max_heap)
item = heapq.heappop(max_heap)[1]
```

이 방법들과 비교하여 본 문서에서 설명한 방법의 장단점에 대해 설명합니다.

### 장점

장점을 설명하기 위해 알고리즘 문제를 예시로 들겠습니다.

[2019 카카오 개발자 겨울 인턴십 - 징검다리 건너기](https://programmers.co.kr/learn/courses/30/lessons/64062?language=python3)

> 알고리즘의 핵심 로직은 건너뛸 수 있는 최대(k) 범위 내에서 디딤돌의 최대값이 가장 작은 구간을 찾는 것입니다.
>
> 각 구간에서의 디딤돌의 최댓값을 구하기 위해 __Max Heap__ 을 사용한 풀이를 예시로 듭니다.

__예시 답안 1 (인자를 반수로 치환하는 방법)__

```python
def solution(stones, k):
    # max_heap[0]: (-max_val_in_range, pos)
    max_heap = list(zip(map(lambda x: -x, stones[:k]), range(k)))
    heapify(max_heap)
    result = float('inf')
    for right_pos, val in enumerate(stones):
        heappush(max_heap, (-val, right_pos))
        left_pos = right_pos - k
        while max_heap[0][1] <= left_pos:
            heappop(max_heap)
        if -max_heap[0][0] < result:
            result = -max_heap[0][0]
    return result
```

- heap에 들어가는 인자가 __반수__ 이기 때문에 코드를 짜면서 이를 항상 신경써야합니다.
- 또한 최댓값을 다른 상수와 비교하는 과정에서 앞에 `-` 부호가 있기 때문에 직관적이지 못합니다.

__예시 답안 2 (튜플로 치환하여 0번째 인자로 반수를 넣는 방법)__

```python
def solution(stones, k):
    # max_heap[0][1]: (max_val_in_range, pos)
    max_heap = list(zip(map(lambda x: -x, stones[:k]), zip(range(k), stones[:k])))
    heapify(max_heap)
    result = float('inf')
    for right_pos, val in enumerate(stones):
        heappush(max_heap, (-val, (right_pos, val)))
        left_pos = right_pos - k
        while max_heap[0][1][0] <= left_pos:
            heappop(max_heap)
        if max_heap[0][1][1] < result:
            result = max_heap[0][1][1]
    return result
```

- heap에서 인자를 추출할 때 다른 경우보다 1수준 더 접근해야하기 때문에 가독성이 떨어집니다.

  위의 코드들은 해결하려는 문제가 복잡해질수록 사용하기 까다로워집니다.

  그런 경우에는 새롭게 max_heap 함수를 구현하여 사용합시다.

__예시 답안 3 (max_heap 함수를 구현하여 사용한 방법)__

```python
def solution(stones, k):
    # max_heap[0]: (max_val_in_range, pos)
    max_heap = list(zip(stones[:k], range(k)))
    _heapify_max(max_heap)
    result = float('inf')
    for right_pos, val in enumerate(stones):
        _heappush_max(max_heap, (val, right_pos))
        left_pos = right_pos - k
        while max_heap[0][1] <= left_pos:
            _heappop_max(max_heap)
        if max_heap[0][0] < result:
            result = max_heap[0][0]
    return result
```

### 단점

- 함수를 사용하기 위한 선수과정이 번거롭습니다.

  ```python
  from heapq import _heapify_max, _heappop_max, _siftdown_max

  def _heappush_max(heap, item):
      heap.append(item)
      _siftdown_max(heap, 0, len(heap)-1) 
  ```

  함수를 사용하기 위해서는 위의 코드를 입력해야 합니다.

  만약 구현하고자 하는 코드가 간단하다면, 코드가 길어져 오히려 가독성을 해칠 수 있습니다.

- 공식적으로 이용하도록 지정한 함수가 아니기 때문에 프로젝트에 사용하기에 부적합합니다.

  *아래는 공식적으로 이용하도록 지정한 함수 목록입니다.*

  ```python
  __all__ = ['heappush', 'heappop', 'heapify', 'heapreplace', 'merge',
             'nlargest', 'nsmallest', 'heappushpop']
  ```

  사용하는 함수들이 ___protected__ 접근 제어자를 가집니다. 버전에 민감할 수 있으며, 모든 경우에서 라이브러리의 안정성을 보장하지 않습니다.

  프로젝트에서 사용하기 보다는 코딩테스트에서 사용하는 것을 권장합니다.

## Max Heap을 이용하는 더 좋은 방법

__MaxHeap__ 클래스를 만들어 사용하면 더욱 객체지향적인 코드를 만들 수 있습니다.

```python
import heapq

class MinHeap:
    def __init__(self, heap=[]):
        self.heap = heapq.heapify(heap)

    def __repr__(self):
        return self.heap

    def __getitem__(self, idx):
        return self.heap[idx]

    def __len__(self):
        return len(self.heap)

    def heappush(self, item: int):
        heapq.heappush(self.heap, item)

    def heappop(self, item: int):
        heapq.heappop(self.heap)


class MaxHeap:
    def __init__(self, heap=[]):
        self.heap = heapq._heapify_max(heap)

    def __repr__(self):
        return self.heap

    def __getitem__(self, idx):
        return self.heap[idx]

    def __len__(self):
        return len(self.heap)

    def heappush(self, item:int):
        self.heap.append(item)
        heapq._siftdown_max(self.heap, 0, len(self.heap)-1)

    def heappop(self, item: int):
        heapq._heappop_max(self.heap)
```