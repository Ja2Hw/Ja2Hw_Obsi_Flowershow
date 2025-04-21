---
created: 2025-04-21T21:34:45+09:00
modified: 2025-04-21T23:15:19+09:00
---
각 그래프의 각 정점을 방문하는 그래프 순회(Graph Traversals)에는 크게 **깊이 우선 탐색(Depth-First Search)** 와 **너비 우선 탐색(Breadth-First Search)** 의 두 가지 알고리즘이 있습니다.

- DFS는 주로 스택이나 재귀로 구현하고, 백트래킹을 통해 효용이 뛰어납니다.
	- 코딩 테스트 대부분의 그래프 탐색은 DFS로 구현하게 된다고 합니다.
- BFS는 주로 큐로 구현하며, 그래프의 최단 경로를 구하는 문제에 사용됩니다.
	- 다익스트라 알고리즘 등

<br>

#### 탐색할 그래프의 예시 형태

```python
graph = {
    1: [2, 3, 4],
    2: [5],
    3: [5],
    4: [],
    5: [6, 7],
    6: [],
    7: [3],
}

```

<br><br>

---
<br>

# DFS (깊이 우선 탐색)
## 재귀 구조 DFS

위키피디아의 수도코드에는, 정점 v의 모든 인접 유향 간선들을 반복하라고 표기되어 있습니다. 이를 파이썬 코드로 구현해보면 다음과 같습니다.

```python
def recursive_dfs(v, discovered=[]):
	discovered.append(v) # v를 방문 표시
	for w in graph[v]: # 노드 v와 연결된 이웃 노드들을 순회하면서 방문하지 않은 노드들이 있는지 확인
			if w not in discovered:
				discovered = recursive_dfs(w, discovered)
	return discovered

```

- 방문했던 정점(discovered)를 계속 누적된 결과로 만들기 위해 리턴하는 형태만 받아오도록 처리했습니다.
- 한 노드를 방문하면 그 노드의 이웃 중 아직 방문하지 않은 노드를 계속해서 들어가면서 탐색하고, 더 이상 갈 수 없을 때 다시 돌아오는 구조입니다.

위 코드에서 주의할 점은 `discovered=[]` 기본 인자인데, 파이썬에서 기본 인자로 **변경 가능한 객체(리스트나 딕셔너리 등)** 를 설정하면, 의도치 않게 함수 호출 간 값이 공유될 수 있어 문제가 발생할 수 있습니다. 그렇게 때문에 아래와 같은 방식으로 사용하면 문제를 예방할 수 있습니다.

```python
def recursive_dfs(v, discovered=None):
    if discovered is None:
        discovered = []
    discovered.append(v)
    for w in graph[v]:
        if w not in discovered:
            recursive_dfs(w, discovered)
    
    return discovered

```

<br>

## 스택을 이용한 반복 구조 DFS

```python
def iterative_dfs(start_v):
    discovered = []
    stack = [start_v] # 탐색 시작할 노드, 초기 스택에는 시작 노드만 넣음
    while stack: # 스택이 비어있지 않다면 반복
        v = stack.pop() # 스택에서 하나 꺼내서 v로 저장
        if v not in discovered:
            discovered.append(v) # 방문하지 않은 v라면 방문 처리
            for w in graph[v]:
                stack.append(w) # 인접 노드들을 모두 스택에 추가
    
    return discovered
```

- 실행 속도는 재귀에 비해 더 빠른 편입니다. 
- 앞선 재귀 DFS는 사전식 순서로 방문했는데, 반복 DFS는 역순으로 방문했습니다.
	- 스택으로 구현하다 보니 가장 마지막에 삽입된 노드부터 꺼내서 반복하게 되고 이 경우 인접 노드에 가장 최근에 담긴 노드, 즉 가장 마지막부터 방문하기 때문입니다.

<br><br>

---

<br>

# BFS (너비 우선 탐색)

## 큐를 이용한 반복 구조 BFS

스택을 이용하는 DFS와 달리 BFS는 큐를 이용합니다.
모든 인접 간선을 추출하고 도착점인 정점을 큐에 삽입하는 구조입니다.

```python
def iterative_bfs(start_v):
    discovered = [start_v] # 시작 노드는 방문했으므로 넣고 시작
    queue = [start_v]
    while queue: # 큐가 빌 때까지 반복
        v = queue.pop(0)
        for w in graph[v]:
            if w not in discovered:
                discovered.append(w)  # 방문하지 않은 w라면 방문 처리
                queue.append(w)       # 인접 노드를 큐에 추가
    return discovered

```

최적화를 위해 데크 같은 자료형을 사용하는 것도 고민해볼 수 있겠습니다.

<br>
## 재귀를 이용한 BFS?

- ***BFS는 재귀로 동작하지 않습니다.***
- 큐를 활용한 반복 구조로만 구현이 가능하기 때문에, 코딩 테스트 풀이에서 혼동해서는 안 되겠습니.

<br>

# 스택 버전 DFS와 큐 버전 BFS의 차이

```python
# DFS: Stack을 사용한 반복 깊이 우선 탐색
def iterative_dfs(start_v):
    discovered = []
    stack = [start_v]
    while stack:
        v = stack.pop()                      # ✅ 마지막에 넣은 노드를 꺼냄
        if v not in discovered:
            discovered.append(v)             # 방문 처리
            for w in graph[v]:
                stack.append(w)              # 인접 노드를 스택에 추가
    return discovered

```

```python
# BFS: Queue를 사용한 반복 너비 우선 탐색
def iterative_bfs(start_v):
    discovered = [start_v]
    queue = [start_v]
    while queue:
        v = queue.pop(0)                     # ✅ 처음에 넣은 노드를 꺼냄
        for w in graph[v]:
            if w not in discovered:
                discovered.append(w)         # 방문 처리
                queue.append(w)              # 인접 노드를 큐에 추가
    return discovered

```

| 항목           | DFS (Stack)                         | BFS (Queue)                         |
| ------------ | ----------------------------------- | ----------------------------------- |
| **자료구조**     | `stack = [start_v]`                 | `queue = [start_v]`                 |
| **꺼내는 방식**   | `v = stack.pop()` → 마지막 요소          | `v = queue.pop(0)` → 첫 번째 요소        |
| **넣는 대상**    | 인접 노드 `w`를 **뒤에서부터** 스택에 쌓음         | 인접 노드 `w`를 **순서대로** 큐에 넣음           |
| **탐색 순서**    | 깊게, 깊게 들어감 → 끝까지 갔다가 되돌아옴           | 넓게, 가까운 노드부터 모두 보고 나중에 멀리 감         |
| **방문 순서 예시** | `1 → 4 → 3 → 5 → 7 → 6 → 2` (깊이 우선) | `1 → 2 → 3 → 4 → 5 → 6 → 7` (너비 우선) |
- **DFS는 "가장 최근에 추가된 노드"부터 탐색**  
    → 스택(LIFO)이기 때문
- **BFS는 "가장 먼저 추가된 노드"부터 탐색**  
    → 큐(FIFO)이기 때문

그래서 코드는 **거의 똑같은 구조**지만,  **pop의 위치 차이(끝 vs 앞)** 때문에 완전히 다른 탐색 결과가 나오게 됩니다.


# 백트래킹

**백트래킹(Backtracking)** 은 해결책에 대한 후보를 구축해 나아가다 가능성이 없다고 판단되는 즉시 후보를 포기하고 정답을 찾아 돌아가는 범용적인 알고리즘으로, `제약 충족 문제(Constraint Satisfaction Problems)`에 특히 유용합니다.
- 백트래킹은 DFS와 같은 방식으로 탐색하는 모든 방법을 뜻하며, **DFS(깊이 우선 탐색)** 는 백트래킹의 골격을 이루는 알고리즘입니다.
- 백트래킹은 주로 **재귀** 로 구현하며, 모두 DFS의 범주에 속합니다.
- 브루트 포스와 유사하지만, 가능성이 없는 경우 후보를 포기(트리의 가지치기, Prunning)한다는 점에서 좀 더 효율적이라고 할 수 있겠습니다.