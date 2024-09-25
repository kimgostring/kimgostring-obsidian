#Algorithm 

## 조건 = size 줄어드는 speed
= **D&C recurrence eqn**: `T(n) = CT(n / C) + ...`
- D&C가 poly 
↔️ **Linear recurrence eqn**: `T(n) = T(n - C) + ...`
- D&C가 exp (해결 불가)
= **Overlapping sub-prob 존재** ➡️ [[Dynamic Programming]]
## 특징
- Top-down
## Step
1. **Divide**: 2개 이상 sub-prob로 쪼개기 
2. **Conquer**: 각 prob 해결 
3. (Optional) Merge
	- Divide를 열심히 할지 or 나중에 Merge할지 선택
## 구현
1. **Recursive**: sub-prob에 대해 자신을 호출하는 [[재귀]] 함수 
	- ⭐ 종료 조건 (= 충분히 작은 sub-prob)
	- 💔 stack call, memory, speed
		- 적당한 size일 때 재귀 멈추는 게 좋을 수도 🔎[[Parallelism for Sum#Cutoff]]
2. **Iterative**: for문
