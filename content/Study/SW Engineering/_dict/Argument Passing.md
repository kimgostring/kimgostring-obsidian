#Java #CPP 

## Pass by value
- 모든 Primitive 타입의 인수 
- #CPP **포인터(`*`), Reference(`&`)가 아닌** 모든 타입의 인수
	- 클래스 타입의 객체를 넘길 경우 자동으로 Copy Constructor가 호출되어 새 객체를 생성하여 value를 넘기게 됨
	- Copy Constructor를 정의하지 않은 경우, Default로 자동 생성됨 ([[Copy Method#Shallow Copy]])
		- 객체 자체는 새로 생성되더라도, 포인터 타입의 멤버 변수는 원본 객체의 것을 공유하게 됨 
	- ❤️ **[[Safety (안전성)]]**: 원본 객체를 수정하지 못하는 것이 보장됨 
	- 💔 **Overhead**: 객체의 생성, 삭제가 Expensive 
		- **Copy-On-Write**: **Write할 때만** 새 메모리 할당, Read만 있을 경우 기존 객체 공유/참조

## Pass by reference
📌 포인터/주소 값을 인수로 넘기는 방식
- #CPP **포인터(`*`), Reference(`&`)** 타입의 파라미터 
	- **차이점**: Reference는 가리키는 메모리가 **무조건 같은 타입의 실제 객체**, C++ Style
		- `NULL` 아님 보장 ➡ [[Safety (안전성)]]
		- 다른 타입을 가리킬 수 없음 
		↔ 포인터, 어느 타입의 값이든 가리킬 수 있음
	- 💔 **원본이 수정될 위험** 존재
		- **`함수명(타입 const 변수명)`**: `변수명`이 가리키는 원본이 수정되지 않음 보장 ➡ [[Safety (안전성)]]
			↔ `함수명(const 타입 변수명)`: `변수명`에 저장된 주소값이 수정되지 않음 
- #Java **Reference 변수**를 파라미터로
	- **Reference 변수**: Primitive 타입이 아닌 변수, **객체가 저장된 실제 [[Heap]] 공간의 주소를 저장**하고 있음 
		- 참조하는 객체가 없을 경우 `null` (!= `0`)
	- 포인터 문법 없음 ➡ [[Study/SW Engineering/_dict/Security (보안성)|Security (보안성)]]

## Callee ➡ Caller
1. 리턴값
2. #C #CPP 포인터, #Java Reference 