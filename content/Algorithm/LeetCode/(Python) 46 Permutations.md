---
created: 2025-04-22T02:12:21+09:00
modified: 2025-04-22T02:17:37+09:00
---
리트코드 LeetCode `46 Permutations` 문제의 파이썬 python 풀이입니다.

[https://leetcode.com/problems/permutations/](https://leetcode.com/problems/permutations/)  
  
  
<br><br>


## 문제

2에서 9까지 숫자가 주어졌을 때 전화 번호로 조합 가능한 모든 문자를 출력하라.


<br><br>


## 풀이

DFS를 활용한 정석적인 풀이 방법과, 파이썬 `itertools` 모듈을 사용한 편리한 풀이 방법 두 가지를 작성했습니다.

<br>

### 1. DFS를 활용한 순열 생성

```
# 순열 : DFS

class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:

        result = [] # 최종 순열 결과를 저장하는 리스트
        prev_elements = [] # 지금까지 선택한 숫자들을 저장 (경로)

        def dfs(elements): # 현재 사용할 수 있는 숫자들(elements)를 입력으로 받음
            # 더 이상 선택할 수 없는 숫자가 없는 리프 노드일 때 -> 하나의 순열이 완성된 것
            if len(elements) == 0:
                result.append(prev_elements[:]) # 현재까지 선택된 숫자 경로(prev_elements)를 복사하여 결과에 추가
            
            # 순열 생성 재귀 호출
            for e in elements:
                next_elements = elements[:]
                next_elements.remove(e)

                prev_elements.append(e)
                
                dfs(next_elements)

                prev_elements.pop()
        
        dfs(nums)

        return result
```

```
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
```

- `permute` 함수는 nums라는 정수 리스트를 받아서, 가능한 모든 순열을 리스트 형태로 반환합니다.
- 예: nums = [1, 2, 3] → 결과는 [[1,2,3],[1,3,2],[2,1,3],...[3,2,1]] 

<br>

#### DFS  재귀 호출

```python
for e in elements:
    next_elements = elements[:]
    next_elements.remove(e)

    prev_elements.append(e)
    dfs(next_elements)
    prev_elements.pop()

```

- 현재 선택할 수 있는 숫자들 중 하나(e)를 고릅니다.
- e를 선택한 후엔 더 이상 해당 숫자를 선택할 수 없으므로 elements에서 제거한 새 리스트(next_elements)를 만듭니다.
- 그 숫자를 현재 경로(prev_elements)에 추가하고 나머지 숫자들을 대상으로 DFS를 재귀적으로 실행합니다.
- 재귀 호출 이후에는 prev_elements에서 마지막에 추가한 숫자를 제거하여 백트래킹합니다.

<br>

#### 예시

입력:  nums = [1, 2, 3]

```
Start: prev=[] | elements=[1,2,3]

→ 1 선택 → prev=[1] | elements=[2,3]
  → 2 선택 → prev=[1,2] | elements=[3]
    → 3 선택 → prev=[1,2,3] | elements=[]
      → 저장: [1,2,3]
    ← 3 제거
  ← 2 제거

  → 3 선택 → prev=[1,3] | elements=[2]
    → 2 선택 → prev=[1,3,2] | elements=[]
      → 저장: [1,3,2]
    ← 2 제거
  ← 3 제거
← 1 제거

→ 2 선택 → prev=[2] | elements=[1,3]
...
(이와 같이 총 6개의 순열이 완성됨)

```

<br>

이 코드는 백트래킹 기법을 사용하여 nums의 모든 순열을 구합니다.

재귀 호출을 통해 하나씩 선택하고, 선택이 완료되면 결과에 저장하며, 선택을 되돌리며(백트래킹) 다른 경우의 수를 시도합니다.

**시간복잡도는 O(n!)**입니다. 가능한 모든 순열을 생성해야 하므로, n개의 숫자에 대해 n!개의 경우의 수가 생깁니다.

<br><br>

---

<br>

### 2. itertools 모듈 사용

파이썬에는 `itertools`라는 모듈이 있습니다.   `itertools` 모듈은 반복자 생성에 최적화된 효율적인 기능들을 제공하므로, 실무에서는 알고리즘으로 직접 구현하기보다는 가능하다면  `itertools` 모듈을 사용하는 편이 낫겠습니다.

```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
    	return list(map(list, itertools.permutations(nums)))
```

<br>

`itertools.permutations()` 함수의 결과물은 리스트 내 튜플입니다. 리트코드 문제에서는 리스트를 반환하도록 요구하기 떄문에, 리스트로 map 해주는 과정을 추가했습니다.

<br><br>