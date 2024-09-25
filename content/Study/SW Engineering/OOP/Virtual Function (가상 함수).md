 #CPP 
 [TCP School > 가상 함수](https://www.tcpschool.com/cpp/cpp_polymorphism_virtual)
 
 📌 Child 클래스에서 재정의가 가능한 멤버 함수
- **문법**: 함수 정의 앞에 `virtual` 키워드를 붙여 선언 (`virtual 함수원형/정의`)
- Parent 클래스 타입의 포인터로 참조하여 가상 함수 호출 ➡️ [[Binding#Dynamic Binding (동적 바인딩)]]
	- 그 외의 멤버 함수는 모두 [[Binding#Static Binding (정적 바인딩)]]
- **가상 함수 테이블 (VTbl)**: 선언된 가상 함수들의 주소를 관리하는 테이블 
	- 가상 함수를 하나라도 가지는 클래스의 경우, 각 객체마다 숨겨진 멤버 변수를 통해 가상 함수 테이블을 가리킴
	- 컴파일러 대부분의 구현 방식

## 순수 가상 함수 
[TCP School > 순수 가상 함수](https://www.tcpschool.com/cpp/cpp_polymorphism_abstract)
📌 **반드시 재정의가 필요**한 멤버 함수
- **문법**: **구현이 없는** 함수 원형 앞에 `virtual` 키워드를 붙여 선언 (`virtual 함수원형=0;`)
- 멤버 함수가 하나라도 순수 가상 함수일 경우 ➡️ [[Abstract Class (추상 클래스)]]