---
created: 2025-02-27T22:09:23+09:00
modified: 2025-03-07T00:40:53+09:00
---
백준 BOJ 1251 단어 나누기

https://www.acmicpc.net/problem/1251

파이썬 python 문제 풀이

<br><br>

## 문제

알파벳 소문자로 이루어진 단어를 가지고 아래와 같은 과정을 해 보려고 한다.

먼저 단어에서 임의의 두 부분을 골라서 단어를 쪼갠다. 즉, 주어진 단어를 세 개의 더 작은 단어로 나누는 것이다. 각각은 적어도 길이가 1 이상인 단어여야 한다. 이제 이렇게 나눈 세 개의 작은 단어들을 앞뒤를 뒤집고, 이를 다시 원래의 순서대로 합친다.

예를 들어,

- 단어 : arrested
- 세 단어로 나누기 : ar / rest / ed
- 각각 뒤집기 : ra / tser / de
- 합치기 : ratserde

단어가 주어지면, 이렇게 만들 수 있는 단어 중에서 사전순으로 가장 앞서는 단어를 출력하는 프로그램을 작성하시오.

<br><br>


## 입력

첫째 줄에 영어 소문자로 된 단어가 주어진다. 길이는 3 이상 50 이하이다.

<br><br>

## 출력

첫째 줄에 구하고자 하는 단어를 출력하면 된다.

<br><br>


## 풀이

```python
# 1251 단어 나누기
# 실버 5

import sys

word = list(sys.stdin.readline().strip())
answer = []
tmp = []

for i in range(1, len(word) - 1):
    for j in range(i + 1, len(word) ):
        a = word[:i]
        b = word[i:j]
        c = word[j:]
        a.reverse()
        b.reverse()
        c.reverse()
        tmp.append(a + b + c)

for a in tmp:
    answer.append(''.join(a))

print(sorted(answer)[0])

```



