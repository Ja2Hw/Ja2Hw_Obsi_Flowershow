---
created: 2025-02-27T22:09:23+09:00
modified: 2025-04-20T14:32:11+09:00
tags:
  - Python
  - "#Combinations"
---
리트코드 LeetCode `937 Reorder Data in Log Files` 문제의 파이썬 python 풀이입니다.

https://leetcode.com/problems/reorder-data-in-log-files/description/


<br><br>


## 문제

로그를 재정렬하라. 기준은 다음과 같다.
1. 로그의 가장 앞 부분은 식별자
2. 문자로 구성된 로그가 숫자 로그보다 앞에 온다
3. 식별자는 순서에 영향을 끼치지 않지만, 문자가 동일할 경우 식별자 순으로 한다
4. 숫자 로그는 입력 순서대로 한다
<br><br><br>

## **Constraints**
- `1 <= logs.length <= 100`
- `3 <= logs[i].length <= 100`
- All the tokens of `logs[i]` are separated by a **single** space.
- `logs[i]` is guaranteed to have an identifier and at least one word after the identifier.

<br>
<br><br><br>

## 풀이

[[람다 표현식]]과 + 연산자를 이용해서 작성했습니다.

```python
# 로그 파일 재정렬 : 람다와 + 연산자를 이용

class Solution:
    def reorderLogFiles(self, logs: List[str]) -> List[str]:

        letters, digits = [], []

        for log in logs: # 숫자 로그인지 문자 로그인지 확인
            if log.split()[1].isdigit(): # 식별자 뒤에 로그가 숫자라면
                digits.append(log)
            else: # 식별자 뒤에 로그가 문자라면
                letters.append(log)

        # 2개의 키를 람다 표현식으로 정렬
        letters.sort(key=lambda x: (x.split()[1:], x.split()[0]))

        return letters + digits

```

<br>

### 람다 표현식
```python

    letters.sort(key=lambda x: (x.split()[1:], x.split()[0])
    
```

`letters` 는 ["let1 art can","dig2 3 6","let2 own kit dig","let3 art zero"] 이런 형태로 되어있으니, `x.split()[0]`는 식별자, `x.split()[1:]`는 식별자 뒤의 문자 로그입니다.

문제에서 제시하기로, 식별자는 순서에 영향을 끼치지 않고 문자 순서로 정렬하되, 문자가 동일한 경우 식별자 순으로 한다고 합니다.
따라서 람다식에 2개의 키를 줍니다. 우선순위 정렬 기준으로 문자 로그, 후순위 정렬 기준으로 식별자를 주면 되겠습니다.

<br>

이후 숫자 로그는 입력 순서대로 한다고 하였으므로, digits 리스트는 그대로 이어붙이면 재정렬이 완료됩니다.
<br><br>