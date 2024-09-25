#OOP #Design

[소공 > SOLID](onenote:https://d.docs.live.net/1bc46ecce059cbf9/Documents/2022%201학기%20소공/PPT.one#23%20SOLID&section-id={617B379E-0044-4355-A551-3465FD92561C}&page-id={F248513A-DE58-4A94-A1ED-66780D0CB8EA}&object-id={C886C2FF-CDBC-0F8F-13FD-AD3F4ED347B4}&34)  ([웹 보기](https://onedrive.live.com/view.aspx?resid=1BC46ECCE059CBF9%21263&id=documents&wd=target%28PPT.one%7C617B379E-0044-4355-A551-3465FD92561C%2F23%20SOLID%7CF248513A-DE58-4A94-A1ED-66780D0CB8EA%2F%29))
📌 [[OOP]] 설계 원칙 5가지
- 많은 원리/원칙들은 **다 경향성/Guideline일 뿐 법이 아님, 상황에 맞게 취사선택 필요**
	- ⭐ 징후가 없는데도 불구하고 **미리/과하게 적용할 경우 [[Design Smells (디자인 악취)#Needless Complexity (불필요한 복잡성)]]**, Bad [[Readability (가독성)]]
	- 미래에 자주 일어날 만한 변화에 대해서는 대응하되, 고려하는 것 자체가 비용임을 생각하기
- ⭐ 대부분의 경우 해결책은 **[[Abstraction (추상화)]]**, 또는 클래스/[[Interface]] 분리

## SRP (Single Responsibility Principle, 단일 책임 원칙)
📌 하나의 클래스는 하나의 [[#Responsibility (책임)]] 만을 가져야 함
= **High [[Cohesion (응집도)]]**
- ⭐ 각 책임별로 **클래스 분리**
- 🔎 핵심 Business Entity인 Student 클래스의 **정렬**
	![[SRP 1.png]]
	- 💔 [[Design Smells (디자인 악취)#Rigidity (경직성)]] ➡ 정렬이 필요 없는 클래스들까지 [[Design Smells (디자인 악취)#Change Propagation]]
		- **Student 클래스는 어떤 순서로 정렬되어야 하는지 모름, Student를 정렬하고 싶은 건 Client**, 그러므로 정렬은 Student 클래스의 본질적인 역할이 아님
		- Student 클래스에 이 책임을 지게 할 경우, 새로운 정렬 순서가 필요할 때마다 **(핵심 Business Entity이므로) 많은 관련 클래스 및 Client들을 다시 재컴파일**해야 함
	![[SRP 2.png]]
	- **해결**: 공통의 [[Interface]] Comparator를 상속받아, 각 Client에서 원하는 정렬 순서를 구현한 서비스 클래스들을 각각 구현하고 알맞은 Client에서 이를 참조하여 사용  
- 🔎 Rectangle 클래스를 여러 패키지에서 사용하는데, **각 패키지에서 활용하는 메소드가 다를 때** 
	![[SRP 3.png]]
	- 💔 클래스 내부의 한 메소드의 변화가 그를 사용하지 않는 패키지에 [[Design Smells (디자인 악취)#Change Propagation]]
		- 한 패키지가 참조하는 메소드들만이 하나의 책임인 것, 즉 한 클래스에 두 개의 책임이 섞여 있는 것 
	![[SRP 4.png]]
	![[SRP 5.png]]
	- **해결**: **겹치는 부분만 공통의 `abstract` 클래스**를 두고, 각 패키지에서는 `abstract` 클래스를 상속받아 각 패키지에 필요한 메소드만 추가 구현한 Concrete 클래스 참조
### Responsibility (책임)
📌 Class의 제약 또는 의무
= **Class가 수정되어야 하는 이유**, 수정이 생길 수 있는 가능성 
- Class가 많이 변화할수록 Bug를 일으킬 가능성 높아짐, [[Design Smells (디자인 악취)#Change Propagation]]으로 인해 다른 곳에 영향을 끼치게 될 수 있음 
### God Object (신 객체)
📌 다른 객체와 협력하지 않고 혼자서 모든 것을 처리하는 객체, 너무 많은 책임이 있는 객체 
### Separation of Concerns (관심사의 분리)
🔎 인터페이스 계층과 비즈니스 로직 계층의 분리

## OCP (Open Close Principle, 개방-폐쇄 원칙)
📌 확장에는 열려 있고 수정에는 닫혀 있어야 함, **특정 클래스의 행동을 수정 없이 확장**할 수 있어야 함 
- 완전히 수정이 없을 수는 없으나, **구조적 변경과 같은 비싼 수정**은 없어야 함 
- ⭐ **[[Abstraction (추상화)]] Level을 높여야** 함 
	- **상위의, 추상성 높은 부모 [[Interface]]/`abstract` 클래스는 Fixed**한 필드만 넣어 정의
	- **상위의, 추상성 높은 클래스에 [[Dependency (의존성)]]** 해야 함 ➡ 하위의 Concrete 클래스가 변하더라도, **부모 [[Interface]]/`abstract` 클래스만 그대로라면 [[Design Smells (디자인 악취)#Change Propagation]] 일어나지 않음**
		↔ 하위 Concrete Class에 의존, 변경사항 많이 생길 수 있으므로 부담됨, 의존된 쪽이 변하면 의존하는 쪽도 변해야 하므로 
- **일단은 변하지 않을 것이라 가정**하고 OCP를 적용하지 않은 채로 간단하게 Design하여 구현한 뒤, 아래와 같은 상황을 Design 변경의 계기로 삼아 개선  
	1. [[Design Smells (디자인 악취)#Change Propagation]] 의해 수정이 필요해지고,
		- 이러한 변경 계기를 최대한 빨리 잡아내는 것이 좋음 **➡ 🔎TDD, 테스트가 깨졌을 때**
	2. 이러한 수정이 반복될 것이라는 근거가 있을 때,
↔ [[Design Smells (디자인 악취)#Rigidity (경직성)]]
- 🔎 Employee를 상속받은 다양한 Concrete 직업 클래스들이 있고, Client에서는 각 Concrete 클래스 타입별로 조건문을 활용하여 동작 
	![[OCP.png]]
	- 💔 **문제**
		1. [[Design Smells (디자인 악취)#Rigidity (경직성)]] ⬅ 새 Concrete 클래스를 추가하면, Client 클래스로 [[Design Smells (디자인 악취)#Change Propagation]] 
		2. [[Design Smells (디자인 악취)#Fragility (취약성, 깨지기 쉬움)]] ⬅ 많은 조건문, 이해하고 수정하기 어려움
		3. [[Design Smells (디자인 악취)#Immobility (부동성)]] ⬅ 재사용 어려움
	![[OCP 2.png]]
	- **해결**: Client에서 조건문으로 각 Concrete 클래스 타입에 따라 다르게 작업하는 것이 아닌, **공통 Employee [[Interface]]의 메소드를 호출하여 [[Polymorphism (다형성)#Overriding (오버라이딩)]]** 활용
		- Client는 Concrete 클래스들에 대해 알 필요 없음

## LSP (Liskov Substitution Principle, 리스코프 치환 원칙)
📌 [[Inheritance (상속)]]를 사용하기 위해서는, 자식 클래스는 그들의 **부모 클래스를 의미/내용적으로 Substitution (대체)** 할 수 있어야 함
= 부모 클래스 위치에서 자식 클래스가 **Altering 없이 동작**해야 함
= 부모와 자식 클래스가 [[Inheritance (상속)#IS-A Relationship]] 관계여야 함 (자식 ⊂ 부모)
!= 프로그램/컴파일 관점에서 부모 클래스 자리에 전달할 수 있어야 함 (이는 [[OOP]] 언어라면 당연한 것, 이것보다 더 좁은 의미)
![[LSP.png]]
- [[Inheritance (상속)]]를 사용할지, [[Composition (합성)]]을 사용할지 결정할 수 있는 근거
![[LSP 2.png]]
![[LSP 3.png]]
- 💔 보통 [[Inheritance (상속)]]를 사용할 경우, **부모 클래스 자리에서 정상 동작할 것이라고 기대**, 그러나 그렇지 않으므로 (= LSP를 어겼으므로) 여러 문제 생김 
	1. [[Design Smells (디자인 악취)#Fragility (취약성, 깨지기 쉬움)]] ⬅ 부모 클래스 자리에서 정상 동작할 것이라고 예측했으나, 그렇지 않음 
	2. [[#OCP (Open Close Principle, 개방-폐쇄 원칙)]] ⬅ 함수 f는 PType의 또다른 하위 클래스들이 생길 때마다 수정 필요 (= 수정에 열려 있음)

## DIP (Dependency Inversion Principle, 의존성 역전 원칙)
📌 **High [[Abstraction (추상화)]] Level Module은 Low-Level에 [[Dependency (의존성)]]하면 안됨, 대신 Concrete를 추상화한 [[Interface]]/`abstract` 클래스에 의존**해야 함 
= Detail/Implementation에서 [[Interface]]/`abstract` 클래스에 의존해야 함, 반대 방향은 의존하면 안됨
### Inversion (역전) 
1.  **[[Dependency (의존성)]]의 역전**: [[Structured Analysis (구조적 분석설계)]]의 결과로 얻을 수 있는 Dependency를 반대로 뒤집어야 함
	![[DIP.png]]
	![[DIP 3.png]]
	- 💔 [[Design Smells (디자인 악취)#Rigidity (경직성)]]
		- Detail한 Function을 점점 더 High-Level에서 참조 ➡ 한 Funciton을 수정하면, Module과 Program으로 [[Design Smells (디자인 악취)#Change Propagation]]
	![[DIP 2.png]]
	![[DIP 4.png]]
	- **해결**: **Low-Level Concrete 클래스에서 [[Interface]] 구현 = 의존관계 역전**, Program에서도 Concrete를 직접 참조하지 않고 각 Module의 [[Interface]] 참조
		- ⭐ **Program과 Concrete 클래스 둘 다 높은, [[Abstraction (추상화)]] Level의 [[Interface]]에 [[Dependency (의존성)]]** 갖도록 함
2. **Ownership (소유권)의 역전**: 보통 [[Interface]]는 구현을 공급하는 Server쪽에서 소유하지만, **Interface를 사용/호출하는 Client 쪽에서 Interface를 소유**하고 Client 쪽에 맞게 이름을 짓도록 함
	- 🔎 Policy Layer에서 Policy Service를 소유하고 사용/호출 ↔ Mechanism Layer에서 Policy Service를 구현
	- ❤️ (생각) 사소한 변경의 경우, 변화가 아래 구현 Layer로 전파되지 않을 수 있음 

## ISP (Interface Segregation Principle, 인터페이스 분리 원칙)
📌 Client가 사용하지 않는 [[Interface]]에 의해 강제되어서는 안됨
= Client-Specific하게, **각 Client가 실제로 사용하는 것만 의존할 수 있도록 Interface를 Fine Grained (미세한 알갱이)로 나눠야** 함 
~= [[#SRP (Single Responsibility Principle, 단일 책임 원칙)]]의 Interface 버전
- 🔎 Student Enrollment, 한 클래스 안에 여러 Client를 위한 Interface가 포함됨
	![[ISP 1.png]]
	- 💔 Client 간 불필요한 [[Coupling (결합도)]]을 만들게 됨 ➡ **[[Design Smells (디자인 악취)#Change Propagation]]**, 한 Client가 Interface의 변화를 만들게 될 경우 모든 다른 Client들이 모두 Interface의 변화에 영향을 받게 됨
	![[ISP 2.png]]
	- **해결**: Interface를 [[Cohesion (응집도)]]한 Group으로 분리 ➡ **진짜 사용하는 Interface에게만 [[Dependency (의존성)]]를 가지도록** 함 
### Fat Interface
📌 서로 다른 Client에 대한 [[Interface]]를 하나로 묶어 정의한 Interface
= Non-[[Cohesion (응집도)]] Interface
- [[GoF 디자인 패턴#Facade (퍼사드)]]
- **해결**: [[#ISP ( Interface Segregation Principle, 인터페이스 분리 원칙)]]