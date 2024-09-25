#Algorithm #Sort 

[바킹독 > 정렬 2](https://blog.encrypted.gg/966)
📌 원소를 k진수로 취급하며, 1의 자릿수부터 d번째 자릿수까지 각 자릿수에 대해 0~k을 범위로 Counting Sort를 수행하는 정렬 방법
![[Radix Sort.png]]
![[Radix Sort 2.png]]
- [[Merge Sort#Stable Sort]] 성질 이용, d차 정렬 기준인 1의 자릿수에 대해 먼저 정렬하고 d-1차 정렬 기준인 k^1의 자릿수에 대해 정렬하고... 🔃 
- ⏰ O(d * (n + k)) ~= O(dn) (k는 무시 가능할 정도로 작음)
- 🏚 (배열 구현) k개 수에 대해 크기 n인 배열이 필요하므로 O(nk)
	- 심한 공간 낭비 ➡ 동적 배열 또는 연결 리스트를 쓰는 것이 좋음 
	- 실제 코딩 테스트에서 구현할 일 없음
	↔ Counting Sort, k개 수에 대해 각 수의 개수만 세면 되므로 O(k), 1차원 배열만으로 해결 가능 

