#OOP 

📌 Object Oriented Programming, 프로그램을 [[Abstraction (추상화)#Object]] 중심으로 구성하는 것 
- **Program = [[Abstraction (추상화)#Object]]의 상호작용**  
	- [[Abstraction (추상화)#Object]] = 관련 있는 함수(연산)와 **데이터**의 Set, Program을 구성하기 위한 부품 
- **멤버 변수** ➡️ Good [[Independence (독립성)]]
	 1. **[[Readability (가독성)]]**: 한 모듈을 이해하기 위해 다른 모듈들을 이해할 필요 없음
	 2. **[[Safety (안전성)]]**: 모듈 안의 데이터에 다른 모듈들이 접근하지 않았음을 보장할 수 있음
- 설계는 Top-down, 구현은 Bottom-up 
↔ **Procedural (절차 지향)**

## 발전 순서
1. Unstructured
	 - 전역 변수 ➡️ [[Dependency (의존성)]]
	 - `goto` ➡️ [[SW Complexity (SW Crisis)]]
		 - Bad [[Readability (가독성)]], [[Maintainability (유지보수성)]]
	 - 코드 반복 ➡️ Bad [[Reusability (재사용성)]]
2. Procedural
	- Program = 데이터 + **함수** 호출의 Sequence
	- 반복되는 코드를 함수로 만들어 [[Reusability (재사용성)]]를 높임
3. Modular (Structured)
	- Module = 관련 있는 **함수**의 Set
		- 프로그램은 [[Interface]]를 통해 함수 사용 
	- Top-down, [[Divide & Conquer]], [[Decomposition (분해)]]
4. **OOP**

## 목적 
[[SW Engineering#배경]]과 동일 
- 급격히 높아진 [[SW Complexity (SW Crisis)]]에 대응하기 위함

## 개념
1. [[Abstraction (추상화)]], [[Data Abstraction, Encapsulation (캡슐화), Information Hiding (정보 은닉)]]
	- Maximize [[Cohesion (응집도)]], Minimize [[Coupling (결합도)]]
	- 느슨하게 결합된 객체들의 상호작용을 통해 시스템 조직 
2. [[Inheritance (상속)]]
3. [[Polymorphism (다형성)]]