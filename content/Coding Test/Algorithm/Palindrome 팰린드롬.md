---
created: 2024-12-10T15:50:55+09:00
modified: 2025-01-17T11:40:22+09:00
---
# Palindrome 팰린드롬
[백준 1259 팰린드롬수](https://www.acmicpc.net/problem/1259)
<br>

- 앞에서부터 읽으나 귀에서부터 읽으나 같게 읽히는 문자열
	- 기러기, 토마토, ...
	- aba, abcdcba, ...
- 최악의 경우
	- 원본 문자열의 길이만큼 대칭
		- 원본 문자열 : "abab"
			- "abab <font color="#d83931">|</font> abab" 인 경우 최악
- 최선의 경우
	- 문자열 맨 앞 글자부터 최대한 적은 길이만큼 대칭
		- 원본 문자열 : "abab"
			- "a <font color="#d83931">|</font> bab <font color="#d83931">|</font> a" 인 경우 최선이라 볼 수 있음

`abab`의 경우에는 `bab` 를 대칭에 해당하는 부분이라고 분류하고, a를 추가하는 방식으로 구현할 수 있다.

```python
import sys
s = sys.stdin.readline().strip()

for i in range(len(s)):
  if s[i:] == s[i:][::-1]:
    print(len(s)+i)
    break
```

<br>