#Algorithm #Sort

## 활용
- [[SQL#Merge Join]] in Database
- [[Parallelism for Sum]] in Multicore 

## k-way
[k way merge sort : 내부 정렬을 할 때 일반적인 병합 정렬보다 빠를까?](https://m.blog.naver.com/PostView.naver?blogId=chogahui05&logNo=221543302703&categoryNo=6&proxyReferer=)
- [[Heap]]을 쓰게 되면 **T.C가 2-way와 동일**
> 	우리가 흔히 알고 있는 **2 way랑 복잡도가 같아요**. 심지어 밑도 같고요. optimize를 위해서, 원본 배열과, 임시 배열 말고도 **추가적인 자료구조가 필요한 것은 덤**이고요. 내부 정렬을 하는 경우에는 굳이 쓸 이유는 없어 보입니다. 한 가지 의문이 남습니다. 그런데 왜 쓸까요? **k가 커질수록 호출 깊이가 작아집니다.** 배열을 읽는 것에 비해서, 파일에서 읽어들이는 것은 매우 무겁습니다. 그러면 뎁스가 큰 것에 비해서 작은 경우에 어떠한 이점이 있을 듯 싶습니다. 어떤 이점이 있을까요?
- **T.C 증명**
	```
	T(n)  = divide + conquer + merge 
		= 0 + kT(n/k) + nlogk // nlogk = n(한 depth에서 merge할 때의 T.C) * logk(depth)
		= k{kT(n/k^2) + (n/k)logk} + nlogk = k^2T(n/k^2) + 2nlogk 
		= k^2{kT(n/k^3) + (n/k^2)logk} + 2nlogk = k^3T(n/k^3) + 3nlogk ... 
		= k^mT(n/k^m) + mnlogk 
	suppose k^m = n, m = log_k(n)
		= nT(n/n) + log_k(n) * n * logk 
		= n + logn/logk * n * logk 
		= n + nlogn <= O(nlogn)
	```
- ❤️ **그럼에도 depth가 작아지는 것의 장점**
	- [[Parallelism for Sum]] in Multicore ➡️ 동일 depth의 호출을 parallel하게 처리 가능

## Stable Sort
[바킹독 > 정렬](https://blog.encrypted.gg/955)
📌 우선 순위가 같은 원소들끼리는 원래의 순서를 따라가도록 하는 정렬
- **구현**: 앞쪽 배열과 뒤쪽 배열의 맨 앞의 원소를 비교하며 merge할 때, 앞쪽 배열 **<=** 뒤쪽 배열일 때 앞쪽 배열의 것을 선택
- ❤️ 이미 특정 조건 B로 정렬된 배열을 또다른 조건 A로 정렬할 때 ➡ A로 1차 정렬, B로 2차 정렬한 것 같은 결과를 얻을 수 있음 
	- but 정렬을 두 번 하는 것보다는 정렬 조건에서 A와 B를 동시에 고려하는 것이 더 좋은 방법 