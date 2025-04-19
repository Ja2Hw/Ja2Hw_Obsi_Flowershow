---
created: 2025-04-19T23:25:04+09:00
modified: 2025-04-19T23:27:47+09:00
---
# 딕셔너리 Dictionary

<br>

## 딕셔너리 모듈

### defaultdict 객체
- `defaultdict` 객체는 존재하지 않는 키를 조회할 경우, 에러 메시지를 출력하는 대신 디폴트 값을 기준으로 해당 키에 대한 딕셔너리 아이템을 생성해줍니다.
- 실제로는 `collections.defaultdict` 클래스를 갖습니다.

```python
a = collections.defaultdict(int)
a['A'] = 5
a['B'] = 4
a['C'] += 1
a
```

출력 결과
```bash
defaultdict(<class 'int'>, {'A' : 5, 'B' : 4, 'C' : 1})
```

디폴트 값 0을 기준으로 자동 생성되어, `KeyError` 없이 만들어집니다.

<br><br>
### Counter 객체
[[content/Python/Counter.md|Counter]]

<br><br>

### OrderedDict 객체

<br><br>
