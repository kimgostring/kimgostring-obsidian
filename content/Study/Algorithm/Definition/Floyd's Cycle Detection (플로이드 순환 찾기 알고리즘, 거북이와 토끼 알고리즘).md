#Algorithm #LinkedList

[Algorithm-Floyds-Cycle-Detection-알고리즘이란](https://fomaios.tistory.com/entry/Algorithm-Floyds-Cycle-Detection-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98%EC%9D%B4%EB%9E%80)
📌 사이클이 있는 연결 리스트에 속도가 다른 두 개의 포인터를 루프에 진입시켜 진행하다 보면, 언젠가는 서로 같은 노드를 가리키게 된다는 알고리즘 
- 느린 포인터는 한 칸씩, 빠른 포인터는 두 칸씩 이동
- 빠른 포인터가 먼저 끝에 다다르게 될 경우, 루프가 존재하지 않음
- 두 포인터가 만난 경우, 루프가 존재함
	- 루프 시작점을 알고 싶을 경우, **느린 포인터를 Head로 이동시킨 뒤 두 포인터를 한 칸씩만 이동시켜 만나게 되는 위치** 계산 