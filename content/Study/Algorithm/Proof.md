#Algorithm 

## Induction (귀납법)
### Step
1. **Base**: `P(최솟값)`이 True임을 보이기
2. **Inductive step**: 모든 `n >= 최솟값`에 대해, `P(n) => P(n + 1)` 보이기
	1. **가설 (Hyperthesis)**: `P(n)이 True`라고 가정
	2. **유도**: `P(n)`으로부터 `P(n + 1)` 유도
		이 소정리(Lemma) 증명 과정에서 Contradiction 많이 쓰임
### 참고
- Base와 Inductive step 연결 부분 = **edge case** 주의 
	- `...`로 퉁치지 X
- 유도 어려우면, 문제 더 general하게 바꾸기
	- 더 powerful한 도구들을 사용할 수 있음
## Contradiction (귀류법)
📌 P가 True임을 증명하기 위해, `!P가 True`라는 가설 세우고 **모순** 모이기
## p => q
| p     | q     | p => q |
| ----- | ----- | ------ |
| T     | T     | T      |
| **T** | **F** | **F**  |
| F     | T     | T      |
| F     | F     | T      |
