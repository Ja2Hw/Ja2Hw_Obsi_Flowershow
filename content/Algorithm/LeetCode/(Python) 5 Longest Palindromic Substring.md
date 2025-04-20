---
created: 2025-02-27T22:09:23+09:00
modified: 2025-04-20T16:50:04+09:00
tags:
  - Python
  - "#Combinations"
---
리트코드 LeetCode `5 Longest Palindromic Substring` 문제의 파이썬 python 풀이입니다.

https://leetcode.com/problems/longest-palindromic-substring/description/


<br><br>


## 문제
Given a string `s`, return _the longest_ _palindromic_ _substring_ in `s`.
<br><br><br>

## **Constraints**
- `1 <= s.length <= 1000`
- `s` consist of only digits and English letters.

<br>
<br><br><br>

## 풀이

투 포인트를 활용한 슬라이딩 윈도우 방식으로 풀어보겠습니다.
<br>

### 확장 함수 선언
```python
# 투 포인터 확장 함수 선언
        def expand(left: int, right: int) -> str:
            while left >= 0 and right < len(s) and s[left] == s[right]:
                left -= 1
                right += 1

            return s[left + 1 : right]
    
```

주어진 left와 right를 기준으로, 팰린드롬인지 확인하며 양쪽으로 확장해 나가는 함수입니다.
- 예시
	- 'abba'에서 left=1, right=2이면 → "bb"에서 "abba"까지 확장 가능
- `left + 1 : right`를 리턴하는 이유
    - while 루프가 끝날 때는 한 번 더 초과한 상태라, 올바른 구간을 잘라야 함

```
# 예시
s = "racecar"
expand(3, 3) → "racecar"  ← 가운데가 'e'인 홀수 팰린드롬
expand(2, 3) → "cec"      ← 가운데가 'ce'인 짝수 팰린드롬
```

<br>
### 슬라이딩 윈도우
```python
result = ''
for i in range(0, len(s) - 1):
    result = max(result, expand(i, i+1), expand(i, i+2), key=len)

```

- 각 문자 위치 `i`를 기준으로
    - `expand(i, i+1)` → **짝수 길이** 팰린드롬 시도 (ex: `"abba"`)
    - `expand(i, i+2)` → **홀수 길이** 팰린드롬 시도 (ex: `"racecar"`)
        - 홀수 길이 팰린드롬 조사는 expand(i, i)로 시작할 수도 있음. 하지만 어차피 한 글자 문자열이기 때문에, 세 글자 문자열부터 조사를 하는 것이 효율적
- `max(..., key=len)` → 세 후보 중에서 **가장 긴 팰린드롬 문자열을 선택**해서 `result`에 저장

#### 왜 i+1, i+2를 쓸까?
- 팰린드롬은 중심을 기준으로 좌우 대칭이기 때문에,
    - 중심이 **하나**일 수도 있고 (`expand(i, i+2)` → 홀수)
    - 중심이 **두 개**일 수도 있음 (`expand(i, i+1)` → 짝수)

그래서 둘 다 시도해 보고, **더 긴 쪽을 선택**
- `max(...)`로 현재 result보다 더 긴 팰린드롬이 나오면 업데이트함
<br><br>

### `for i in range(0, len(s) - 1)` 루프 과정

`for i in range(0, len(s) - 1)`  
→ 각 위치에서 `expand(i, i+1)` (짝수) 와 `expand(i, i+2)` (홀수) 수행

#### 🔹 i = 0
- `expand(0, 1)` → `b ≠ a` → 팰린드롬 X → return `""`
- `expand(0, 2)` → `b == b`, 그다음 `a == a` → 팰린드롬 `"bab"`  
    → 현재 result: `"bab"`

#### 🔹 i = 1
- `expand(1, 2)` → `a ≠ b` → return `""`
- `expand(1, 3)` → `a == a`, 그다음 `b == b` → 팰린드롬 `"aba"`  
    → 길이 같음. `"bab"` 또는 `"aba"` 중 하나 유지

#### 🔹 i = 2
- `expand(2, 3)` → `b ≠ a` → return `""`
- `expand(2, 4)` → `b ≠ d` → return `""`

#### 🔹 i = 3
- `expand(3, 4)` → `a ≠ d` → return `""`
- `expand(3, 5)` → 범위 벗어남 → return `""`

<br>

### 정답 코드
```python
# 가장 긴 펠린드롬 수 : 투 포인트, 슬라이딩 윈도우 방식 응용

class Solution:
    def longestPalindrome(self, s: str) -> str:

        # 투 포인터 확장 함수 선언
        def expand(left: int, right: int) -> str:
            while left >= 0 and right < len(s) and s[left] == s[right]:
                left -= 1
                right += 1

            return s[left + 1 : right]

        # 길이가 1 이하거나, 뒤집어도 똑같은 그 자체로 펠린드롬인 경우
        if len(s) < 2 or s == s[::-1]:
            return s

        result = ''
        
        # 슬라이딩 윈도우 우측 이동
        for i in range(0, len(s)-1):
            result = max(result, expand(i, i+1), expand(i, i+2), key=len)

        return result

```
