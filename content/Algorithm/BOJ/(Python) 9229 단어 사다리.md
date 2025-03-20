---
created: 2025-02-27T22:09:23+09:00
modified: 2025-03-15T00:25:32+09:00
---
백준 BOJ 9229 단어 사다리

https://www.acmicpc.net/problem/9229

파이썬 python 문제 풀이

<br><br>

## 문제

단어 사다리란 퍼즐의 한 종류인데, 두 단어가 주어지면 한 단어에서 한 글자씩 바꿔서 다른 단어를 만드는 것이다. 이 게임은 좋은 어휘력과 맞춤법이 필요하다. 그래서 정답인지 아닌지 확인하는 게 너무 지루하고 귀찮다. 

한 쌍의 단어가 단어 사다리가 되는 조건은 다음과 같다:

- 단어의 길이가 같고
- 반드시 한 글자씩 바뀌어야한다.

단어 사다리가 가능한 지 판별하는 프로그램을 작성하시오.

<br><br>


## 입력

입력이 여러 번 주어지는데, '#'이 입력되기 전까지를 하나의 테스트케이스로 간주한다.

각 테스트케이스는 3자 이상 20자 이하의 대문자 알파벳으로 된 단어들이 순서대로 입력된다. 입력의 마지막 줄에는 '#'이 주어진다.

<br><br>

## 출력

단어 사다리가 가능하다면 'Correct'를, 아니면 'Incorrect'를 출력한다.

<br><br>


## 풀이

```python
# 9229 단어 사다리
# 브론즈 1

import sys

while True:
    before = sys.stdin.readline().strip()
    if before == "#":
        break
        
    flag = True # 모든 연속된 문자열 쌍이 조건을 만족하는지 체크
    
    while True:
        now = sys.stdin.readline().strip()
        if now == "#":
            break
            
        # 두 문자열이 길이가 같고 하나의 문자만 다른지 확인
        if len(before) != len(now) or sum(c1 != c2 for c1, c2 in zip(before, now)) != 1:
            flag = False
            
        before = now
        
    print("Correct" if flag else "Incorrect")

```



