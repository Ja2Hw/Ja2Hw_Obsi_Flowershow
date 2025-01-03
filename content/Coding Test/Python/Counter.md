---
created: 2025-01-03T10:30:18+09:00
modified: 2025-01-03T10:48:22+09:00
---
# Counter
- `Counter`는 Python의 표준 라이브러리인 `collections` 모듈에서 제공하는 클래스
- 리스트나 문자열 등의 iterable 객체에서 각 요소의 빈도(횟수)를 쉽게 계산할 수 있게 도와주는 도구로, **빈도 수를 계산하는 데 특화된 딕셔너리**라고 생각할 수 있음
<br>

## ## `Counter`의 기본 개념

`Counter`는 주어진 iterable을 입력받아, 각 요소를 키(key)로, 해당 요소의 빈도(개수)를 값(value)으로 저장하는 딕셔너리 형태의 객체를 생성
```python
from collections import Counter 

# 리스트에서 각 요소의 빈도 계산 
votes = [1, 2, 2, 3, 3, 3] 
counter = Counter(votes) 

print(counter) 
# 출력: Counter({3: 3, 2: 2, 1: 1})
```
### **딕셔너리와 유사하게 동작**
- `Counter` 객체는 딕셔너리와 유사하게 동작함. 키를 사용해 특정 요소의 빈도를 조회하거나, 새 값을 추가할 수 있음
```python
counter = Counter([1, 2, 2, 3, 3, 3]) 

print(counter[2])  
# 2의 빈도 출력: 2 
counter[2] += 1    
# 2의 빈도 증가 
print(counter[2])  
# 2의 빈도 출력: 3

```
<br>

## 주요 메서드
### 1. **`values()`**
- counter.values()는 각 후보의 투표 수를 리스트 형태로 반환해줌
```python
max_vote = max(counter.values())
```

### 2. **`items()`**
- counter.items()는 각 후보와 투표 수의 쌍을 반환
```python
#enumerate 쓸 때와 유사한 방식으로 사용
res = [i for i, value in counter.items() if value == max_vote]
```

### 3. **`most_common(n)`**

- 빈도 수가 높은 순으로 상위 `n`개의 요소와 빈도를 반환
```python
counter = Counter([1, 2, 2, 3, 3, 3]) 
print(counter.most_common(1)) 
# 출력: [(3, 3)]  # 가장 빈도가 높은 3이 3번 등장
```

### 4. **`del`**
- `del` 키워드를 사용해 특정 키를 제거할 수 있음

```python
`counter = Counter([0, 1, 2, 0, 3]) 

del counter[0]  # 0의 빈도를 제거 

print(counter) 
# 출력: Counter({1: 1, 2: 1, 3: 1})`
```

<br>
## 참고 예제 : 백준 20113번 긴급 회의
https://www.acmicpc.net/problem/20113

```python

import sys
from collections import Counter

n = int(sys.stdin.readline())
vote = list(map(int, sys.stdin.readline().split()))
# vote만 사용해서 enumerate로 하면 0 값 처리가 어려움
# 0은 투표를 받지 않은 경우이므로 0을 제외한 후보들의 투표 수를 카운트
counter = Counter(vote)
if 0 in counter:
    del counter[0]

# 투표 수가 비어있으면 "skipped"
if not counter:
    print("skipped")
else:
    # counter.values()는 각 후보의 투표 수를 리스트 형태로 반환해줌
    # max를 사용해 최다 득표수 계산
    max_vote = max(counter.values())
    
    # counter.items()는 각 후보와 투표 수의 쌍을 반환
    # enumerate 쓸 때와 유사한 방식으로 사용
    res = [i for i, value in counter.items() if value == max_vote]
    
    if len(res) > 1:  # 동점자 있을 때
        print("skipped")
    else: # 최다 득표자 유일
        print(res[0])

```
### 이 코드에서 `Counter`의 사용 목적
- **후보별 투표 수를 계산**: `vote` 리스트에 저장된 각 후보 ID의 빈도를 계산
- **투표를 받지 않은 경우(값이 0)를 제외**: 후보 ID가 아닌 `0` 값을 무시
- **최다 득표자를 계산하는 데 활용**: `Counter` 객체를 통해 각 후보의 득표 수를 저장하고, 이를 기반으로 최다 득표자와 동점 여부를 판단

### 코드 흐름
- **입력 처리**
    - `n`은 투표 수, `vote`는 투표 데이터(후보 ID 목록)
- **투표 수 계산**
    - `Counter(vote)`를 사용하여 각 후보 ID의 투표 수를 계산
    - `0`을 제거하여 투표하지 않은 값을 무시
- **결과 판별**
    - `max_vote`를 이용해 최다 득표수를 계산
    - `res`를 이용해 최다 득표 후보 리스트를 작성
    - 동점자가 있으면 "skipped"를 출력
    - 그렇지 않으면 유일한 최다 득표자의 ID를 출력

<br>