#Algorithm #Sort 

## In-Place Sort
[바킹독 > 정렬](https://blog.encrypted.gg/955)
📌 추가적인 공간을 사용하지 않는 정렬
- ❤️ **장점**
	- 메모리 절약 
	- 높은 Cache Hit Rate ⬅ 한 배열 안에서의 자리 바꿈만으로 처리 가능 

## Intro Sort, Introspective Sort (내성 정렬)
[c++ STL sort 내성 정렬 알고리즘](https://choiseokwon.tistory.com/209#google_vignette)
📌 Quick Sort의 장점을 유지하면서, 최악의 시나리오 O(`n^2`) (= 이미 오름/내림차순 정렬되어 항상 배열이 극단적으로 나쁘게 나뉘는 상황)을 해결하기 위해 만들어진 하이브리드 알고리즘 
= Quick Sort + Heap Sort + Insertion Sort
```pseudo
// Intro Sort 주요 특징
/*
	1. 크기가 작은 (= 16) 파티션의 경우 Insertion Sort
	2. Quick Sort를 재귀적으로 반복하다 Max Recursion Depth (= log(len(A))) 도달하면 Heap Sort
	3. Quick Sort의 경우, 파티션의 처음/중간/끝 원소 중 중간값(median)을 Pivot으로 선택
*/
 
procedure sort(A):
    maxdepth ← ⌊log(length(A))⌋ × 2
    introsort(A, maxdepth)

procedure introsort(A, maxdepth):
    n ← length(A)
    size ← endIdx - startIdx
    if n ≤ 1:
        return  // base case
    else if maxdepth = 0:
        heapsort(A)
    else if size < 16:
        insertsort(A)
    else:
	    // pivot
        p ← partition(A)  
        introsort(A[0:p-1], maxdepth - 1)
        introsort(A[p+1:n], maxdepth - 1)
```
- [[#In-Place Sort]], **Not [[Merge Sort#Stable Sort]]**
- ⏰ (최악의 경우에도) `nlogn` 
	- `n`이 충분히 작을 경우, `n^2`이 `nlogn`보다 빨리 동작하므로 Insertion Sort를 사용
- 🏚 `logn`