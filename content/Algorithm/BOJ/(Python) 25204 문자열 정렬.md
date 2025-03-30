---
created: 2025-02-27T22:09:23+09:00
modified: 2025-03-30T00:04:42+09:00
tags:
  - Python
---
백준 BOJ 25204 문자열 정렬 파이썬 python 문제 풀이입니다.

https://www.acmicpc.net/problem/25204


<br>

두 가지 풀이 방법을 작성했습니다.

1. functools 모듈을 활용한 풀이
2. `__lt__` (less than) 연산자를 오버라이드한 래퍼 클래스를 이용한 풀이


<br><br><br>


## 문제
Bob은 문자열 정렬기를 만들어야한다.

문자열은 영문 알파벳 이외에도 붙임표 ('`-`')가 들어가는 경우가 있기 때문에 처리가 까다롭다. 이 문제에 한해, 서로 다른 두 문자열 $X$, $Y$의 "사전식" 순서는 아래 규칙에 따라 정한다:

1.  $X$가 $Y$의 접두사인 (prefix) 경우 $X$가 $Y$보다 앞선다. 반대로, $Y$가 $X$의 접두사인 경우 $Y$가 $X$보다 앞선다.
2. 위 경우에 해당하지 않을 경우: 첫번째 문자부터 시작하여 $X$, $Y$가 처음으로 _달라지는 부분이_ i$i$번째 문자라 하자. $X$의 $i$번째 문자를 $X_i$, $Y$의 $i$번째 문자를 $Y_i$라 했을 때:
    1. 둘 중 하나만 붙임표 ('`-`')인 경우, 붙임표가 들어간 문자열이 사전순에서 뒤진다. 예를 들어, Xi$X_i$가 붙임표이고 $Y_i$가 붙임표가 아닌 경우, $Y$가 앞선다.
    2. 둘 다 알파벳인 경우, 대소문자를 무시했을 때 서로 다른 알파벳이라면, 알파벳 순서를 따른다. 만약 같은 알파벳이지만 하나만 대문자인 경우 대문자가 들어간 문자열이 사전순에서 앞선다.

예를 들어, 아래와 같은 문자열 쌍을 비교해보자:

-  $X$  =`Santa-Mar`과 $Y$ = `Santa-Maria`이 경우 규칙 1에 따라 $X$가 앞선다. $X$가 $Y$의 접두사이기 때문이다.
-  $X$ = `San-Francisco` 와 $Y$ = `Santa-Clara`: 이 경우 처음 3$3$개의 문자는 같지만 4$4$번째 문자가 '`-`'와 '`t`'로 다르다. 규칙 2-1에 따라 $X$가 $Y$에 뒤진다.
-  $X$ = `Seoul`과 $Y$ = `seoul`: 이 경우 규칙 2-2에 따라 `Seoul`이 앞선다.

입력으로 $n$개의 문자열이 주어졌을 때, 위 규칙에 따라 사전식 정렬 후 출력하는 문자열 정렬기를 만들어보자.

<br><br><br>



## 입력

첫 줄에 테스트 케이스의 수 $T$가 주어진다.

각 테스트 케이스의 첫 줄에는 $n$이 주어진다. 다음 $n$줄에 걸쳐 각 줄에 문자열이 하나씩 주어진다.

<br><br><br>


## 출력

각 테스트 케이스의 정답을 $n$줄에 걸쳐 출력한다. 각 줄에는 사전순으로 정렬된 문자열 하나씩을 순서대로 출력한다.

<br><br><br>


## 풀이

### 1. functools 모듈을 활용한 풀이
- `functools` 모듈과 `cmp_to_key` 함수
	- `functools`는 파이썬의 내장 모듈로, 함수를 다루는 데 유용한 도구들을 제공합니다. 그 중 `cmp_to_key` 함수는 Python 2에서 사용되던 비교 함수를 Python 3의 키 함수로 변환해주는 역할을 합니다.
		- Python 2에서는 `sort()` 함수에 `cmp` 매개변수를 제공하여 두 요소를 직접 비교하는 함수를 사용할 수 있었습니다.
		- Python 3에서는 이러한 방식이 제거되고 `key` 매개변수만 남았습니다.
		- `cmp_to_key`는 두 요소를 비교하는 함수를 받아서, 각 요소에 대해 정렬 키를 생성하는 함수로 변환해줍니다.

```python
# 25204 문자열 정렬

import sys
import functools

def compare(s1, s2):
    # 두 문자열을 한 글자씩 비교
    i = 0
    min_len = min(len(s1), len(s2)) # 두 문자열 중 더 짧은 문자열의 길이
    
    while i < min_len:
        # 두 문자열의 현재 위치 문자가 같으면 다음 문자로 넘어감
        if s1[i] == s2[i]:
            i += 1
            continue
            
        # 다른 문자를 발견했을 때
        # 붙임표(-)) 우선순위 처리 => 두 문자가 다르고 그 중 하나가 붙임표(-)라면, 붙임표가 아닌 문자열이 앞에 온다
        if s1[i] == '-':
            return 1  # s2가 앞선다
        if s2[i] == '-':
            return -1  # s1이 앞선다
            
        # 알파벳 비교
        if s1[i].lower() != s2[i].lower():
            # 대소문자 무시했을 때 다른 알파벳이면 알파벳 순서
            return -1 if s1[i].lower() < s2[i].lower() else 1
        else:
            # 같은 알파벳이지만 대소문자가 다른 경우 대문자가 앞선다
            return -1 if s1[i].isupper() and s2[i].islower() else 1 if s1[i].islower() and s2[i].isupper() else 0
    
    # 여기까지 왔다면 접두사 관계 확인
    if len(s1) == len(s2):
        return 0
    # 짧은 문자열이 긴 문자열의 접두사면 짧은 것이 앞선다
    return -1 if len(s1) < len(s2) else 1

# 입력 처리
input = sys.stdin.readline
T = int(input())

for _ in range(T):
    N = int(input())
    strings = [input().strip() for _ in range(N)]
    
    # functools.cmp_to_key를 사용하여 정렬
    sorted_strings = sorted(strings, key=functools.cmp_to_key(compare))
    
    # 순차적으로 출력
    for s in sorted_strings:
        print(s)

```

각 테스트 케이스마다
1. 문자열 개수 `N`을 입력받습니다.
2. `N`개의 문자열을 입력받아 리스트에 저장합니다.
3. `sorted()` 함수와 `functools.cmp_to_key(compare)`를 사용하여 문자열을 정렬합니다.
4. 정렬된 문자열을 한 줄에 하나씩 출력합니다.

<br><br>

### 2. `__lt__` (less than) 연산자를 오버라이드한 래퍼 클래스를 이용한 풀이
모듈 없이도 구현이 가능한 방식입니다.

- 파이썬 클래스와 비교 연산자 오버라이딩

파이썬에서 객체를 비교할 때 (`<`, `>`, == 등), 파이썬은 특정 메서드를 찾아 호출합니다. 이것을 `magic method` 또는 `double underscore method` 라고 부릅니다.

- `__lt__` (less than): `<` 연산자에 대응
- `__gt__` (greater than): `>` 연산자에 대응
- `__eq__` (equal): == 연산자에 대응

파이썬의 `sort()` 함수는 객체들을 정렬할 때 기본적으로 `<` 연산자를 사용합니다.
	`__lt__` 메서드를 구현하면, 파이썬은 정렬 과정에서 이 메서드를 호출하여 객체들의 순서를 결정합니다.


```python
# 25204 문자열 정렬

import sys

# 입력 처리
input = sys.stdin.readline
T = int(input())

for _ in range(T):
    N = int(input())
    strings = [input().strip() for _ in range(N)]
    
    # 직접 비교 정렬 구현 (파이썬 내장 정렬 알고리즘 활용)
    # 두 문자열 비교를 위한 클래스 정의
    # 문자열을 받아서 객체 내부에 저장
    class StringComparer:
        def __init__(self, string):
            self.string = string
            
        def __lt__(self, other):
		    # '<' 연산자 구현
		    # self는 왼쪽 객체(a), other은 오른쪽 객체(b)를 가리킴
            s1, s2 = self.string, other.string
            
            # 문자별 비교
            # i는 현재 비교하는 문자의 위치(인덱스)
            # min_len은 두 문자열 중 더 짧은 것의 길이
            i = 0
            min_len = min(len(s1), len(s2))
            
            while i < min_len:
                # 같은 위치의 문자가 같으면 다음 문자로
                if s1[i] == s2[i]:
                    i += 1
                    continue
                    
                # 다른 문자 발견 시
                #  붙임표 처리
                # s1[i]가 붙임표면, s1은 s2보다 뒤에 와야 함 => s1 < s2는 거짓이므로 False 반환
                # s2[i]가 붙임표면, s1은 s2보다 앞에 와야 함 => s1 < s2는 참이므로 True 반환
                if s1[i] == '-':
                    return False  # s2가 앞선다
                if s2[i] == '-':
                    return True   # s1이 앞선다
                
                # 알파벳 비교
                if s1[i].lower() != s2[i].lower():
                    # 대소문자 무시했을 때 다른 알파벳이면 알파벳 순서
                    return s1[i].lower() < s2[i].lower()
                else:
                    # 같은 알파벳이지만 대소문자가 다른 경우 대문자가 앞선다
                    if s1[i].isupper() and s2[i].islower():
                        return True
                    if s1[i].islower() and s2[i].isupper():
                        return False
                    # 둘 다 같은 대소문자면 계속 비교
                    i += 1
                    continue
            
            # 접두사 관계 확인
            return len(s1) < len(s2)
    
    # 래퍼 클래스를 사용하여 정렬
    sorted_strings = [s.string for s in sorted([StringComparer(s) for s in strings])]
    
    # 순차적으로 출력
    for s in sorted_strings:
        print(s)

```

- 두 문자가 다를 때, 먼저 붙임표(`-`)를 처리
	- `s1[i]`가 붙임표면, `s1`은 `s2`보다 뒤에 와야 함 ( `s1 < s2`는 거짓이므로 `False` 반환 )
	- `s2[i]`가 붙임표면, `s1`은 `s2`보다 앞에 와야 함 (`s1 < s2`는 참이므로 `True` 반환 )

- 붙임표가 아닌 경우 알파벳 비교
	- 두 문자를 소문자로 변환했을 때 다르면, 알파벳 순서로 비교
	- 두 문자가 같은 알파벳의 다른 대소문자인 경우, 대문자가 앞에 온다
	    - `s1[i]`가 대문자이고 `s2[i]`가 소문자면 `s1`이 앞에 와야 함 (`True` 반환)
	    - `s1[i]`가 소문자이고 `s2[i]`가 대문자면 `s2`가 앞에 와야 함 (`False` 반환)
	- 같은 대소문자라면 다음 문자로 넘어감

- 한 문자열이 다른 문자열의 접두사인 경우
	- 더 짧은 문자열이 앞에 와야 하므로, `s1`의 길이가 `s2`보다 짧으면 `True`, 아니면 `False`를 반환


```python
# 래퍼 클래스를 사용하여 정렬
sorted_strings = [s.string for s in sorted([StringComparer(s) for s in strings])]
```
1. `[StringComparer(s) for s in strings]`: 모든 문자열을 `StringComparer` 객체로 변환
2. `sorted([...])`: 변환된 객체들을 정렬 =>  `__lt__` 메서드를 사용하여 객체들을 비교
3. `[s.string for s in ...]`: 정렬된 객체에서 원래 문자열을 추출

<br>

문자열 비교를 위한 클래스를 정의하고, 파이썬의 내장 비교 연산자(`<`)를 오버라이드하여 문제에서 요구하는 특별한 정렬 규칙을 구현했습니다. 이렇게 하면 `functools` 모듈이나 다른 외부 의존성 없이도 복잡한 정렬 로직을 구현할 수 있습니다.




<br><br>

