---
created: 2025-02-27T22:09:23+09:00
modified: 2025-04-22T02:12:20+09:00
tags:
  - Python
---
리트코드 LeetCode `17 Letter Combinations of a Phone Number` 문제의 파이썬 python 풀이입니다.

https://leetcode.com/problems/letter-combinations-of-a-phone-number/description/


<br><br>


## 문제

2에서 9까지 숫자가 주어졌을 때 전화 번호로 조합 가능한 모든 문자를 출력하라.

<br><br>

## 풀이



```python
# 폰 번호 문자 조합 : 모든 조합 탐색 (DFS)

class Solution:
    def letterCombinations(self, digits: str) -> List[str]:

        def dfs(index, path):
            # 끝까지 탐색하면 백트래킹
            if len(path) == len(digits):
                result.append(path)
                return

            # 입력값 자릿수 단위 반복
            for i in range(index, len(digits)):
                # 숫자에 해당하는 모든 문자열 반복
                for j in dic[digits[i]]:
                    dfs(i+1, path+j) #path에 digits[i]을 더해서 가져감

        # 예외 처리
        if not digits: # 아무것도 없을 때
            return []

        dic = {"2": "abc", "3": "def", "4": "ghi", "5": "jkl",
               "6": "mno", "7": "pqrs", "8": "tuv", "9": "wxyz"}

        result = []

        dfs(0, "")

        return result

```

<br>

```python
def dfs(index, path):
```

- `index`: 현재 몇 번째 숫자를 처리 중인지 나타냄
- `path`: 지금까지 조합한 문자들을 저장하는 문자열
    예를 들어, `'2' → abc`, `'3' → def`이면
    - 첫 번째 단계에서 `'a'`를 선택 → `path = 'a'`
    - 두 번째 단계에서 `'d'`를 선택 → `path = 'ad'`
    - 끝까지 조합이 완료되면 → `result.append('ad')`

즉, `path`는 **현재 만들고 있는 조합 중간 결과**입니다.
  
<br>

### 예시

입력: `digits = "23"`  
딕셔너리:
```python
dic = {"2": "abc", "3": "def"}
```

#### DFS 흐름
```scss
dfs(0, ""):
  ├─ dfs(1, "a"):
  │   ├─ dfs(2, "ad") → 끝 → 저장
  │   ├─ dfs(2, "ae") → 끝 → 저장
  │   └─ dfs(2, "af") → 끝 → 저장
  ├─ dfs(1, "b"):
  │   ├─ dfs(2, "bd") → 끝 → 저장
  │   ├─ dfs(2, "be") → 끝 → 저장
  │   └─ dfs(2, "bf") → 끝 → 저장
  ├─ dfs(1, "c"):
      ├─ dfs(2, "cd") → 끝 → 저장
      ├─ dfs(2, "ce") → 끝 → 저장
      └─ dfs(2, "cf") → 끝 → 저장

```

<br>

재귀 호출을 하면서 이 문자열을 점점 길게 만들어 가다가, `digits` 길이와 같아지면 결과에 저장합니다.
<br><br>