#Algorithm 

## 조건 = Sub-prob Overlapping
= Principle of Optimality 
= Optimal Substructure
= T.C eqn이 linear `T(n) = T(n - C) + ...`
조건 만족 시, **항상 optimal (local ➡️ global)** != efficient
## 특징
- Bottom-up
- Save & Reuse
## 로직
1. Recurrence Eqn 유도
	- example ➡️ guess
2. Table building
	1. 가장 쉬운 prob 해결 + save
	2. **이웃 위치의 이전 결과**들을 reuse = 다음 prob 해결
3. 실제 runtime 최적화 
	- 모든 값을 다 구해야 하는지? 
	- 반복 순서? 
## 구현
1. Bottom-up
	table size만한 for문 활용
2. Top-down (= Memoization)
	[[Divide & Conquer]] [[재귀]] 구현 + 이번 값 구하기 전 table check
## 상황
- Recurrence Eqn이 알려진 경우 
- !같은 결과 여러 위치에 중복 저장되는 경우 ➡️ [[Heuristics]]