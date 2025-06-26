---
created: 2025-02-27T22:09:23+09:00
modified: 2025-03-30T15:14:43+09:00
tags:
  - Python
---
백준 BOJ 27162 Yacht Dice 문제의 파이썬 python 풀이입니다.

https://www.acmicpc.net/problem/27162


<br><br>


## 문제
《Yacht Dice》는 여러 명이 플레이하는 주사위 게임입니다. 플레이어는 우선 주사위를 $5$개 굴립니다. 이후 원하는 주사위를 고정시킨 뒤, 남은 주사위를 다시 굴리는 일을 두 번 이하로 할 수 있습니다. 그렇게 주사위를 굴려 나온 값들의 조합으로 아래 족보에서 이전까지 선택하지 않은 하나를 선택해 점수를 기록합니다.

- **Ones**: $1$이 나온 주사위의 눈 수의 총합.
- **Twos**: $2$가 나온 주사위의 눈 수의 총합.
- **Threes**: $3$이 나온 주사위의 눈 수의 총합.
- **Fours**: $4$가 나온 주사위의 눈 수의 총합.
- **Fives**: $5$가 나온 주사위의 눈 수의 총합.
- **Sixes**: $6$이 나온 주사위의 눈 수의 총합.
- **Four of a Kind**: 동일한 주사위 눈이 $4$개 이상**이라면, 동일한 주사위 눈 $4$개의 총합. 아니라면 $0$점.
- **Full House**: 주사위 눈이 **정확히 두 종류**로 이루어져 있고 한 종류는 $3$개, 다른 종류는 $2$개일 때, 주사위 눈 $5$개의 총합. 아니라면 $0$점.
- **Little Straight**: 주사위 눈이 $1$, $2$, $3$, $4$, $5$의 조합이라면, 즉 $1$에서 $5$까지의 눈이 한 번씩 등장했다면 $30$점, 아니라면 $0$점.
- **Big Straight**: 주사위 눈이 $2$, $3$, $4$, $5$, $6$의 조합이라면, 즉 $2$에서 $6$까지의 눈이 한 번씩 등장했다면 $30$점, 아니라면 $0$점.
- **Yacht**: 동일한 주사위 눈이 $5$개라면 $50$점, 아니라면 $0$점.
- **Choice**: 모든 주사위 눈의 총합.

모든 플레이어의 모든 족보가 사용되면 게임이 끝납니다.

지금은 한별이 차례입니다. 한별이는 첫 번째로 굴린 주사위의 조합에서 세 개의 주사위를 고정하고 나머지 두 개의 주사위를 다시 굴려야 하는 상황입니다. 이 상황에서 나올 수 있는 점수의 최댓값은 얼마일까요?
<br><br><br>



## 입력

첫 번째 줄에는 이미 선택한 족보를 의미하는 $12$글자의 문자열이 주어집니다. 문자열의 모든 글자는 `Y` 또는 `N`이며, 글자들 중 적어도 하나는 `Y`입니다.

-  $1$번째 글자가 `Y`라면 **Ones**를 선택할 수 있습니다. 마찬가지로 $6$번째 글자까지 같은 규칙이 적용되어, $6$번째 글자가 `Y`라면 **Sixes**를 선택할 수 있습니다.
-  $7$번째 글자가 `Y`라면 **Four of a Kind**를 선택할 수 있습니다.
-  $8$번째 글자가 `Y`라면 **Full House**를 선택할 수 있습니다.
-  $9$번째 글자가 `Y`라면 **Little Straight**를 선택할 수 있습니다.
-  $10$번째 글자가 `Y`라면 **Big Straight**를 선택할 수 있습니다.
-  $11$번째 글자가 `Y`라면 **Yacht**를 선택할 수 있습니다.
-  $12$번째 글자가 `Y`라면 **Choice**를 선택할 수 있습니다.

각각의 경우에서 글자가 `N`이라면 해당하는 족보를 이미 선택하여 다시 선택할 수 없음을 의미합니다.

두 번째 줄에는 한별이가 첫 번째로 굴린 조합에서 고정한 주사위들의 눈의 수를 나타내는 $1$과 $6$ 사이의 정수 $3$개가 공백으로 구분되어 주어집니다.
<br><br><br>


## 출력

한별이가 얻을 수 있는 점수의 최댓값을 출력합니다.

<br><br><br>


## 풀이

### compute_score 함수

이 함수는 주사위 조합과 선택한 족보에 따라 점수를 계산합니다.

```python
def compute_score(dice, category_idx):
````
- `dice`: 5개의 주사위 눈을 담은 리스트
- `category_idx`: 0-11 사이의 정수로, 선택한 족보를 나타냄

<br>

함수는 각 족보 유형에 따라 다른 로직을 적용합니다.

1. **Ones부터 Sixes까지 (인덱스 0-5)**
```python
if 0 <= category_idx <= 5:  # Ones to Sixes
    target = category_idx + 1
    return sum(d for d in dice if d == target)
```
-  `category_idx + 1`이 찾고자 하는 주사위 눈 값
- 해당 값과 일치하는 주사위 눈들의 합을 반환
<br>

2. **Four of a Kind (인덱스 6)**
```python
elif category_idx == 6:  # Four of a Kind
    for value in range(1, 7):
        if dice.count(value) >= 4:
            return value * 4
    return 0
```
- 같은 눈이 4개 이상인 경우, 그 눈 × 4를 반환
- 없으면 0점을 반환
<br>

3. **Full House (인덱스 7)**
```python
elif category_idx == 7:  # Full House
    counts = {}
    for d in dice:
        counts[d] = counts.get(d, 0) + 1
    
    values = list(counts.values())
    if len(counts) == 2 and (3 in values and 2 in values):
        return sum(dice)
    return 0
```
- 주사위 눈의 등장 횟수를 딕셔너리에 저장
- 딕셔너리의 길이가 2(두 종류의 눈만 있음)이고, 한 종류는 3번, 다른 종류는 2번 등장한다면 모든 주사위의 합을 반환
- 조건을 만족하지 않으면, 0점을 반환

<br>

4. **Little Straight (인덱스 8)**
```python
elif category_idx == 8:  # Little Straight
    if sorted(dice) == [1, 2, 3, 4, 5]:
        return 30
    return 0
```
- 주사위 눈이 1부터 5까지 모두 있으면 30점을 반환
- 아니면 0점을 반환

<br>

5. **Big Straight (인덱스 9)**
```python
elif category_idx == 9:  # Big Straight
    if sorted(dice) == [2, 3, 4, 5, 6]:
        return 30
    return 0
```
- 주사위 눈이 2부터 6까지 모두 있으면 30점을 반환
- 아니면 0점을 반환

<br>

6. **Yacht (인덱스 10)**
```python
elif category_idx == 10:  # Yacht
    if len(set(dice)) == 1:
        return 50
    return 0

```
- 모든 주사위 눈이 같으면(집합의 크기가 1이면) 50점을 반환
- 아니면 0점을 반환

<br>

**Choice (인덱스 11)**:
```python
elif category_idx == 11:  # Choice
    return sum(dice)
```
- 모든 주사위 눈의 합을 반환

<br>
<br>


### 시간 복잡도 분석
- 남은 두 주사위를 굴리는 경우의 수: 6 × 6 = 36
- 각 경우마다 최대 12개의 족보를 확인: 36 × 12 = 432
- 각 족보마다 점수 계산은 O(1) 또는 O(n) 시간이 소요됨 (여기서 n은 주사위 개수 5, 상수 취급 가능)
- 따라서 전체 시간 복잡도는 O(36 × 12 × 5) = O(2160) = O(1)


<br>
<br>

### 예제 검증
```python

def test_examples():
    # 예제 1
    example1_available = "YYYYYYYYYYYN"
    example1_fixed = [1, 5, 6]
    
    available_categories1 = [i for i in range(12) if example1_available[i] == 'Y']
    max_score1 = 0
    best_dice1 = []
    best_category1 = -1
    
    for d1 in range(1, 7):
        for d2 in range(1, 7):
            current_dice = example1_fixed.copy() + [d1, d2]
            for category in available_categories1:
                score = compute_score(current_dice, category)
                if score > max_score1:
                    max_score1 = score
                    best_dice1 = current_dice
                    best_category1 = category
    
    print(f"예제 1 최대 점수: {max_score1}")
    print(f"최적 주사위: {best_dice1}, 사용 족보: {best_category1}")

# 예제 테스트
test_examples()

```

<br><br>

### 전체 코드

```python
# 25204 문자열 정렬

def compute_score(dice, category_idx):
    if 0 <= category_idx <= 5:  # Ones to Sixes
        target = category_idx + 1
        return sum(d for d in dice if d == target)
    
    elif category_idx == 6:  # Four of a Kind
        for value in range(1, 7):
            if dice.count(value) >= 4:
                return value * 4
        return 0
    
    elif category_idx == 7:  # Full House
        counts = {}
        for d in dice:
            counts[d] = counts.get(d, 0) + 1
        
        values = list(counts.values())
        if len(counts) == 2 and (3 in values and 2 in values):
            return sum(dice)
        return 0
    
    elif category_idx == 8:  # Little Straight
        if sorted(dice) == [1, 2, 3, 4, 5]:
            return 30
        return 0
    
    elif category_idx == 9:  # Big Straight
        if sorted(dice) == [2, 3, 4, 5, 6]:
            return 30
        return 0
    
    elif category_idx == 10:  # Yacht
        if len(set(dice)) == 1:
            return 50
        return 0
    
    elif category_idx == 11:  # Choice
        return sum(dice)
    
    return 0

def solve():
    # 선택 가능한 족보 읽기
    available = input().strip()
    available_categories = [i for i in range(12) if available[i] == 'Y']
    # Y가 있는 위치의 인덱스를 리스트에 저장
    
    # 고정된 주사위 읽기
    fixed_dice = list(map(int, input().strip().split()))
    
    max_score = 0
    
    # 남은 두 주사위의 모든 경우의 수 시뮬레이션 (1-6, 1-6) => 36가지 모두 확인
    for d1 in range(1, 7):
        for d2 in range(1, 7):
            current_dice = fixed_dice.copy() + [d1, d2]
            
            # 각 선택 가능한 족보에 대한 점수 계산
            for category in available_categories:
                score = compute_score(current_dice, category)
                max_score = max(max_score, score)
    
    return max_score




print(solve())

```


<br>
<br><br>

