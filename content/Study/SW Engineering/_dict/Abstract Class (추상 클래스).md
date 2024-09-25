#OOP 

📌 #Java `abstract` 키워드를 붙여 선언한 클래스 (**구현되지 않은 메소드가 없을 수도** 있음)
- [[Inheritance (상속)#IS-A Relationship]]을 갖는 상황에서만 사용하고, 나머지 경우에서는 [[Interface]]를 대신 사용해야 함 
	↔ [[Interface]], #Java8 이전까지는 모든 Method가 Abstract
📌 #CPP Abstract Method가 하나 이상 존재하는 클래스

- **목적**: [[Polymorphism (다형성)]]을 가진 함수 집합 정의
- **참조**로만 사용 가능 (자식 객체들을 가리킬 수 있음)
	- 더 상위 부모일수록 가리킬 수 있는 객체의 범위 넓어짐
	- #Java `Object` 타입으로 모든 객체 인스턴스 가리킬 수 있음, 그러나 **상위 참조 변수로는 하위 자식에서 추가된 속성 사용 불가** 
- **객체 인스턴스 생성 X** ➡ [[Inheritance (상속)]]을 통해 Concrete한 자식 Class 만들어야 함 