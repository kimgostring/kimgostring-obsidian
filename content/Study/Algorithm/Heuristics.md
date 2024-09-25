#Algorithm 

## Bounding/Promising Fn
- 📌 `g + h` compare `BEST`
	- **g**: 현재까지 계산된 값, 명확
	- **h**: heuristic, 추정치
### H-value = 예측값
1. `0 < h < ∞`
2. ⭐ **보수적 = 실수 가능성 0 = 정답 보장**
	1. **최댓값** 구할 때 = **Over**estimate + `BEST` init 0
		- `0 < h* < h < ∞`
		- `g + h(최대한 크게) < BEST`일 때 가지치기
			- **과장해서 예측했는데도 `BEST`보다 작다? = 절대 최댓값 X** = 볼 가치 X
	2. 최솟값 = Underestimate + `BEST` init ∞
		- `0 < h < h* < ∞`
		- `g + h(최대한 작게) > BEST`일 때 가지치기
## 특징
- [[Brute Force]] + **Bounding/Promising fn**
	- `T(n) = 상태공간/탐색공간 tree` = BF tree와 동일
	- exp T.C 
	- 항상 optimal **!= efficient**
	- ⚠️ **`visited`는 promising의 조건 중 하나**일 뿐, 이를 확장시켜 다른 조건에 어긋나는지 비교하거나 지금까지의 최적값과 비교하여 pruning 가능
		- `if(visited[node])` 대신 `if(promising(value1, ...))` 처럼 구현한 뒤, `promising` 함수 따로 떼 구현 
			- `promising` 로직에서 계산할지, bounding에 쓸 값을 미리 기록해둘지는 선택 사항
		- DFS, [[Backtracking]] ➡️ 재귀 호출 전 bounding에 쓰이는 값을 세팅하고, 호출 후 (값이 덮어쓰여지지 않는 경우) 값 복원 
			- 재귀 함수 호출 한 번 = 상태 공간 트리의 node 하나 
			- 객체가 아닐 경우, 그냥 재귀 함수에 값으로 넘겨주면 됨 (객체 프로퍼티처럼)
			![[백트래킹 예시.png]]
		- BFS ➡️ 상태 공간 트리의 한 노드를 하나의 객체로 선언하여 queue에 push, 배열 또는 객체 프로퍼티로 bounding에 쓰이는 값 세팅
			- bounding에 쓰이는 값이 **배열일 경우 복사본 전달**해야 함
- 이론적 T.C가 아닌 **실제 runtime** 개선
	- ⭐ 얼마나 **가지치기** 되는지? (by **Bounding/Promising fn**)
	- Performance gain = `# 탐색 node / # 전체 node`
	- Tree Design ➡️ **깊이 vs 넓이 (Branch Factor)**
		- [[DFS]] = [[Backtracking]] vs [[BFS]] = [[Branch and Bound]]
## 상황
[[Brute Force#상황]]과 동일
## 비교
|        | [[Backtracking]]                                        | [[Branch and Bound]]         | B&B with Best-First                      |
| ------ | ------------------------------------------------------- | ---------------------------- | ---------------------------------------- |
| **정의** | DFS + bounding fn                                       | BFS + bounding fn            | B&B + **greedy**                         |
|        |                                                         |                              | fn값이 가장 최적인 node 선택 <br>+ children 확인 🔁 |
| **특징** | 한 번에 depth를 깊이 파고듦<br>= g값이 빨리 갱신됨<br>= h값의 불확실성 빠르게 감소 | root 근처에서 빠르게 <br>가지치기 가능    |                                          |
| **구현** |                                                         |                              | node 선택에 우선순위 큐(힙) 활용                    |
| **상황** | h 예측이 어려울 때<br>= 연구                                     | h 예측이 정확할 때<br>= 익숙한, 당연한 문제 |                                          |
