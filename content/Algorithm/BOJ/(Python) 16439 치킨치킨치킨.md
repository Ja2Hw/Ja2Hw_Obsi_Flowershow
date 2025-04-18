---
created: 2025-02-27T22:09:23+09:00
modified: 2025-04-05T20:34:09+09:00
tags:
  - Python
  - "#Combinations"
---
백준 BOJ `16439 치킨치킨치킨` 문제의 파이썬 python 풀이입니다.

https://www.acmicpc.net/problem/16439


<br><br>


## 문제

N명의 고리 회원들은 치킨을 주문하고자 합니다.

치킨은 총 M가지 종류가 있고 회원마다 특정 치킨의 선호도가 있습니다. 한 사람의 만족도는 시킨 치킨 중에서 선호도가 가장 큰 값으로 결정됩니다. 진수는 회원들의 만족도의 합이 최대가 되도록 치킨을 주문하고자 합니다.

시키는 치킨의 종류가 많아질수록 치킨을 튀기는 데에 걸리는 시간도 길어지기 때문에 최대 세 가지 종류의 치킨만 시키고자 합니다.

진수를 도와 가능한 만족도의 합의 최댓값을 구해주세요.
<br><br><br>



## 입력

첫 번째 줄에 고리 회원의 수 _N_ (1 ≤ _N_ ≤ 30) 과 치킨 종류의 수 _M_ (3 ≤ _M_ ≤ 30) 이 주어집니다.

두 번째 줄부터 N개의 줄에 각 회원의 치킨 선호도가 주어집니다.

i+1번째 줄에는 i번째 회원의 선호도가 주어집니다.

<br><br><br>


## 출력

첫 번째 줄에 고리 회원들의 만족도의 합의 최댓값을 출력합니다.

<br><br><br>


## 접근 : `itertools.combinations`

정석적인 풀이 방법은 치킨의 종류 M개 중에서 3개를 고르는 조합을 브루트포스로 전부 시도해 보는 것일 겁니다.
- `combinations`를 활용하면 좋겠습니다.

<br>
### `itertools.combinations`

`itertools` 모듈의 `combinations(iterable, r)` 함수는
- 주어진 `iterable`에서 **r개를 뽑는 모든 조합**을 만들어 줍니다.
- **순서 상관없이 중복 없이** 조합을 만듭니다.
- 반환되는 것은 **튜플들의 이터레이터**

<br>
#### 사용 예시
```python
from itertools import combinations

for comb in combinations([1, 2, 3, 4], 2):
    print(comb)

```

출력 결과

```scss
(1, 2)
(1, 3)
(1, 4)
(2, 3)
(2, 4)
(3, 4)
```
- 4개의 원소 중에서 2개씩 뽑는 모든 조합을 출력한 것!

<br><br><br>

## 풀이

```python
# 16439 치킨치킨치킨

import sys
from itertools import combinations

input = sys.stdin.readline

n, m = map(int, input().split())

# 선호도 정보 저장
likes = [list(map(int, input().split())) for _ in range(n)]

max_total = 0

# 치킨 종류 중 3개 고르는 모든 조합
for comb in combinations(range(m), 3):
    total = 0
    for i in range(n):
        # 회원 i의 선호도 중 선택된 3개 치킨에 대한 점수 중 최고값
        max_like = max(likes[i][comb[0]], likes[i][comb[1]], likes[i][comb[2]])
        total += max_like
    max_total = max(max_total, total)

print(max_total)

```

- `likes [1] [3]`은 2번째 회원이 4번째 치킨에 대해 매긴 선호 점수에 해당할 것
- `combinations(range(m), 3)` → 치킨 종류 중 3개 고르기
- 회원별로 3개 치킨 중 최고 선호도만 선택해서 총합 계산
- 모든 조합을 시도해서 **최대값**을 찾음


<br><br>