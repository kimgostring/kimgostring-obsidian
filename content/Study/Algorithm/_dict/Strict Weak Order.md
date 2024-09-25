#Sort 

[정렬 비교함수의 조건](https://www.acmicpc.net/board/view/61621)
📌 정렬에 사용하는 비교함수가 만족해야 하는 4가지 성질
1. **Irreflexivity (비반사성)**: compare(a, a) = false
2. **Asymmetry (비대칭성)**: compare(a, b) = true => compare(b, a) = false
3. **Transitivity (전이성)**: compare(a, b) = true && compare(b, c) = true => compare(a, c) = true
4. **Transitivity of Equivalence (상등 관계의 전이성)**: compare(a, b) = false && compare(b, a) = false => a == b