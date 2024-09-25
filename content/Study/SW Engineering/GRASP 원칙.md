#OOP #Design 

📌 General Responsibility Assignment Software Pattern, 객체 설계에 대한 9가지 원칙 
- **목적**: 하나의 큰 시스템을 Module로 쪼갠 뒤, 각각에 Responsibility를 부여 ➡ [[SW Complexity (SW Crisis)]]를 낮추기 위함
- **방법 = Modularity 증가**
	1. [[SOLID 원칙, 객체지향 설계 원칙#Separation of Concerns (관심사의 분리)]]
	2. [[Data Abstraction, Encapsulation (캡슐화), Information Hiding (정보 은닉)]]
	- **측정 ➡ Low [[Coupling (결합도)]], High [[Cohesion (응집도)]]**

## Information Expert Pattern
📌 누구에게 특정 책임을 부여할지? 
= **필요한 정보를 가진 객체**
- **상황**
	1. 🚫 정보가 여러 객체에 나뉘어 있을 때 ➡ 협업 필요 
	2. ⭐ **[[SOLID 원칙, 객체지향 설계 원칙#Separation of Concerns (관심사의 분리)]] = Pure Fabrication과 고민**
		- 🔎 DB 저장 ➡ 정보를 가진 모든 객체마다 DB 로직을 추가하는 것보다, Domain에 없는 새 DB Class를 생성하는 것이 더 나은 선택 (Low [[Cohesion (응집도)]])
- ❤️ [[Data Abstraction, Encapsulation (캡슐화), Information Hiding (정보 은닉)]] ➡ Low [[Coupling (결합도)]]

## Creator Pattern
📌 객체 A를 누가 생성할지?
**= A를 포함하는/[[UML#(Shared) Aggregation (집합)]]하는 B**
= A를 기록하는/밀접하게 사용하는/초기값을 갖는 B
🔎 [[GoF 디자인 패턴#Abstract Factory]], [[GoF 디자인 패턴#Singleton]]
- **상황**
	1. 🚫 성능 문제로 객체 생성보다 재활용이 필요할 때  
	2. 🚫 조건에 따라 여러 클래스 타입 중 하나를 선택해서 생성해야 할 때
	3. 🚫 외부에서 객체를 쉽게 갈아끼워야 할 때 ➡ [[Dependency Injection (의존성 주입)]]
- ❤️ Low [[Coupling (결합도)]]

## [[MVC 패턴#Controller]] Pattern
📌 UI의 유저 인풋을 응용 로직으로 전달하는 
= 옵션 둘 중 하나 택 1
1. (Sub) System을 대표할 수 있는 하나의 객체 생성
2. [[Use Case]] 시나리오에 따라 

## Low [[Coupling (결합도)]] Pattern
📌 서로 다른 대안 중 어떤 걸 선택할지?
= [[Coupling (결합도)]]이 낮아지는 방향으로 책임 부여
= ⭐ 꼭 필요한 Link가 아니라면, **처리 [[Delegation (위임)]]**
- **상황**: 🚫 변화가 거의 일어나지 않을 때 
	- 🔎 Java Library 등, 응용 단에서 만든 Class가 아닌 것들

## High [[Cohesion (응집도)]] Pattern
📌 서로 다른 대안 중 어떤 걸 선택할지?
= [[Cohesion (응집도)]]이 높아지는 방향으로 책임 부여  
= ⭐ 여러 다른 책임이 추가될 것으로 보이는 Class의 경우, **[[Interface]] 역할만 맡고 책임이 적어지는 방향**으로 

## [[Polymorphism (다형성)]] Pattern
📌 타입에 따라 행동이 달라질 때, 누구에게 책임을 부여할지?
= [[Polymorphism (다형성)]] OP 활용 ➡ 외부에서 보이는 [[Interface]]는 같되, 동작 다르도록
- **상황**
	1. 비슷한 변종들을 어떻게 다룰지 고민할 때
	2. ⭐🚫 **Future Proofing**, 미리 확장될 미래를 예견하여 하는 것은 괜한 Cost
- ❤️ **장점**
	1.  [[Reliable (신뢰성)]]
	2. 새 선택지 추가 시 기존 로직에 영향 주지 않음 ↔ `if`, `switch`
- 💔 **단점**
	1. [[Inheritance (상속)#단점]]#**클래스 폭발**
	2. Less [[Readability (가독성)]]

## Pure Fabrication Pattern
📌 Low [[Coupling (결합도)]], High [[Cohesion (응집도)]]을 위반하고 싶지 않을 때, 누구에게 책임을 부여할지?
📌 순수한 것을 만들어 내다
= [[SOLID 원칙, 객체지향 설계 원칙#Separation of Concerns (관심사의 분리)]]
= ⭐ **Domain에는 없던** 가상의, 편의를 위한 **Behavior Class 생성**
- **상황**: 비슷한 로직이 Information Expert 원칙 등에 의해 여러 곳에 분산 ➡ Low [[Cohesion (응집도)]]일 때
- ❤️ 동일 로직에 대한 복사본 감소 ➡ [[Reusability (재사용성)]]

## Indirection Pattern
📌 Direct [[Coupling (결합도)]]을 줄이기 위해, 누구에게 책임을 부여할지?
= ⭐ **Intermediate 객체 = 중간 [[Interface]]**랑만 [[Coupling (결합도)]]을 맺도록 함
🔎 [[GoF 디자인 패턴#Adapter]], [[GoF 디자인 패턴#Facade (퍼사드)]], [[GoF 디자인 패턴#Proxy]], [[GoF 디자인 패턴#Mediator]]

## PV (Protected Variations Pattern)
📌 다양성/변화가 다른 Element에 영향을 주지 않기 위해, 누구에게 책임을 부여할지?
= 기존 로직/디자인에 변화를 주지 않도록, **다양성/변화가 생길 지점을 예측하여 안정적인 [[Interface]]를 통해 포장**
= [[SOLID 원칙, 객체지향 설계 원칙#OCP (Open Close Principle)]]
- ❤️ **장점**
	1. [[Flexibility (유연성)]], 기존 코드를 변화로부터 보호할 수 있음
	2. **더 구조화된 디자인** 제공 ➡ [[Maintainability (유지보수성)]] 시 기존 디자인이 유지될 가능성 더 높음