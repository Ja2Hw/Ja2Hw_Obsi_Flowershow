---
created: 2025-03-03T20:10:52+09:00
modified: 2025-03-03T20:10:52+09:00
---

# 파이썬의 math 모듈

https://docs.python.org/ko/3.13/library/math.html

```python
import math
```

<br>
## 1. 상수(constants)

- **math.pi**  
    원주율 π 값을 제공 (약 3.141592653589793)
- **math.e**  
    자연상수 e 값을 제공 (약 2.718281828459045)

이런 상수들을 사용할 때는 `math.pi` 등으로 사용

```python
import math

r = 5
circle_area = math.pi * (r**2)
print(circle_area)  # 원의 넓이
```
<br>

---

## 2. 자주 쓰는 함수들

### (1) 제곱근과 지수, 로그

- **math.sqrt(x)**  
    x\sqrt{x}x​ (제곱근)을 반환
- **math.pow(x, y)**  
    x^y 을 계산하여 반환 (거듭제곱). 
    ※ 파이썬에서 거듭제곱은 `x**y` 로도 가능
- **math.log(x)**  
    자연로그 ln⁡(x)을 반환
- **math.log2(x)**  
    log⁡2(x) (밑이 2인 로그)를 반환
- **math.log10(x)**  
    log⁡10(x) (밑이 10인 로그)를 반환

```python
import math

val = 16

sqrt_val = math.sqrt(val)      # sqrt(16) = 4
log_val = math.log(val)        # ln(16) = 2.772588...
log2_val = math.log2(val)      # log2(16) = 4
log10_val = math.log10(val)    # log10(16) = 1.204119...

```
<br>

---

### (2) 올림, 내림, 반올림

- **math.floor(x)**  
    실수 `x`를 **내림**(버림)한 정수를 반환 ( ⌊x⌋ )
- **math.ceil(x)**  
    실수 `x`를 **올림**(반올림이 아니라 무조건 위로)을 한 정수를 반환 ( ⌈x⌉ )
- **round(x)**  
    파이썬 내장 함수로 반올림을 할 때 사용 (소수점 첫째 자리에서 반올림).
    `math` 모듈의 함수는 아니지만 자주 헷갈리기에 함께 언급

```python
import math

num = 3.7

print(math.floor(num))  # 3
print(math.ceil(num))   # 4
print(round(num))       # 4

```

<br>

---

### (3) 최소공배수, 최대공약수

- **math.gcd(a, b)**  
    두 수 aaa와 bbb의 최대공약수(gcd⁡(a,b)\gcd(a, b)gcd(a,b))를 구해 정수로 반환
- **math.lcm(a, b)**  
    두 수 aaa와 bbb의 최소공배수(lcm⁡(a,b)\operatorname{lcm}(a, b)lcm(a,b))를 구해 정수로 반환
    (파이썬 3.9 이상에서 지원)

```python
import math

print(math.gcd(16, 24))  # 8
print(math.lcm(16, 24))  # 48 (Python 3.9+)

```

<br>

---

### (4) 팩토리얼

- **math.factorial(n)**  
    n!을 정수로 반환


```python
import math

print(math.factorial(5))  # 120


```

<br>

---

### (5) 조합(comb)

- **math.comb(n, k)**  
    조합 (nk)\binom{n}{k}(kn​) ( nnn개 중에서 kkk개를 순서 없이 고르는 방법의 수 ) 을 정수로 반환
![[Pasted image 20250303201906.png]]

```python
import math

n = 5
k = 2
print(math.comb(n, k))  # 10

```
<br>

---

### (6) 기타 유용한 함수들

- **math.fabs(x)**  
    절댓값(부동소수점)을 반환합니다. 내장함수 `abs()`와 비슷하지만, 리턴 타입이 float인 점이 특징
- **math.trunc(x)**  
    `x`를 정수 부분만 남기고 소수점 이하를 잘라내어 반환(0 방향으로 버림)
- **math.hypot(x, y)**  
    직각삼각형에서 빗변의 길이를 구함 -> 2차원 거리 계산에도 유용

```python
import math

print(math.fabs(-3.7))   # 3.7
print(math.trunc(3.7))   # 3
print(math.hypot(3, 4))  # 5.0 (3-4-5 삼각형)

```

<br>
