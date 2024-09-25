📌 컴퓨터 프로그램이 필요하지 않은 메모리를 계속 점유하고 있는 현상
- #Java 메모리를 참조하는 포인터가 없어야 [[Memory Leak (메모리 누수)#Garbage Collector (가비지 컬렉터)]]에 의해 해제됨 
- #C #CPP [[Dynamic Allocation (동적 할당)]]된 메모리가 **해제되지 않은 상태에서 해제할 방법이 없어진** 경우
	- [[Dynamic Allocation (동적 할당)]]된 메모리를 참조하는 포인터가 사라진 경우
	- #CPP 객체 멤버일 경우, Destructor에서 명시적 해제 필요

## Garbage Collection
#Java
📌 **어떤 참조 변수도 가리키지 않는** 동적 할당된 메모리를 [[JVM (Java Virtual Machine)]]가 자동으로 모아 해제해주는 것
- 필요 없어진 객체를 가리키는 참조 변수에는 대신 `null` 대입 
- HW 발전으로 리소스가 풍부해짐 ➡ 메모리 효율을 버리고 [[Maintainability (유지보수성)]] 챙김
- 🔎 임베디드, 장기간 돌아야 하는 프로그램 

## Dangling Pointer 
#C #CPP 
📌 가리키고 있는 메모리가 해제되어 접근 불가능한 포인터 