---
created: 2025-02-27T22:09:23+09:00
modified: 2025-03-20T21:27:46+09:00
---
백준 BOJ 32205 네모의 꿈

https://www.acmicpc.net/problem/32205

파이썬 python 문제 풀이

<br><br>

## 문제

귀여운 세모 등장 ><

세모는 네모가 되고 싶어요. 세모 N$N$개의 각 변의 길이가 주어질 때 원하는 세모 두 개를 골라 같은 길이의 변이 맞닿게 붙여서 네모를 만들 수 있는지 알려주세요!

네모란, 꼭짓점이 네 개이면서 어떤 내각도 180도가 아닌 단순 다각형이에요.

<br><br>


## 입력

첫째 줄에 세모의 개수 N이 주어진다. $(2 \le N \le 500\ 000)$ 

둘째 줄부터 N개의 줄에 걸쳐 각 줄에 세모의 세 변의 길이를 나타내는 정수 a, b, c가 공백으로 구분되어 주어진다. 변의 길이가 a, b, c인 삼각형은 존재한다. $(1 \le a \lt b \lt c \le 10^6)$

<br><br>

## 출력

원하는 세모 두 개를 골라 같은 길이의 변이 맞닿게 붙여서 네모를 만들 수 있으면 `1`, 없으면 `0`을 출력한다.

<br><br>


## 풀이


두 개의 삼각형을 맞붙여서 네모(사각형)를 만들 수 있는지 확인하는 문제로, 두 삼각형이 같은 길이의 변을 공유하면서 붙으면 네모가 된다.

```python
edges = {}
```
이 딕셔너리는 `특정 길이의 변을 가진 모든 삼각형들의 정보`를 저장
- 키: 변의 길이
- 값: 그 길이의 변을 가진 삼각형들의 나머지 두 변의 길이를 튜플로 저장

<br>

```python
for a, b, c in tris:
    if a not in edge:
        edge[a] = []
    if b not in edge:
        edge[b] = []
    if c not in edge:
        edge[c] = []
    
    edge[a].append((b, c))
    edge[b].append((a, c))
    edge[c].append((a, b))
```
- 각 삼각형(a, b, c)에 대해
    - a 길이의 변을 공유한다면: 나머지 두 변 (b, c)를 edge[a]에 저장
    - b 길이의 변을 공유한다면: 나머지 두 변 (a, c)를 edge[b]에 저장
    - c 길이의 변을 공유한다면: 나머지 두 변 (a, b)를 edge[c]에 저장

예를 들어, [3, 4, 5]와 [3, 7, 8] 두 삼각형이 있다면
- edge[3]에는 [(4, 5), (7, 8)]이 저장
	- `길이가 3인 변을 가진 삼각형들의 나머지 두 변의 길이들`을 의미

<br>

```python
for edge_length, triangle_side in edge.items():
    if len(triangle_side) >= 2:  # 같은 길이의 변을 가진 삼각형이 2개 이상인 경우
        for i in range(len(triangle_side)):
            for j in range(i + 1, len(triangle_side)):
                side1, side2 = triangle_side[i]
                side3, side4 = triangle_side[j]
                
                sides = [side1, side2, side3, side4]
                sides.sort()
                
                if sides[3] < sides[0] + sides[1] + sides[2]:
                    return True
```
- 각 변의 길이마다, 그 길이를 공유하는 삼각형이 2개 이상 있는지 확인
	- 만약 있다면, 두 삼각형을 선택해서 사각형을 만들 수 있는지 판단
- 사각형이 될 수 있는 조건: **가장 긴 변 < 나머지 세 변의 합**
    - 이는 사각형의 성립 조건으로, 이 조건이 만족되지 않으면 사각형이 아닌 일자로 펴진 형태가 된다

예를 들어, 삼각형 [3, 4, 5]와 [3, 7, 8]을 길이 3인 변에서 붙인다면
- 사각형의 네 변은 4, 5, 7, 8이 됨
- 가장 긴 변(8) < 나머지 변의 합(4+5+7=16)이므로 사각형 형성이 가능함

<br>

### 전체 코드

```python
# 32205 네모의 꿈

import sys

def sq_check(tri):
    # 각 변의 길이를 키로 하는 해시맵을 생성
    # 값은 해당 변을 가진 삼각형들의 다른 두 변의 길이 쌍을 저장

    edge = {}
    for a, b, c in tri:
        if a not in edge:
            edge[a] = []
        if b not in edge:
            edge[b] = []
        if c not in edge:
            edge[c] = []
        edge[a].append((b, c))
        edge[b].append((a, c))
        edge[c].append((a, b))

    # 각 변 길이에 대해 확인
    for edge_length, triangle_side in edge.items():
        # 같은 길이의 변을 가진 삼각형이 2개 이상인 경우에만 확인
        if len(triangle_side) >= 2:
            for i in range(len(triangle_side)):
                for j in range(i + 1, len(triangle_side)):
                    side1, side2 = triangle_side[i]
                    side3, side4 = triangle_side[j]
                    
                    # 삼각형을 붙였을 때 사각형이 될 수 있는지 검사
                    # 삼각형 부등식을 활용하여 사각형이 될 수 있는지 확인
                    # 네 변의 길이를 a, b, c, d라 할 때,
                    # 가장 긴 변 < 나머지 세 변의 합이어야 사각형 형성 가능
                    sides = [side1, side2, side3, side4]
                    sides.sort()
                    
                    if sides[3] < sides[0] + sides[1] + sides[2]:
                        return True

    return False
    # 모든 가능한 변의 길이와 삼각형 쌍에 대해 확인
    # 하나라도 사각형을 만들 수 있는 경우가 발견되면 바로 True를 반환
    # 끝까지 찾지 못하면 False를 반환


n = int(sys.stdin.readline())

tri = []

for _ in range(n):
    a, b, c = map(int, sys.stdin.readline().split())
    tri.append((a, b, c))

# 결과 출력
if sq_check(tri):
    print(1)
else:
    print(0)

```