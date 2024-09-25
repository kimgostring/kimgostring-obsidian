#Design #DesignPattern 

[Refactoring.Guru > 디자인 패턴](https://refactoring.guru/ko/design-patterns/catalog)
📌 Gang of Four (4인조), 4명의 사람이 구체화한 23개([[#Adapter, Wrapper]]를 중복으로 치면 22개)의 디자인 패턴 

|         | [[#생성 (Creational)]]                                                      | [[#구조 (Structural)]]                                                                                                                          | [[#행위 (Behavioral)]]                                                                                                                                                                                           |
| ------- | ------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **클래스** | [[#Factory Method, Virtual Constructor]]                                  | [[#Adapter, Wrapper]]                                                                                                                         | [[#Interpreter]]<br>[[#Template Method]]                                                                                                                                                                       |
| **객체**  | [[#Abstract Factory]]<br>[[#Builder]]<br>[[#Prototype]]<br>[[#Singleton]] | [[#Adapter, Wrapper]]<br>[[#Bridge]]<br>[[#Composite]]<br>[[#Decorator, Wrapper]]<br>[[#Facade (퍼사드)]]<br>[[#Flyweight, Cache]]<br>[[#Proxy]] | [[#Chain of Responsibility (CoR, Chain of Command)]]<br>[[#Command]]<br>[[#Iterator]]<br>[[#Mediator (중재자)]]<br>[[#Memento]]<br>[[#Observer, Event-Subscriber]]<br>[[#State]]<br>[[#Strategy]]<br>[[#Visitor]] |
# 관계도
![[GoF 디자인 패턴 관계도.png]]

# 생성 (Creational)
## 클래스 생성 패턴
### Factory Method, Virtual Constructor
[Factory Method](https://refactoring.guru/ko/design-patterns/factory-method)
📌 객체(= Product)를 생성하여 리턴하는 **Factory Method를 갖는 클래스/[[Interface]] 가 있고, 자식 클래스에서 Factory Method를 각기 다르게 구현**하여 서로 다른 세부 타입의 Product를 생성할 수 있는 패턴 
![[Factory Method.png]]
- 📌 **용어** 
	- **Product**: Factory Method를 통해 생성되는 객체, 모든 Product는 **공통 부모/[[Interface]] 필요**
		- **Concrete Product**: Product [[Interface]]의 구현
	- **Client Code**: Factory Method를 사용하는 코드, **모든 Product를 공통 부모/[[Interface]]로 간주**
	- **Creator**: 리턴값이 Factory Method를 선언하는 클래스
		- Factory Method가 주요 [[SOLID 원칙, 객체지향 설계 원칙#Responsibility (책임)]] 인 것 아님, ⭐**Creator 메인 로직과 Product 생성 로직을 Decoupling (분리), [[Data Abstraction, Encapsulation (캡슐화), Information Hiding (정보 은닉)]]** 하기 위한 것일 뿐 
		- **Concrete Creator**: **Factory Method [[Polymorphism (다형성)#Overriding (오버라이딩)]]** 을 통해 서로 다른 Concrete Product를 반환하는 객체  
- **상황**
	1. 객체 세부 유형, 의존 관계를 미리 모르는 경우 (앞으로 더 다양한 Product를 추가하게 될 경우, [[Extensibility (확장성)]])
	2. 프레임워크(Creator)가 사용자 정의 클래스(Concrete Product)를 사용할 수 있도 만들 경우
		- 🔎 기본 버튼과 UI 프레임워크가 있고, 사용자가 기본 버튼을 [[Inheritance (상속)]]받아 새 버튼을 만들었을 때 ➡ UI 프레임워크의 Factory Method에서 사용자 정의 버튼을 사용하도록 구현
	3. 무거운 작업, 공통 객체가 여러 개 필요할 때, **기존 객체를 매번 재구축하는 대신 재사용** ➡ ❤️**리소스 절약**하고 싶을 때
		↔ 생성자, 
- ❤️ **장점**
	1. Creator와 Concrete Product 간 Loosely [[Coupling (결합도)]]
	2. [[SOLID 원칙, 객체지향 설계 원칙#SRP (Single Responsibility Principle, 단일 책임 원칙)]] ➡ [[Maintainability (유지보수성)]]
	3. [[SOLID 원칙, 객체지향 설계 원칙#OCP (Open Close Principle, 개방-폐쇄 원칙)]], 기존 Client Code 수정 없이 새 유형의 Product 도입 가능 ➡ [[Extensibility (확장성)]] 
- 💔 자식 클래스의 수 증가 ➡ 복잡도 증가
	- 자식 클래스 간 ⭐**계층 구조**를 만들어 해결 가능

## 객체 생성 패턴
### Abstract Factory
[Abstract Factory](https://refactoring.guru/ko/design-patterns/abstract-factory)
📌 **관련 Product들의 Set(= Family, 제품군)** 을 묶어, (주로 프로그램 초기화 단계에) 조건에 따라 특정 Concrete Factory 객체를 생성하고 그를 통해 Family에 포함된 Products들을 생성하는 패턴
**!= ⭐`abstract`으로 선언된 [[#Factory Method, Virtual Constructor]]**
![[Abstract Factory.png]]
- 📌 **용어**
	- **Abstract Products**: 모든 Family의 각 Product에 대한 [[Interface]]를 명시적으로 선언하고, 모든 Concrete Product가 이를 따르도록 함
	- **Abstract Factory**: **모든 Abstract Product에 대한 생성 메소드**가 목록화되어 있는 [[Interface]]
	- **Concrete Factory**: Abstract Factory를 특정 Family에 대해 구현한 클래스 
		- **Client Code는** Abstract Products, Abstract Factory에 대해서만 동작해야 하며, **Concrete에 대해서는 몰라야** 함 ➡ Concrete Factory 객체는 ⭐**프로그램 초기화 단계**에 생성 
			- 🔎 **크로스 플랫폼**, 운영체제에 따라 다른 UI ➡ 앱 시작 시 현재의 운영체제를 확인, 이와 일치하는 Concrete Factory 객체 생성 
		- 각 생성 메소드의 리턴 타입은 Abstract Factory를 [[Polymorphism (다형성)#Overriding (오버라이딩)]]하므로 Abstract Product [[Interface]]이지만, 실제로는 해당 Family에 맞는 Concrete를 리턴해야 함
- **상황**
	1. 다양한 Family와 작동해야 하고, 각 Concrete Product에 의존하고 싶지 않을 때 (향후 [[Extensibility (확장성)]] 고려)
	2. Factory Method가 존재하는 클래스의 [[SOLID 원칙, 객체지향 설계 원칙#Responsibility (책임)]]가 뚜렷하지 않을 때 ➡ [[SOLID 원칙, 객체지향 설계 원칙#SRP (Single Responsibility Principle, 단일 책임 원칙)]] 만족을 위해, 이를 하나의 책임으로 떼어 분리
- ❤️ **장점**
	1. Family 내의 Product 간 상호 호환 보장
	2. Client Code와 Concrete Product 간 Loosely [[Coupling (결합도)]]
	3. [[SOLID 원칙, 객체지향 설계 원칙#SRP (Single Responsibility Principle, 단일 책임 원칙)]]
	4. [[SOLID 원칙, 객체지향 설계 원칙#OCP (Open Close Principle, 개방-폐쇄 원칙)]]
- 💔 복잡도 증가
### Builder
[Builder](https://refactoring.guru/ko/design-patterns/builder)
📌 복잡한 객체 (= Product)를 **Builder에 정의된 필요한 단계들을 호출하여 생성**하는 패턴 
![[Builder.png]]
- 📌 **용어**
	- **Builder**: 객체 **생성 각 단계를 선언**하는 [[Interface]]
		- 모든 Product를 생성하기 위한 공통 생성 단계가 명확해야 함 
		- Builder의 각 단계 중 특정 설정에 **필요한 단계만 호출**하면 됨
	- **Concrete Builder**: 각 생성 단계에 대한 구현 제공, 각 Concrete Builder는 다양한 방식으로 같은 작업을 수행하게 됨 
		- ⭐**생성 결과를 반환하는 메소드** 구현 필요 ➡ 모든 Product 간 [[Interface]]가 동일할 경우 생성 결과 반환 메소드를 Builder나 Director에 추가할 수 있으나, 그렇지 않다면 Concrete Builder에 각각 별도 메소드로 선언해야 함 
	- **(선택 사항) Director/관리자**: **Builder 단계들에 대한 일련의 호출**을 별도로 추출한 클래스 
		1. Client Code에서 **Director의 생성자 매개변수**를 통해 특정 Concrete Builder와 연결
		2. Director의 **객체 생성 메소드 파라미터**를 통해 Builder 객체 전달 
		- ❤️ [[Reusability (재사용성)]], Client Code로부터 Builder를 감출 수 있음 ([[Data Abstraction, Encapsulation (캡슐화), Information Hiding (정보 은닉)]]) 
- **상황**
	1. **점층적 생성자**: 생성자에 선택적 매개변수가 여러 개 존재, [[Polymorphism (다형성)#Overloading (오버로딩)]]으로 매개변수만 다른 생성자 여러 개를 구현해야 하는 경우 
	2. 같은 Product의 다른 표현을 생성할 때, 생성 단계가 세부 사항만 다르고 유사할 때
	3. 객체 생성이 복잡할 때 
		- Product를 단계별로 생성할 때
		- Product의 손상 없이 일부 단계의 실행을 연기해야 할 때
		- 미완성 Product를 노출하지 않아야 할 때
		- 재귀적으로 단계를 호출해야 할 때 (🔎 [[#Composite]] ➡ 객체 트리 구축)
- ❤️ **장점**
	1. 객체 단계별 생성, 생성 단계 연기, 재귀적 실행 등 가능
	2. Product의 다양한 표현을 만들어야 할 때, 생성 코드 [[Reusability (재사용성)]] 가능
	3. [[SOLID 원칙, 객체지향 설계 원칙#SRP (Single Responsibility Principle, 단일 책임 원칙)]] 
- 💔 복잡성 증가 
### Prototype
[Prototype](https://refactoring.guru/ko/design-patterns/prototype)
📌  복제에 대한 [[SOLID 원칙, 객체지향 설계 원칙#Responsibility (책임)]]을 원본 객체에게 부여하여, **`clone` 메소드를 포함하는 [[Interface]] (= Prototype)을 구현하여 Client가 Concrete Prototype에 의존하지 않고 객체를 복사**는 패턴 
![[Prototype.png]]
- 📌 **용어**
	- **Prototype**: `clone` 메소드를 포함하는 [[Interface]]
	- **Concrete Prototype**: `clone` 메소드를 구현한 클래스
		- 🔎 세포의 유사분열, 원본 세포 = Prototype
	- **Prototype Registry**: **자주 사용되는 Prototype 객체들에 대한 참조를 제공**하는 클래스 
		![[Prototype Registry.png]]
		- 🔎 Name - Prototype 해시 맵
- **상황**
	1. ⭐ **Client가 Concrete Prototype에 의존할 수 없는** 경우 (복사 로직이 객체 외부에 존재할 수 없는 경우)
		 - Concrete Prototype이 알려지지 않은 경우 
		 - 객체 안에 `private` 멤버가 있는 경우
	2. 객체 **초기화 코드가 복잡하거나 여러 번 반복**되는 경우 ➡ 이미 생성된 Concrete Prototype **객체 복제를 통해 초기화** 가능 (Prototype Registry 활용)
- ❤️ **장점**
	1. Client와 Concrete Prototype 간 Loosely [[Coupling (결합도)]]
	2. 복잡한/반복되는 객체 초기화/생성을 간단하게 바꿀 수 있음 
- 💔 순환 참조가 있을 경우 어려움
### Singleton
[Singleton](https://refactoring.guru/ko/design-patterns/singleton), [Inpa > Singleton](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EC%8B%B1%EA%B8%80%ED%86%A4Singleton-%ED%8C%A8%ED%84%B4-%EA%BC%BC%EA%BC%BC%ED%95%98%EA%B2%8C-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90)
📌 단 하나의 유일한 객체를 만들고 객체가 필요할 때 전역 참조를 제공하는, **새 인스턴스 생성 대신 하나의 인스턴스를 재활용하는** 패턴
![[Singleton.png]]
- 📌 **Singleton**: **`static getInstance` 메소드를 선언한 클래스**, 해당 메소드를 통해 해당 클래스 타입의 유일한 객체를 반환받을 수 있음 
	1. **생성자에 `private` 키워드**를 붙여 `new` 연산자를 사용 불가능하도록 막은 뒤
	2. 최초 1회 고정 메모리 영역에 객체를 생성/초기화하고, 클래스에 **객체 참조를 리턴하는 `static getInstance` 메소드** 정의
- **상황**
	1. 모든 프로그램의 다른 부분에서 한 객체가 공유되어야 할 때
	2. 무거운 작업을 위한 공통 객체가 여러 개 필요할 때
		- 🔎 DB/디스크 연결 모듈, 네트워크 통신 ➡ 한 번만 생성한 뒤 계속 참조해서 사용하는 게 좋음, **재활용을 통한 리소스 절약** 
	3. 전역에서 해당 클래스 타입의 객체가 단 하나뿐이도록 엄격하게 제어해야 할 때 
- ❤️ 클래스에 대한 객체가 하나뿐임을 강제하고, 해당 객체에 대한 전역 접근 지점을 얻을 수 있음 
- 💔 **단점**
	1. [[Interface]]가 아닌 **실제 객체 (Singleton Class)와 다른 객체들 간의 의존성 및 높은 [[Coupling (결합도)]]** 
		- [[SOLID 원칙, 객체지향 설계 원칙#OCP (Open Close Principle, 개방-폐쇄 원칙)]]
		- [[SOLID 원칙, 객체지향 설계 원칙#DIP (Dependency Inversion Principle, 의존성 역전 원칙)]]
	2. ⭐ **[[SOLID 원칙, 객체지향 설계 원칙#SRP (Single Responsibility Principle)]]에 위배 ➡ [[OOP]]의 안티 패턴**이라고도 불림
	3. 다중 스레드에서의 [[Thread Safe]]한 구현이 어려움 
		- [자바로 작성된 싱글턴](https://refactoring.guru/ko/design-patterns/singleton/java/example), [Inpa > Singleton 패턴 구현 기법 종류](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EC%8B%B1%EA%B8%80%ED%86%A4Singleton-%ED%8C%A8%ED%84%B4-%EA%BC%BC%EA%BC%BC%ED%95%98%EA%B2%8C-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90#%EC%8B%B1%EA%B8%80%ED%86%A4_%ED%8C%A8%ED%84%B4_%EA%B5%AC%ED%98%84_%EA%B8%B0%EB%B2%95_%EC%A2%85%EB%A5%98)
	4. Client에 대한 [[Study/SW Engineering/Test#Unit (Module) Test]] 어려움 ⬅ Singleton Class의 생성자가 `private`이고, `static` 메소드의 [[Polymorphism (다형성)#Overriding (오버라이딩)]]이 불가능한 언어가 많기 때문

# 구조 (Structural)
## 클래스 구조 패턴
### Adapter, Wrapper
[Adapter](https://refactoring.guru/ko/design-patterns/adapter), [기계인간 > Adapter](https://johngrib.github.io/wiki/pattern/adapter/)
📌 수정이 어렵고 **Client와 데이터 형식/[[Interface]]가 호환되지 않는 클래스(= Adaptee)를 Wrapping**하고, Client와 호환되는 데이터 형식/[[Interface]]를 구현하여 Adapter 클래스를 만드는 패턴
![[Adapter.png]]
- 📌 **용어**
	- **Client [[Interface]]**: 다른 클래스가 Client 코드와 협업하기 위해 지켜야 하는 프로토콜
	- **Adapter**: **Adaptee를 Wrapping**하고, Adaptee의 메소드를 호출하여 **Client [[Interface]]를 구현**하여 Client 코드와 협업할 수 있도록 만든 객체 
		- **Class Adapter**: Adaptee를 **Wrapping이 아닌 [[Inheritance (상속)]]** 하는 방식 
			![[Class Adapter.png]]
	- **Adaptee**: Client에서 사용하고 싶지만, Client와 [[Interface]]가 맞지 않아 직접 사용하지 못하는 클래스 
	- **Service**: 타사 또는 레거시, 기존 의존성이 많은 ([[Coupling (결합도)]]) 유용한 클래스, 일반적으로 Service는 Adaptee인 경우가 많음
- **상황**
	1. 기존 클래스의 [[Interface]]가 Client의 코드와 호환되지 않는 경우
	2. 부모 클래스에 추가할 수 없는 누락된 공통 기능이 몇몇 자식 클래스에 있을 경우 ➡ 누락된 기능을 Adapter에 넣어 Wrapping
- ❤️ **장점**
	1. 로직에서 데이터 변환 로직/[[Interface]] 분리 가능 ➡ [[SOLID 원칙, 객체지향 설계 원칙#SRP (Single Responsibility Principle, 단일 책임 원칙)]] 
	2. 기존 Client 코드를 수정하지 않고, 새 유형의 Adapter들을 프로그램에 추가 가능 ➡ [[SOLID 원칙, 객체지향 설계 원칙#OCP (Open Close Principle, 개방-폐쇄 원칙)]] 
- 💔 복잡성 증가 

## 객체 구조 패턴
### [[#Adapter, Wrapper]]
### Bridge
[Bridge](https://refactoring.guru/ko/design-patterns/bridge)
📌 클래스의 **특징이 1차원을 넘을 때, [[Inheritance (상속)]] 대신 [[Composition (합성)]]** 을 통해 자식 클래스의 개수가 조합 수만큼 기하급수적으로 늘어나는 것을 막는 패턴
![[Bridge 2.png]]
- **각 특징을 서로 다른 클래스로 분리**, [[Composition (합성)]]을 통해 참조(= Bridge)
![[Bridge 3.png]]
- 🔎 크로스 플랫폼 앱 ➡ ⭐ 클래스를 **두 개의 계층 구조**로 분리 
	1. **Abstraction (추상화), Interface**: UI Layer, 리모컨, 도메인, 프론트엔드
		!= [[Abstraction (추상화)]], [[Interface]]
	2. **Implementation, Platform**: 운영 체제별 API, 리모컨으로 조종 가능한 장치, 인프라, 백엔드
![[Bridge.png]]
- 📌 **용어**
	- **Abstraction (추상화), Interface**: 일부 Entity에 대한 상위 수준의 Control Layer
		- **실제 작업을 전부 Implementation에 위임**, Implementation에 정의된 메소드만을 통해 Implementation과 소통
	- **(선택 사항) Refined Abstraction (정제된 추상화)**: Control 로직의 변형 제공
	- **Implementation, Platform**: Concrete Implementation에 대한 공통 [[Interface]]
	- **Concrete Implementation (구상 구현)**: 플랫폼별 맞춤 코드 제공 
	- **Client**: Abstraction과 상호 작용
		- Abstraction의 생성자를 통해 Abstraction과 Concrete Implementation을 연결
- **상황**
	1. **여러 변형을 가진 Monolithic (모놀리식) 클래스를 여러 클래스 계층 구조로** 나누고 싶을 때
		- 📌 **Monolithic (모놀리식)**: 단일 코드 베이스의 애플리케이션, 구현이 쉽고 단순하지만 유지 보수 및 확장이 어려움 
			[모놀리식 vs 마이크로서비스, 어떤 아키텍처를 선택할까?](https://yozm.wishket.com/magazine/detail/1813/)
			↔ **Microservice** 
	2. 여러 독립적인 차원(특징)에서 클래스를 수정 및 확장하고 싶을 때
		- ⭐🔎 여러 모양의 버튼이 있고, 각 운영 체제별로 호출 API가 다를 때 ➡ 각 차원(모양, 운영 체제)을 모두 [[Inheritance (상속)]]하여 자식 클래스의 조합을 폭발적으로 만드는 것이 아닌, [[Composition (합성)]]하여 각 특징 별로 별도의 클래스 계층 구조를 두는 것  
	3. 런타임에 Abstraction 내부 Implementation을 변경하고 싶을 때 
- ❤️ **장점**
	1. 플랫폼에 독립적
	2. [[SOLID 원칙, 객체지향 설계 원칙#SRP (Single Responsibility Principle, 단일 책임 원칙)]]
	3. Abstraction, Implementation을 독립적으로 추가/수정 가능 ➡ [[SOLID 원칙, 객체지향 설계 원칙#OCP (Open Close Principle, 개방-폐쇄 원칙)]]
- 💔 [[Coupling (결합도)]]이 높은 클래스의 경우, 코드가 더 복잡해짐
### Composite (복합체)
[Composite](https://refactoring.guru/ko/design-patterns/composite)
📌 [[트리]] 구조로 표현되는 구조에서, **자식에 대해 알 필요 없이 트리의 각 노드에 대한 공통 [[Interface]](= Component)를 통해 [[재귀]]적으로 요청**을 아래로 전달하는 패턴
![[Composite.png]]
- 📌 **용어**
	- **Component**: 트리의 각 노드들에 대한 공통 [[Interface]] 
	- **Leaf**: 자식 노드가 없는 트리의 기본 요소 
	- **Container, Composite**: 자식 노드를 갖는 요소, 자식의 Concrete에 대해 알지 못하며 Component에 정의된 [[Interface]]를 통해서만 자식 노드들과 동작 
		- 공통 메소드 안에서 자식 노드의 메소드를 호출하여 재귀적으로 요청 전달, 이를 요약하여 리턴 
	- **Client**: Component를 통해, 트리의 모든 요소들과 동일한 방법으로 처리
		- ⭐ **자식 요소 여부와 관계없이 공통 [[Interface]]로 작업** 가능
- **상황**
	1. [[트리]] 객체 구조를 구현해야 할 때, Leaf와 Composite 간 공통 [[Interface]](= Component)를 공유할 때
	2. Client 코드가 트리의 모든 요소들을 동일한 방법으로 처리하고 싶을 때 
- ❤️ **장점**
	1. 복잡한 [[트리]] 구조와 편하게 작업 가능 ⬅ [[Polymorphism (다형성)]], [[재귀]]
	2. 기존 트리 요소를 해치지 않고 새 유형 추가 가능 ➡ [[SOLID 원칙, 객체지향 설계 원칙#OCP (Open Close Principle, 개방-폐쇄 원칙)]]
- 💔 공통 [[Interface]]를 제공하기 어렵거나, 과도하게 일반화가 필요한 경우 존재
### Decorator, Wrapper
[Decorator](https://refactoring.guru/ko/design-patterns/decorator)
📌 [[Inheritance (상속)]] 대신 [[UML#Relationship (연관성)]]#Aggregation (집합) 또는 [[Composition (합성)]]을 사용하여 **(Decorator로 Wrapping하여), 런타임에 여러 비즈니스 로직을 추가/삭제**할 수 있는 패턴
![[Decorator.png]]
- 📌 **용어**
	- **Component**: 선택적 비즈니스 계층인 Wrapper(= Decorator), 기본 컴포넌트 Wrappee(= Component/Decorator)의 공통 메소드를 정의한 [[Interface]]
	- **Concrete Component**: 가장 마지막에 Wrapping되는 객체를 정의한 클래스, **기본 행동을 정의**
	- **Base Decorator**: **Component [[Interface]]를 참조하는 필드(= Wrappee)** 가짐  
		- **목적**: 모든 Concrete Decorator에 대한 Wrapping [[Interface]] 정의
		- Concrete Component, Concrete Decorator 모두를 참조 가능 ➡ 각 Decorator의 Wrappee 필드로 ⭐**Component를 여러 겹의 Decorator로 Wrapping** 가능 (**= Wrapper Stack**)
	- **Concrete Decorator**: Base Decorator의 메소드 [[Polymorphism (다형성)#Overriding (오버라이딩)]] ➡ Component에 덧씌울 수 있는 추가 행동 정의 후 ⭐**항상 전/후로 부모 메소드 호출(위임)**  
		- 🔎 데이터 (= Component)를 읽고 쓰기 전 Encryption (암호화/복호화), Compression (압축) (= Decorator)
	- **Client**: Component를 필요에 따라 여러 Decorator로 Wrapping
		![[Decorator Stack.png]]
- **상황**
	1. 객체를 사용하는 코드를 훼손하지 않으면서, ⭐**객체에 추가 행동을 선택적으로 런타임에 추가**(= Aggregation (집합), [[Composition (합성)]])하고 싶을 때 
		- **비즈니스 로직을 계층으로 구성하고 각각을 데코레이터로** 생성, 런타임에 데코레이터를 선택적으로 조합하여 객체를 구성할 수 있도록 함 ➡ 어떤 조합이든, Client는 Component [[Interface]]를 통해 동일하게 다룰 수 있음 
	2.  [[Inheritance (상속)]]로 객체의 행동을 확장하는 것이 어색하거나 불가능할 때 (🔎 `final` ➡ 상속을 통한 확장 불가)
- ❤️ **장점**
	1. 새 자식 클래스를 정의하지 않고 객체 행동 확장 가능
	2. 런타임에 객체들로부터 책임 추가/삭제 가능
	3. 여러 Decorator로 Wrapping하여 여러 행동 합성 가능
	4. 다양한 행동의 여러 변형을 구현한 Monolithic (모놀리식) 클래스를 여러 개로 분해 가능 ⬅ [[SOLID 원칙, 객체지향 설계 원칙#SRP (Single Responsibility Principle, 단일 책임 원칙)]] 
- 💔 **단점**
	1. Wrapper Stack에서 특정 Wrapper를 제거하기 어려움
	2. Decorator의 행동이 Stack 순서에 의존하지 않도록 구현하기 어려움
	3. 초기 계층 설정 코드가 흉할 수 있음
### Facade (퍼사드)
[Facade](https://refactoring.guru/ko/design-patterns/facade), [Infa > Facade](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%ED%8D%BC%EC%82%AC%EB%93%9CFacade-%ED%8C%A8%ED%84%B4-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EC%9E%90) 
📌 정면/외관, 복잡한 서브 시스템의 **기능을 제한하더라도 단순한 (= 높은 [[Abstraction (추상화)]] Level) [[Interface]]** 가 필요할 때 사용하는 패턴
!= [[Data Abstraction, Encapsulation (캡슐화), Information Hiding (정보 은닉)]], 퍼사드에 속한 **하위 클래스에도 접근 가능**
![[Facade.png]]
- 📌 **용어**
	- **Facade**: 하위 시스템의 특정 기능에 편리하게 접근 가능
		- 하위 시스템 초기화, 수명 주기 관리, 적절한 객체로 요청 리다이렉션 등의 책임
		- Client 코드는 최대한 Facade와만 소통 ➡ 하위 시스템 코드의 변경 사항들로부터 보호 가능 
	- **Additional Facade**: 한 Facade가 관련 없는 기능들로 오염되지 않도록 함, Client 및 다른 Facade에 의해 사용됨
		- Facade가 너무 커질 경우, 일부를 새 Refined Facade로 추출 
- **상황**
	1. 복잡한 하위 시스템에 대한 제한적이지만 간단한 [[Interface]]가 필요할 때 + Facade를 통해 Client를 여러 클래스로부터 독립시킬 수 있을 때 (= 실제로 더 간단한 [[Interface]]를 정의할 수 있을 때)
	2. 하위 시스템을 계층으로 구성하고, 각 계층에 대한 진입점(= Facade) 정의하여 계층 별로 Facade로 소통해야 할 때 ➡ 하위 시스템 간 [[Coupling (결합도)]] 감소 ([[Abstraction (추상화)]])
- ❤️ 복잡한 하위 시스템으로부터 코드 분리 가능
- 💔 퍼사드가 서브 시스템에 대한 의존도를 가짐, **[[SOLID 원칙, 객체지향 설계 원칙#God Object (신 객체)]]** 가 될 수 있음
### Flyweight, Cache
[Flyweight](https://refactoring.guru/ko/design-patterns/flyweight)
📌 경량, **외부에 의해 변하지 않고 공통되는 데이터(= Intrinsic, 고유한 상태)를 각각 저장하지 않고 별도 객체(= Flyweight)에 두고 Context에서 공유**하여, RAM에 더 많은 객체를 포함할 수 있도록 하는 패턴 
- ⭐ **목적: 최적화**
	1. 유사 객체를 매우 많이 가지고 있고
	2. **RAM 소비에 문제가 있고** 
	3. 다른 해결책이 없는 상황에서 고려
![[Flyweight.png]]
- 📌 **용어**
	- **Flyweight**: 여러 Context에 의해 공유되는 **고유한 상태만 저장**하는 객체, 공유한 상태는 Context 객체에 저장하게 됨
		- 생성자에서 최초 한 번만 고유한 상태를 초기화하고, `setter`나 `public` 필드를 노출하면 안 됨
		- Context의 공유한 상태를 사용하던 메소드의 경우 **메소드 파라미터에 공유한 상태를 추가하고, 같은 이름을 갖는 Context의 메소드 내부에서 이를 호출**하여 동작
		``` 
			// 플라이웨이트 클래스는 트리의 상태 일부를 포함합니다. 이러한 필드는 각 특정
			// 트리에 대해 고유한 값들을 저장합니다. 예를 들어 여기에서는 트리 좌표들을 찾을
			// 수 없을 것입니다. 그러나 많은 트리들이 공유하는 질감들과 색상들은 찾을 수 있을
			// 것입니다. 이 데이터는 일반적으로 크기 때문에 각 트리 개체에 보관하면 많은
			// 메모리를 낭비하게 됩니다. 대신 질감, 색상 및 기타 반복되는 데이터를 많은 개별
			// 트리 객체들이 참조할 수 있는 별도의 객체로 추출할 수 있습니다.
			class TreeType is
			    field name
			    field color
			    field texture
			    constructor TreeType(name, color, texture) { ... }
			    method draw(canvas, x, y) is
			        // 1. 주어진 유형, 색상 및 질감의 비트맵을 만드세요.
			        // 2. 비트맵을 캔버스의 X 및 Y 좌표에 그리세요.
			
			// 플라이웨이트 Factory (팩토리)는 기존 플라이웨이트를 재사용할지 아니면 새로운 객체를
			// 생성할지를 결정합니다.
			class TreeFactory is
			    static field treeTypes: collection of tree types
			    static method getTreeType(name, color, texture) is
			        type = treeTypes.find(name, color, texture)
			        if (type == null)
			            type = new TreeType(name, color, texture)
			            treeTypes.add(type)
			        return type
			
			// 콘텍스트 객체는 트리 상태의 공유된 부분을 포함합니다. 이러한 부분들은 두 개의
			// 정수로 된 좌표와 하나의 참조 필드만 참조하여 크기가 작기 때문에 하나의 앱이
			// 이런 부분을 수십억 개씩 만들 수 있습니다.
			class Tree is
			    field x,y
			    field type: TreeType
			    constructor Tree(x, y, type) { ... }
			    method draw(canvas) is
			        type.draw(canvas, this.x, this.y)
			
			// Tree 및 Forest 클래스들은 플라이웨이트의 클라이언트들이며 Tree 클래스를 더
			// 이상 개발할 계획이 없으면 이 둘을 병합할 수 있습니다.
			class Forest is
				field trees: collection of Trees
			
			    method plantTree(x, y, name, color, texture) is
			        type = TreeFactory.getTreeType(name, color, texture)
			        tree = new Tree(x, y, type)
			        trees.add(tree)
			
			    method draw(canvas) is
			        foreach (tree in trees) do
			            tree.draw(canvas)
		```
		- **Intrinsic (고유한 상태, 내부의)**: 많은 객체에 걸쳐 복제된 상태, 외부에 의해 변경 불가한 상수
			↔ **Extrinsic (공유한 상태, 외인적인)**: 모든 원본 객체에 대해 고유한 상태, 외부에 의해 변경 가능
	- **Context**: 공유한 상태를 저장하며, Flyweight 객체에 대한 참조를 갖는 객체
	- **(선택 사항) Flyweight [[Factory (팩토리)]]**: Flyweight 풀을 편하게 관리하기 위한 방법, Client가 원하는 것과 일치하는 기존 객체가 있으면 반환하고 그렇지 않으면 새로 생성하여 풀에 추가 
	- **Client**: Flyweight의 메소드를 호출할 때 파라미터로 넘겨줄 값을 알기 위해 Context의 값을 저장/계산해야 함
- **상황**: 수많은 유사 객체 생성에 의해 RAM을 다 소모하는 문제가 있을 때만
- ❤️ RAM 절약
- 💔 **단점**
	1. Flyweight의 메소드를 호출할 때마다 Context의 값을 계산하게 될 경우, RAM 대신 CPU 주기를 낭비하게 될 수 있음 
	2. 복잡한 코드, 나쁜 [[Readability (가독성)]] 
### Proxy
[Proxy](https://refactoring.guru/ko/design-patterns/proxy)
📌 Service 객체와 [[Interface]]가 동일하고 필드를 통해 Service 객체를 참조하는 **Proxy 객체를 활용하여, Service 객체로 요청이 전달되기 전후로 추가 작업을 수행**하도록 하는 패턴
![[Proxy.png]]
- 📌 **용어**
	- **Service Interface**: Service가 따르는 [[Interface]]
		- ⭐ Proxy 또한 이를 따라야 함, **동일 [[Interface]]를 통해 Service 객체로 위장** 필요
		- 만들기 어려울 경우, Proxy가 Service를 [[Inheritance (상속)]]받는 식으로 해결도 가능  
	- **Service**: 유용한 비즈니스 로직을 제공하는 기존 클래스 
	- **Proxy**: 필드를 통해 Service 객체를 참조, Service 대신 요청을 받아 실제 작업 전후로 추가적인 처리를 함
		- 보통 Service 객체의 전체 수명 주기 또한 관리함 (또는, Client가 Proxy 생성자 매개변수로 Service를 전달해주기도 함)
	- **Client**: Service 객체를 기대하는 모든 코드에, 대신 Proxy 객체를 사용하여 Service의 기능을 사용 
		- Client가 Proxy와 작업할지 실제 Service와 작업할지를 결정하는 [[Factory (팩토리)]]를 두는 것이 좋음 
- **상황**: 기존 객체에 대한 접근을 제어하여, ⭐**기존 객체로 요청이 전달되기 전후로 추가 작업을 수행**해야 할 때 
	1. **지연된 초기화 (가상 프록시)**: 앱이 시작될 때 객체를 생성하는 것이 아닌, 실제 초기화가 필요한 시점까지 초기화를 지연시켜야 할 때
	2. **접근 제어 (보호 프록시)**: 중요한 Service 객체일 경우, 자격 증명이 기준을 통과한 특정 클라이언트일 때만 Service 객체에 요청을 전달하도록 보호해야 할 때
	3. **원격 서비스의 로컬 실행 (로컬 프록시)**: Service 객체가 원격 서버에 존재하여, 네트워크를 통해 요청을 전달하기 위한 작업이 필요할 때 
	4. **요청 로깅 (로깅 프록시)**: 요청 전달 전 로깅이 필요할 때
	5. **요청 결과 캐싱 (캐싱 프록시)**: 같은 결과를 생성하는 반복 요청의 경우, 요청의 매개변수를 키로 캐싱
	6. **스마트 참조**: Service 객체 또는 그 참조를 얻은 Client를 추적해야 할 때 (🔎 Client가 활성 상태인지, Service 객체 수정 여부)
- ❤️ **장점**
	1. Client가 모르는 상태에서 Service 객체 제어, 수명 주기 관리 가능
	2. Service 객체가 준비되지 않았거나 사용 불가능한 경우에도 Proxy는 동작
	3. Client, Service의 변화 없이 Proxy 추가/수정 가능 ➡ [[SOLID 원칙, 객체지향 설계 원칙#OCP (Open Close Principle, 개방-폐쇄 원칙)]]
- **💔 단점** 
    1. 복잡한 코드
    2. Service의 응답이 늦어질 수 있음 

# 행위 (Behavioral)
## 클래스 행위 패턴
### Interpreter
[Interpreter](https://johngrib.github.io/wiki/pattern/interpreter/)
📌 **언어의 문법에 대한 표현(= Terminal, Nonterminal)** 을 정의하면서, 표현을 통해 해당 언어로 기술된 문장을 해석하는 **해석기(= Interpreter)** 를 함께 정의할 수 있는 패턴
![[Interpreter Pattern.png]]
- 📌 **용어**
	- **Abstract Expression**: 추상 구문 트리에 속한 모든 노드에 해당하는 클래스들이 공통으로 가져야 할 Interpret 메소드를 갖는 [[Interface]]
	- **Terminal Expression**: 문법에 정의된 터미널 기호와 관련된 해석 방법 구현
	- **Nonterminal Expression**: 문법의 오른쪽에 나타나는 모든 기호 (즉, 터미널이 아닌 모든 기호) 에 대한 클래스 정의 
	- **Context**: 입력 등 번역기에 대한 포괄적인 정보 
	- **Client**: 언어로 정의된 특정 문장을 나타내는 추상 구문 트리, Terminal과 Nonterminal의 인스턴스로 구성 
- **상황**
	1. [[#Composite (복합체)]] 패턴이 사용되고, 이로 정의된 클래스들이 하나의 언어 구조를 정의할 때
	2. OO 컴파일러를 구현할 때 (특히 적당히 단순하고 복잡한, 10개 이하의 클래스로 구현 가능한 언어의 문법일 경우)
### Template Method
[Template Method](https://refactoring.guru/ko/design-patterns/template-method)
📌 **알고리즘을 일련의 Step**으로 나누어 각각을 메소드로 정의하고 이 **Step들을 특정 순서로 호출하는 Template Method**를 Abstract Class에 정의하는 패턴, 각 Step은 자식 클래스(= Concrete Class)에서 [[Polymorphism (다형성)#Overriding (오버라이딩)]]할 수 있음 
![[Template Method.png]]
- 📌 **용어**
	- **Abstract Class (추상 클래스)**: Template Method 및 각 단계에 해당하는 메소드들을 선언한 클래스 
		- **Template Method**: 알고리즘을 각 단계별로 세분화한 메소드를 특정 순서로 호출하여 동작하는 단일 메소드
			- ⭐ **`final`로 선언 ➡ 자식 클래스의 [[Polymorphism (다형성)#Overriding (오버라이딩)]] 방지** 
		- **Abstract Step**: `abstract`으로 선언되어 자식 클래스에게 자체 구현을 강요하는 단계
		- **Optional Step**: Default 구현이 존재하여 필요에 따라 [[Polymorphism (다형성)#Overriding (오버라이딩)]]할 수 있는 단계
			- **Hook**: Body가 Empty인 Optional Step
				- **주요 알고리즘 단계 사이에 추가 ➡ 자식 클래스에게 추가 확장 가능 지점을 제공** 
	- **Concrete Class (구상 클래스)**: 각 단계에 해당하는 메소드를  [[Polymorphism (다형성)#Overriding (오버라이딩)]]할 수 있는 클래스
- **상황**
	1. Monolithic (모놀리식) 알고리즘을 각 단계로 세분화하여, **부모 클래스에서 정의된 구조(= Template Method)를 그대로 유지하면서 알고리즘의 특정 단계만 확장**할 수 있도록 제한을 두고 싶은 경우
	2. 유사한 알고리즘을 포함하는 여러 클래스가 존재하는 경우 (코드 중복이 존재하는 경우)
- ❤️ **장점**
	1. Client가 대규모 알고리즘의 특정 부분만 [[Polymorphism (다형성)#Overriding (오버라이딩)]] 가능, 다른 부분의 알고리즘 변경에 영향을 덜 받을 수 있음 
	2. 중복 코드 제거 (부모 클래스로 이동)
- 💔 **단점**
	1. 알고리즘의 제한된 골격(= Template Method)로 인해 Client가 제한될 수 있음
	2. 자식 클래스를 통해 Default 단계 구현 억제 ➡ [[SOLID 원칙, 객체지향 설계 원칙#LSP (Liskov Substitution Principle, 리스코프 치환 원칙)]] 위반
	3. 단계가 많을수록 유지하기 어려움

## 객체 행위 패턴 
### Chain of Responsibility (CoR, Chain of Command)
[Chain of Responsibility](https://refactoring.guru/ko/design-patterns/chain-of-responsibility)
📌 각 행동을 동일한 [[Interface]](= Handler)를 따르는 Concrete Handler로 구현하고, **각 Concrete Handler에서 요청을 처리하고 다음 핸들러로 전달하거나 종료하는 방식을 구현**하여, **런타임에 핸들러 체인을 구성**할 수 있도록 하는 패턴 
![[Chain of Responsibility 3.png]]
- 📌 **용어**
	- **Handler**: 모든 Concrete Handler에 대한 공통 [[Interface]]
		- **`handler(req)`**: 요청을 처리하기 위한 단일 메소드
			- req 데이터를 Client에서 메소드로 전달하는 방법 정의 필요 (Default, 요청 객체를 인수로 전달) 
		- **`next`**: 다음 Handler의 참조를 저장하기 위한 필드
			- 생성자 또는 Setter에 Handler를 전달하여 체인 구축 가능 
		- (선택사항) **`setNext(h: Handler)`**: 체인의 다음 Handler 세팅을 위한 메소드
			- 런타임에 체인을 수정하고 싶을 경우 정의 필요 
	- (선택사항) **Base handler**: 모든 Handler의 공통 코드, Default Handler 구현 가능 
		- 또는, 이 클래스 없이 Handler [[Interface]]를 `abstract`으로 정의할 수도 있음
	- **Concrete Handler**: 각 요청 처리를 위한 실제 코드, **각 행동을 독립 실행형 객체로 변환**한 것 
		- ⭐**어떻게 요청을 처리할지, 다음 체인으로 요청을 전달할지 결정 필요**  
			- 각 Concrete Handler는 체인의 다음 핸들러에 대한 참조를 가짐, 요청을 처리한 뒤 다음 핸들러로 전달하거나 처리 중지 
		- 자체 포함, **불변**, 필요한 모든 데이터는 생성자로부터 한 번만 받아옴
	- **Client**: 체인을 정적/동적으로 구성
		- [[Factory (팩토리)]]를 통해 설정에 따라 체인을 구축할 수도 있음
		- **체인의 첫 번째가 아닌 핸들러에도 요청 전달 가능**
			- 전달된 요청은 어떤 핸들러가 요청 전달을 거부하거나 체인 끝에 도달할 때까지 전파됨
- **상황**
	1. 다양한 요청을 다양한 방식으로 처리하게 될 예정이지만 그 유형/순서를 미리 알 수 없을 때 때
	2. 핸들러의 집합과 그 순서가 런타임에 변경되어야 할 때
	3. 여러 핸들러를 특정 순서로 실행해야 할 때
		![[Chain of Responsibility.png]]
	4. 객체 트리 [[#Composite (복합체)]] (🔎 GUI) 
		![[Chain of Responsibility 2.png]]
		- 요청을 처리할 수 있는 경우 다음 핸들러로 넘기지 않고 종료, 결과적으로 하나 이하의 핸들러가 요청을 처리하게 됨  
- ❤️ **장점**
	1. 요청의 처리 순서 제어 가능
	2. 작업을 호출하는 클래스와 수행하는 클래스 분리 가능 ➡ [[SOLID 원칙, 객체지향 설계 원칙#SRP (Single Responsibility Principle, 단일 책임 원칙)]]
	3. 기존 Client를 수정하지 않고 새 Handler 도입 가능 ➡ [[SOLID 원칙, 객체지향 설계 원칙#OCP (Open Close Principle, 개방-폐쇄 원칙)]]
- 💔 처리되지 않는 요청이 있을 수 있음
### Command
[Command](https://refactoring.guru/ko/design-patterns/command)
📌 Invoker 객체에서 Receiver 객체로 요청을 직접 보내는 것이 아닌, **Invoker 객체의 필드에 중간에서 두 객체를 이어주는 링크 객체(= Concrete Command)의 참조를 두어 작동**시키는 패턴 
![[Command.png]]
- 📌 **용어**
	- **Invoker (발송자)**: Receiver에게 직접 요청을 보내는 것이 아닌, `command()`를 호출하여 요청을 시작하는 역할 (🔎 인터페이스의 한 요소)
		- 필드를 통해 Concrete Command 객체에 대한 참조를 가짐, [[Interface]](= Command)를 통해서만 Concrete Command 객체들과 통신
		- Concrete Command 객체를 생성할 책임 없음, 주로 Invoker를 생성할 때 Client로부터 Concrete Command의 참조를 생성자로 넘겨받게 됨 
	- **Command**: 커맨드 실행을 위한 [[Interface]], 일반적으로 `command()`처럼 **단일 실행 메소드만 선언**
		- **`command()`에는 매개변수 없음**, 다른 방법으로 데이터를 가져와야 함  
			1. **필드를 정의하고 Command 생성자**에서 미리 초기화 ➡ 불변  
			2. **자체적으로 데이터를 가져**올 수 있도록 설정 
		- 실제 요청을 수행할 Receiver 객체에 대한 참조 필요 
	- **Concrete Command**: 각 유형의 요청을 비즈니스 논리 객체에 전달하는 구현체, 각 요청별로 하나의  
	- **Receiver (수신자)**: 비즈니스 로직이 포함된 객체, Command로부터 받은 정보를 토대로 실제 작업 수행 
	- **Client**: 모든 요청 매개변수를 Concrete Command 생성자에 전달하여 생성, Invoker에게 전달  
		- **객체 초기화 순서** (참조 순서 Invoker ➡ Command ➡ Receiver 와 반대)
			1. Receiver 생성
			2. Concrete Command 생성, 이를 Receiver와 연결
			3. Invoker 생성, 이를 Concrete Command와 연결
- **상황**
	1. [[SOLID 원칙, 객체지향 설계 원칙#Separation of Concerns (관심사의 분리)]]에 따라 레이어가 분리되어 있고, 두 레이어 사이 요청을 전달할 필요가 있을 때 
		- 🔎 인터페이스와 비즈니스 로직의 분리
			![[Command 2.png]]
	1. 작업들로 객체를 매개변수화할 때
	2. 작업의 실행을 예약하거나, 대기열에 추가하거나, 로그 기록, (네트워크) 원격으로 실행하려고 할 때 
		- 📌 **직렬화**: 파일 및 DB에 쉽게 쓸 수 있는 문자열로 변환하는 행위, 객체들의 특성 
			- **실행 예약, 대기열에 추가, (네트워크) 원격 실행** ⬅ 문자열로 변환되었던 커맨드를 객체로 복원
	3. 되돌릴 수 있는 작업을 구현해야 할 때 
		- 🔎 편집기, 에디터 ➡ undo/redo 기능 구현에 가장 많이 사용되는 패턴
			![[Command History.png]]
			- **Command History**: 앱 상태의 백업 및 실행된 모든 Command 객체를 포함하는 Stack 
				- 커맨드 작업 실행 전, 앱 상태의 백업 복사본 생성
				- 실행 후, 실행한 Command 객체를 백업 복사본과 함께 Command History에 push
		- 💔 **단점**: 상태 백업 관련
			1. 앱 일부가 비공개일 경우, 상태 백업이 어려울 수 있음 ➡ [[#Memento]] 패턴으로 완화 
			2. 상태 백업에 상당히 많은 RAM 소모 ➡ 대안 구현으로 대체해야 할 수 있음 (대안에도 대가 존재, 구현 어렵거나 불가능할 수도)
				- 🔎 백업본을 통해 복원하는 대신, 커맨드가 작업을 역으로 수행 
- ❤️ **장점**
	1. [[SOLID 원칙, 객체지향 설계 원칙#SRP (Single Responsibility Principle, 단일 책임 원칙)]] ⬅ Invoker로부터 Receiver 분리
	2. [[SOLID 원칙, 객체지향 설계 원칙#OCP (Open Close Principle, 개방-폐쇄 원칙)]] ⬅ 기존 Client를 손상하지 않고 새 Concrete Command 도입 가능
	3. **Command History** (⊂ Stack) ➡ undo/redo 구현 가능
	4. **직렬화** ➡ 작업의 실행 지연, 예약, 대기열 추가, 원격 실행 등 가능 
	5. 간단한 커맨드 집합을 복잡한 커맨드로 조합 가능 
- 💔 코드가 더 복잡해질 수 있음 ⬅ Invoker와 Receiver 사이 새 레이어가 추가됨
### Iterator
[Iterator](https://refactoring.guru/ko/design-patterns/iterator)
📌 각 Concrete Collection의 세부 자료구조를 노출하지 않고, **(`getNext()` 메소드를 통해) 하나씩 순회할 수 있는 방법(= Concrete Iterator)을 [[Data Abstraction, Encapsulation (캡슐화), Information Hiding (정보 은닉)]]하여 제공**하는 패턴 
- [[SOLID 원칙, 객체지향 설계 원칙#Separation of Concerns (관심사의 분리)]] ➡ 컬렉션의 주요 책임은 **효율적인 데이터 저장**, 순회 알고리즘이 추가될수록 책임이 모호해짐, 또한 Client에 따라 원하는 순회 방식이 다를 수 있음 ➡ 각 순회 방법을 별도 객체로 책임 분리 필요 **= Iterator**
![[Iterator.png]]
- 📌 **용어**
	- **Iterator**: Collection 순회에 필요한 작업들을 선언한 공통 [[Interface]]
		- ⭐ **이번 순서의 요소를 리턴하는 메소드 `getNext()`** 필수
		- 그 외, 이전 요소를 가져오거나, 현재 위치를 추적하거나, 순회의 끝인지 확인하는 메소드 등을 추가할 수도 있음 
	- **Concrete Iterator**: 특정 순회 알고리즘 구현
		- 각 Concrete Iterator 객체는 **서로 독립적으로 순회**, **순회의 진행 상황 자체적으로 추적** 필요
		- Concrete Iterator의 생성자를 통해 단일 Concrete Collection과 연결됨 
	- **Collection**: Client에게 특정 Concrete Collection에 대한 Concrete Iterator를 생성할 수 있는 바로 가기를 제공하는 [[Interface]]
		- ⭐ **Iterator를 리턴해주는 메소드 `createIterator()`** 선언
	- **Concrete Collection**: `createIterator()`를 구현하여, Client가 요청할 때마다 호환되는 Concrete Iterator의 새 인스턴스 반환 
		- Concrete Collection을 노출하고 싶지 않을 경우, 해당 컬렉션에 대한 Iterator만 전달하면 됨 
	- **Client**: Concrete 객체들과 직접 결합하지 않고, **[[Interface]](= Iterator, Collection)를 통해 동작** 
		- 자체 특수 Concrete Iterator를 정의할 때가 아니라면 보통은 Client에서 Concrete Iterator를 생성하지 않음, Collection의 메소드를 통해 리턴받아 가져옴 
		- 특별한 순회 방법이 필요할 경우 자체 특수 Concrete Iterator만 정의하면 됨, Collection이나 Client 코드에 변화 생기지 않음
- **상황**
	1. Collection 내부의 복잡한 데이터 구조를 Client로부터 숨기고 싶을 때 ➡ [[Data Abstraction, Encapsulation (캡슐화), Information Hiding (정보 은닉)]]
		- ❤️ 보안, 편의성
	2. 순회 코드의 중복 제거 ➡ [[SOLID 원칙, 객체지향 설계 원칙#SRP (Single Responsibility Principle, 단일 책임 원칙)]], [[Maintainability (유지보수성)]]
	3. 여러 데이터 구조를 순회해야 할 때, 데이터 구조의 유형을 미리 알 수 없을 때
- ❤️ **장점**
	1. [[SOLID 원칙, 객체지향 설계 원칙#SRP (Single Responsibility Principle, 단일 책임 원칙)]] ➡ 
	2. [[SOLID 원칙, 객체지향 설계 원칙#OCP (Open Close Principle, 개방-폐쇄 원칙)]] ➡ 
	3. 같은 컬렉션을 병렬로 순회 가능 ⬅ 각 Concrete Iterator 객체에 고유한 순회 상태 저장되어 있음 
	4. 순회 지연 및 필요할 때 멈춘 위치에서부터 다시 순회 가능  
- 💔 **단점**
	1. 앱에서 사용하는 컬렉션들이 단순할 경우, 과도함 
	2. 특수 컬렉션의 요소들을 직접 탐색하는 것이 더 효율적일 수 있음
### Mediator (중재자)
[Mediator](https://refactoring.guru/ko/design-patterns/mediator)
📌 Component 간 서로를 모른 채, **참조를 통해 Concrete Mediator의 `notify`를 호출하면, 모든 Concrete Mediator에서 직접 요청을 처리하거나 다른 Component를 참조하여 노티를 리다이렉트**하여 동작하도록 하는 패턴
![[Mediator.png]]
- 📌 **용어**
	- **Component**: 특정 비즈니스 로직을 포함한 클래스
		- **Mediator Interface에 대한 참조**를 가짐
			- 일반적으로 Component를 생성할 때 생성자에 인수로 전달
		- Mediator 타입이므로 다른 Concrete Mediator로 갈아끼울 수 있음 ➡ ❤️ 재사용성
		- Component에게 중요한 일이 생기면 Mediator에게만 노티해야 함 ➡ **❤️ Component 간 [[Coupling (결합도)]] 감소, 독립성, [[Reusability (재사용성)]], [[Maintainability (유지보수성)]]** 
	- **Mediator (중재자)**: [[Interface]], 일반적으로 단일 알람 메소드 `notify`만을 포함 (더 복잡한 노티가 필요할 경우 원하는 통신 프로토콜 정의)
		- `notify`의 인수로 모든 종류의 콘텍스트를 전달 가능하나, **송신자와 수신자 Component 간 [[Coupling (결합도)]]가 없어야** 함  
		- Mediator는 노티 송신 Component가 누군지 식별 가능하며, 이를 통해 자체적으로 요청을 처리하거나 어떤 Component에게 리다이렉트할지 (= 수신자 Component가 누구인지) 결정 가능
			↔ **송신자 및 수신자 Component, Black Box**, 그냥 노티를 보내고 받을 뿐 어떤 Component와 소통한 것인지 알지 못함 ➡ **❤️ 상호 의존성 제거**
	- **Concrete Mediator**: 다양한 **Component간의 관계를 [[Data Abstraction, Encapsulation (캡슐화), Information Hiding (정보 은닉)]]**
		- 필요한 Component에 대한 참조 유지
		- (선택 사항) Component들의 수명 관리 
			- ~= [[#Abstract Factory]] [[#Facade (퍼사드)]]
- **상황**
	1. 일부 클래스가 다른 클래스와 단단하게 [[Coupling (결합도)]]할 때
		- **Component 간의 모든 관계를 별도 클래스(= Concrete Mediator)로 추출** 가능 ➡ 특정 Component의 변경을 다른 Component로부터 고립 가능 ➡ [[SOLID 원칙, 객체지향 설계 원칙#OCP (Open Close Principle, 개방-폐쇄 원칙)]]
	2. Component간 의존 관계 때문에 다른 앱에서 **Component 재사용이 어려울 때**
		- 개별 Component는 서로를 인식하지 못하게 됨 
		- 다른 앱에서 재사용할 때, 새로운 Concrete Mediator를 제공하면 됨 
	3. 다양한 컨텍스트에서 기본 행동들을 재사용하기 위해, 수많은 Component 자식 클래스가 생기게 되는 경우 
		- Component를 변경하거나 [[Inheritance (상속)]]하는 것이 아닌, Concrete Mediator를 통해 협업 방법을 정의 
- **❤️ 장점**
	1. [[SOLID 원칙, 객체지향 설계 원칙#SRP (Single Responsibility Principle, 단일 책임 원칙)]], [[Readability (가독성)]], [[Maintainability (유지보수성)]] ⬅ Component 간 통신을 한 곳(= Mediator)으로 추출
	2. [[SOLID 원칙, 객체지향 설계 원칙#OCP (Open Close Principle, 개방-폐쇄 원칙)]] ⬅ Component 변경 없이 새 Concrete Mediator 도입 가능
	3. Component 간 Loosely [[Coupling (결합도)]]
	4. Component [[Reusability (재사용성)]]
- 💔 Mediator가 [[SOLID 원칙, 객체지향 설계 원칙#God Object (신 객체)]]가 될 수 있음
### Memento
[Memento](https://refactoring.guru/ko/design-patterns/memento)
📌 Originator 객체의 `private` 필드를 공개하지 않으면서, 상태 스냅샷을 Originator 객체가 직접 Memento 객체로 생성하고 Caretaker 객체의 스택에 push하여 저장 및 복원할 수 있도록 해주는 패턴
![[Memento.png]]
- 📌 **용어**
	- **Originator (오리지네이터)**: 상태를 실제 소유하고 있는 객체
		- ⭐ **상태 스냅샷 생성** 책임을 여기에서 ➡ 외부 객체에서 복사 시도하지 않아도 됨, `private` 필드 유지 가능
	- **Memento**: 상태 스냅샷 역할을 하는 객체, 또는 그러한 객체의 [[Interface]]
		- Originator 내부의 상태 필드들을 미러링 
		- **Originator에서만 Memento의 상태에 접근 가능** ➡ Originator에서는 언제든지 undo 가능 
			↔ Caretaker 및 외부 객체들에서 접근 불가, 제한된 Interface로만 통신 (제한된 메타데이터에만 접근 가능)
		- 관행적으로 **⭐불변, 생성자를 통해 Originator로부터 한 번만 상태들을 전달받음**
	- **Caretaker (케어테이커, 간호인, 관리인)**: (여러 개일 경우, Stack에) Memento를 저장하는 객체 
		- 복사본 저장이 필요할 때, Originator가 생성한 Memento를 리턴받아 저장
			- Caretaker에서 **Originator에 대한 참조를 필드에 저장**해야 함
		- undo가 필요할 때, **Stack에서 Memento를 pop한 뒤 Originator의 `restore` 메소드 인수로 전달** ➡ Originator는 Memento에 접근 가능하므로, 과거 상태값들을 가져와 복구 가능 
		- Originator로부터 상태 캡쳐를 요청해야 하는 타이밍과 이유, 특정 Memento로부터 Originator 복원 시기 등을 알고 있어야 함
- **구현**
	1. **중첩 클래스에 기반한 구현** (🔎 C++, C#, JAVA)
		![[Memento Implementation.png]]
		- Originator 내부에 **Memento를 중첩 클래스로 선언** ➡ Originator에서 Memento의 `private` 필드들에도 접근 가능 
	2.  **중간 인터페이스에 기반한 구현** (= 중첩 클래스를 지원하지 않는 경우의 대안, 🔎 PHP)
		![[Memento Implementation 2.png]]
		- Client = Caretaker
		- Caretaker와 Concrete Memento 사이에 [[Interface]]를 두고, 이를 통해서만 작업할 수 있도록 함 (메타데이터와 관련된 멤버만 선언)
			- Originator 외의 객체들과는 모두 Memento Interface로만 소통 
			- Originator에서 Concrete Memento를 생성할 때, Memento Interface 타입으로 반환해야 함 
				- Originator의 `restore` 메소드에서 Memento 타입 객체를 받으면, **Concrete Memento 타입으로 타입캐스팅** 필요 (Concrete Memento의 모든 멤버에 접근해야 함)
		- Originator는 Memento와 직접 작업 ➡ **💔Concrete Memento의 모든 멤버를 `public` 선언**해야 함 
	3. **더 엄격한 [[Data Abstraction, Encapsulation (캡슐화), Information Hiding (정보 은닉)]]를 사용한 구현** (= 다른 클래스들이 Memento의 상태에 접근할 가능성을 완전 배제하고자 할 때)
		![[Memento Implementation 3.png]]
		- Originator와 Memento를 [[Interface]]로 둚, Caretaker에서 Concrete Originator를 참조하지 않고 **각각의 Concrete Memento가 자신을 생성한 Concrete Originator 참조**
			- Originator에서 Memento를 생성할 때, 상태값과 더불어 **자신 스스로를 생성자 인수로 전달** 
		- ❤️ **장점**
			1. 여러 유형의 Concrete Originator와 Concrete Memento 보유 가능
			2. 각각은 자신의 상태를 외부에 노출하지 않음 
			3. **`restore` 메소드를 Memento로 이동** 가능 ➡ Caretaker에서 Originator를 참조할 필요 없음, 독립
		- 💔 Memento 클래스가 Originator에 중첩되거나, Originator가 Memento의 상태를 [[Polymorphism (다형성)#Overriding (오버라이딩)]]하기 충분한 `setter`를 제공할 때에만 의미 있음 
- **상황**
	1. `private` 필드를 공개하지 않으면서, 이전 상태 스냅샷을 저장 및 복원할 필요가 있을 때 (필수로 필요한 패턴)
		- 🔎 **undo/redo, [[Transaction (트랜잭션)]] rollback** 
	2. 객체의 필드/`getter`/`setter`를 직접 접근하는 것이 [[Data Abstraction, Encapsulation (캡슐화), Information Hiding (정보 은닉)]]을 위반할 때
		= 일부 객체들이 원래 해야 할 일보다 더 많은 일을 수행하려 할 때 (여기서는, 다른 외부 객체)
		= `private` 정보들을 공개하면 너무 취약해지고, 공개하지 않으면 작업이 불가능할 때 
		-  **객체가 스스로 자신의 스냅샷 생성** ➡ 다른 객체는 그를 읽을 수 없으므로 안전 
	3. **편집기 ([[#Command]] 패턴 함께 적용)**
		![[Memento 2.png]]
		- Editor = Originator, Snapshot = Memento, **Command = Caretaker**
			- Command의 작업 실행 전, Originator가 생성한 Memento를 리턴받아 저장
				= 해당 Command를 실행하기 전 상태의 복사본
			- Command를 undo할 때, Command에 저장된 Memento를 통해 이전 상태로 복구 
				- **Memento는 Originator를 참조** ➡ 자신의 `restore` 메소드에서  Originator의 상태 복구 가능 
- **❤️ 장점**
	1. [[Data Abstraction, Encapsulation (캡슐화), Information Hiding (정보 은닉)]] 위반하지 않고 객체 스냅샷 생성 가능
	2. Originator 코드 단순화 ⬅ Caretaker에서 Memento 기록 관리 
- **💔 단점**
	1. Memento를 자주 생성할 경우, 많은 RAM 소모
	2. 쓸모 없는 Memento 파괴 필요 ➡ Originator 수명주기 추적 필요
	3. 대부분의 동적 프로그래밍 언어(🔎 PHP, Python, JavaScript)에서는 Memento 상태의 불변 보장 불가 
### Observer, Event-Subscriber
[Observer](https://refactoring.guru/ko/design-patterns/observer)
📌 상태를 갖고 있는 Subject/Publisher와 Observers/Subscribers 간의 1:多 관계를 정의하여, **Publisher의 상태가 바뀌었을 때 모든 Subscriber의 `update` 메소드를 호출하여 변화를 자동으로 알리는** 패턴
![[Observer Pattern.png]]
![[Observer.png]]
- 📌 **용어**
	- **Subject, Publisher (출판사)**: **비즈니스 로직 중 핵심 기능**을 포함하는 객체 (또는 그러한 객체들의 [[Interface]]), 시간이 지남에 따라 상태가 변경될 수 으며 그를 Subscriber들에게 알리게 됨 
		- `subscibe` 메소드의 인자로 넘겨받은 Subscriber의 참조를 배열에 저장, `unsubscribe`로 넘겨받은 Subscriber는 배열에서 삭제
			- `subscribe` 메소드 및 구독 리스트의 경우 모든 Publisher에서 유사 ➡ Publisher Interface에서 파생된 `abstract` 클래스에 공통 구현을 넣고, 모든 Publisher가 이를 [[Inheritance (상속)]]하도록 구현
			- 기존 클래스 계층 구조가 존재하고 여기에 패턴을 적용하는 경우, [[Inheritance (상속)]] 대신 [[Composition (합성)]] 고려하기 (= 별도 객체에 `subscribe` 로직을 넣고, 이를 모든 Publisher들이 참조하여 사용하도록)
		- **⭐ 상태가 변경되었을 때, `notifySubsrcibers` 호출을 통해 모든 Subscriber에게 변경 사항 노티**
			- **내부에서 모든 Subscriber의 `update` 호출**
		- #Java  `extends Observable`
	- **Observer, Subscriber (구독자)**: 일반적으로 **⭐단일 `update` 메소드**가 선언된 [[Interface]] (또는 더 복잡한 알림 Interface 선언도 가능)
		- Publisher에서 `update`를 호출할 때, **`update`의 매개변수**를 통해 컨텍스트 데이터, 특정 이벤트의 세부 정보 등 전달 가능
			- 또는, **Publisher 객체 스스로를 전달**하여 Subscriber에서 직접 모든 데이터를 가져올 수 있도록 할 수 있음 
		- 함수형 타입을 제공하는 언어의 경우, Subscriber 계층 구조를 함수들의 집합으로 바꿀 수 있음 
		- #Java  `implements Observer` + `abstract update` 메소드 구현
	- **Concrete Observer, Subscriber (구상 구독자)**: Publisher가 보낸 노티에 대한 응답으로 다양한 작업 수행
	- **Client**: Publisher 및 Subscriber 객체 생성 후, Subscriber를 Publisher에 등록 (= `subscribe` 메소드의 인자로 적절한 Subscriber 넘겨줌)
- **상황**
	1. 한 객체의 상태 변경으로 다른 객체들도 변경해야 할 때, **이러한 객체 집합을 미리 알기 어렵거나 동적으로 변경될 때** 
		- 🔎 GUI 클래스와 작업할 때 ➡ Publisher와 사용자 정의 버튼 클래스를 위한 Subscriber Interface를 생성하면, Client들이 사용자 정의 코드 연결 가능 
	2. 앱의 일부 객체들이 **제한된 시간 동안만, 특정 경우에만 다른 객체를 관찰**해야 할 때
		- 구독 리스트는 동적 변경 가능, 필요할 때만 `subscribe`하면 됨 
- **❤️ 장점**
	1. [[SOLID 원칙, 객체지향 설계 원칙#OCP (Open Close Principle, 개방-폐쇄 원칙)]] ⬅ Publisher 코드를 변경하지 않고 새 Subscriber 도입 가능 (Publisher의 [[Interface]]가 있다면, 그 역도 성립)
	2. **객체 간 관계를 런타임에 형성** 가능
- 💔 Subscriber는 무작위로 알림을 받게 됨
### State
[State](https://refactoring.guru/ko/design-patterns/state)
📌 각 상태별로 관련 로직을 Concrete State 클래스로 분리하고, 상태에 따라 조건부로 행동을 선택하던 원래 **Context 객체에서 Concrete State에 대한 참조를 통해 상태별 작업을 위임**하는 패턴 
↔ 일반적으로, **[[Finite State Machine (FSM, 유한 오토마타)]]를 구현할 때 조건문** 활용
- **상태에 따라 행동을 선택하는 거대한 조건문**이 대부분의 메소드에 포함될 것 ➡ 💔**코드 중복, Bad [[Maintainability (유지보수성)]]**
![[State.png]]
- 📌 **용어**
	- **Context**: 상태에 따라 조건부로 행동을 선택하던 원래 객체
		- ⭐ **Concrete State 중 하나에 대한 참조를 저장하고, 모든 상태별 작업 위임** 
		- [[Polymorphism (다형성)#Overriding (오버라이딩)]] 가능한, **State 참조를 변경할 수 있는 `setter`** 추가 ➡ Context, Concrete State, Client 등 아무 곳에서나 상태 변이 가능
			- **상태 변이** = Concrete State 생성 후, Context에 연결된 State 참조 교체 
	- **State**: 모든 상태에서 사용될, 상태별 메소드를 선언한 [[Interface]]
	- **Concrete State**: 각 상태별 메소드에 대한 구현을 제공하는 객체 
		- **공통 행동이 있을 경우, 중간에 `abstract` 클래스**를 두어 [[Data Abstraction, Encapsulation (캡슐화), Information Hiding (정보 은닉)]]
		- **Context 객체에 대한 참조** 저장 ⬅ Context 객체로부터 필요한 정보 가져올 수 있음, 상태 전이 가능 
		- ⚠ **구현이 Context의 `private` 필드에 의존**할 경우 
			1. 필드를 `public`으로 전환
			2. **구현을 Context의 `public` 메소드로 정의**한 뒤, Concrete State에서 호출 ➡ ❤️빠르게 작성 가능, 이후에 언제든 수정 가능, 💔보기 흉함
			3. Context 클래스 안에 **Concrete State를 중첩 클래스로 정의**  
- **상황**
	1. 현재 상태에 따라 다르게 행동하는 객체가 있을 때, **상태의 수가 많을 때, 상태별 코드가 자주 변경될 때**
		- 각 상태별 코드를 서로 다른 클래스로 추출 ➡ ❤️[[Maintainability (유지보수성)]], Concrete State가 서로 독립적이므로 추가 및 변경 용이  
	2. **상태에 따라 행동을 선택하는 거대한 조건문**으로 클래스가 오염된 때
		- 거대한 조건문의 **각 브랜치를 각 상태 클래스의 메소드로 추출** 가능 
		- 조건문 대신 Concrete State 메소드의 호출로 변경 
	3. **유사한 상태별로 중복 코드**가 많을 때, 조건문 기반 전이가 많을 때
		- Concrete State의 계층 구조 구성 가능
		- **State의 공통 코드를 부모 `abstract` 클래스로** 추출 ➡ ❤️중복 제거 
- **❤️ 장점**
	1. [[SOLID 원칙, 객체지향 설계 원칙#SRP (Single Responsibility Principle, 단일 책임 원칙)]] ⬅ 특정 상태들과 관련된 코드를 별도 클래스로 구성 
	2. [[SOLID 원칙, 객체지향 설계 원칙#OCP (Open Close Principle, 개방-폐쇄 원칙)]] ⬅ 기존 Context, Concrete State 변경 없이 새 상태 도입 가능 
	3. Context 코드 단순화 ⬅ 거대한 조건문의 각 브랜치를 Concrete State로 나눔 
- 💔 상태가 몇 개 없거나, 상태별 코드가 거의 변경되지 않을 경우 과도함 
### Strategy
[Strategy](https://refactoring.guru/ko/design-patterns/strategy)
📌 같은 행동에 대한 여러 세부 알고리즘이 있고 그 중 하나를 런타임에 선택하도록 할 때, **각 세부 알고리즘마다 Concrete Strategy 클래스를 만들어 구현한 뒤 원본 Context 객체에서 이를 참조**하여 세부 알고리즘 동작을 위임시키는 패턴
![[Strategy.png]]
- 📌 **용어**
	- **Context**: 특정 역할을 수행하는 세부 알고리즘 중 하나를 선택해서 실행하던 원래 클래스
		- ⭐ **Concrete Strategy 중 하나에 대한 참조를 저장**하고, 특정 알고리즘이 필요할 때 참조하고 있는 Concrete Strategy 객체에 작업 위임 
			- Concrete Strategy에 대해 알지 못함, 그저 **Strategy [[Interface]]를 통해 작동** 
		- Concrete Strategy 선택 책임 없음, **`setter`를 통해 Client로부터 특정 객체 전달받음**
		- Strategy에서 Context의 데이터에 직접 접근해야 할 경우, Context [[Interface]]를 정의해도 됨
	- **Strategy**: Context에서 호출할, 모든 세부 알고리즘에 대해 공통인 메소드가 선언된 [[Interface]]
	- **Concrete Strategy**: Strategy에 선언된 메소드를 세부 알고리즘으로 구현
	- **Client**: 원하는 Concrete Strategy를 생성하여 Context `setter`에 전달 
- **상황**
	1. 한 알고리즘의 다양한 변형을 사용하고 싶을 때, 런타임 중 다른 세부 알고리즘으로 전환하고 싶을 때 
	2. 같은 행동을 다른 방식으로 수행하는 유사한 클래스가 많을 때
		- 유사 클래스를 하나의 Context 클래스로 결합하고, 수행 방식별로 별도 Concrete Strategy 클래스로 분리 ➡ ❤️유사 클래스 간 중복 제거
	3. Context 클래스의 다른 비즈니스 로직들을 알고리즘의 각 세부 구현 사항으로부터 분리, 의존 관계 고립 
		- Client에서 간단한 Interface를 통해 알고리즘 런타임에 전환 가능
	4. 클래스 내부에서 **거대한 조건문**을 통해 알고리즘의 다른 변형을 전환했던 경우  
- ❤️ **장점**
	1. 런타임에 특정 객체 내부에서 사용되는 알고리즘 교환 가능
	2. 알고리즘의 구현 세부 정보를 고립시킬 수 있음
	3. [[Inheritance (상속)]]를 [[Composition (합성)]]으로 대체 가능
		- 각 세부 알고리즘을 구현할 때 Context 객체를 상속하여 구현하는 것이 아닌, 각 알고리즘 개수에 맞게 Concrete Strategy를 구현한 뒤 Context 객체가 참조하도록 함 
	4. [[SOLID 원칙, 객체지향 설계 원칙#OCP (Open Close Principle, 개방-폐쇄 원칙)]] ⬅ Context 변경 없이 새 Concrete Strategy 도입 가능 
- 💔 **단점**
	1. 세부 알고리즘 개수가 적거나, 거의 변하지 않는 경우 과함 (많은 클래스 및 Interface로 인해 복잡해질 수 있음) 
	2. Client에서 각 Concrete Strategy의 차이점을 잘 알고 있어야, 적절한 것 선택 가능
	3. 현대의 많은 프로그래밍 언어에서는, **익명 함수의 집합 내에서 다양한 버전의 알고리즘을 구현할 수 있는 함수형 타입** 지원 (즉, Strategy 패턴 적용으로 많은 클래스와 Interface를 추가할 필요 없음)
### Visitor
[Visitor](https://refactoring.guru/ko/design-patterns/visitor)
📌 기존 Concrete Element 객체들에 부가 기능들을 추가하기 위해 **`visit(ConcreteE e)`들을 [[Polymorphism (다형성)#Overloading (오버로딩)]]으로 정의한 Visitor 객체를 생성**하고, 각 **Concrete Element에 적합한 `visit` 메소드를 호출하여 요청을 리다이렉트해주는 `accept(Visitor v)`를 구현**하는 패턴
- [[Polymorphism (다형성)#Double Dispatch (이중 디스패치)]] 원칙 기반 (아래에 자세히 기술)
![[Visitor.png]]
- 📌 **용어**
	- **Visitor**: 원본 객체인 각 Concrete Element에 새롭게 추가하려는 작업, **Concrete Element를 파라미터로 받는 `visit(ConcreteE e)` 메소드들을 선언한 [[Interface]]**
		- **각 Concrete Element를 매개변수로 하는, 이름이 동일한 `visitor(ConcreteE e)` 메소드** 정의 (= **⭐[[Polymorphism (다형성)#Overloading (오버로딩)]]**)
	- **Concrete Visitor**: 각 버전별로, 모든 `visit(ConcreteE e)` 메소드 구현 
		- 구현이 Concrete Element의 `private`에 접근해야 할 경우 ➡ `public`으로 변경하여 [[Data Abstraction, Encapsulation (캡슐화), Information Hiding (정보 은닉)]]을 위반하거나, **Concrete Visitor를 Concrete Element 안에 중첩 클래스로 정의** 
	- **Element**: Visitor [[Interface]]를 파라미터로 받는 `accept(Visitor v)` 메소드를 선언한 Interface
		- 기존 Concrete Element 클래스들 간 계층구조 있을 경우, 부모 클래스에 `abstract accept(Visitor v)` 메소드 추가
	- **Concrete Element**: 원본 객체, **적절한 `visit(ConcreteE e)` 메소드를 호출**하도록 `accept(Visitor v)` 구현
		- ⭐ **[[Polymorphism (다형성)#Double Dispatch (이중 디스패치)]]**
			- `v.visit(Element e)`처럼 정의하게 되면, e를 [[Binding#Static Binding (정적 바인딩)]]하게 되므로 e로 넘겨받은 특정 Concrete Element가 아닌 부모Interface Element의 필드들만 참조 가능 
			- Client에서 직접 Concrete Element 타입에 따라 `visit` 메소드를 선택하지 않고, **적절한 `visitor` 메소드 선택을 Concrete Element로 위임** ➡ Client 코드에서는 그냥 `concreteE.accept(v)`처럼 작성
			↔ Client 코드에 조건문을 추가하여, 각 Concrete Element 타입에 맞는 `visitor` 메소드 호출
			```pseudo
			// Without Double DIspatch
			// Client code
			foreach (Node node in graph)
				if (node instanceof City)
					exportVisitor.doForCity((City) node)
				if (node instanceof Industry)
					exportVisitor.doForIndustry((Industry) node)
				// …

			// With Double Dispatch 
			// Client code
			foreach (Node node in graph)
			    node.accept(exportVisitor) 
	
			// City (Concrete Element)
			class City is
			    method accept(Visitor v) is
			        v.doForCity(this) // 적합한 visitor 메소드를 선택하여 호출 
				    // …
	
			// Industry (Concrete Element)
			class Industry is
			    method accept(Visitor v) is
			        v.doForIndustry(this)
				    // …
			```
	- **Client**: Collection 또는 [[#Composite (복합체)]] 트리와 같은 복잡한 객체
		- Concrete Element에 대해 알지 못함, Concrete Visitor 객체 생성 후 `concreteE.accept(v)`처럼 추상 Interface를 통해서만 작업 
- **상황**
	1. 복잡한 객체 구조(🔎 객체 트리)의 모든 요소에 대해 작업을 수행해야 할 때 
		- 여러 Concrete Element 집합에 동일한 작업 `e.accept(v)`을 실행할 수 있음
	2. Concrete Element의 주 작업이 아닌 다른 **보조 행동을 별도 클래스(= Visitor)로 분리**하고 싶을 때 
		- 🔎 파일 내보내기 ➡ 파일 확장자 별로 Concrete Visitor를 만들어 구현
			![[Visitor 2.png]]
		- ❤️ [[SOLID 원칙, 객체지향 설계 원칙#SRP (Single Responsibility Principle, 단일 책임 원칙)]]
	3. 클래스 계층구조의 일부 클래스에게만 의미 있는 행동일 때 
		- 해당 행동을 Visitor로 추출하고, 일부 클래스 각각을 파라미터로 받는 `visit` 메소드 정의
	4. 기존 Concrete Element 클래스를 최대한 덜 변경하면서 새 기능을 추가하고 싶을 때
		- 🔎 이미 프로덕션 단계에 있는 앱 
		- 모든 Concrete Element 클래스 각각에 로직을 추가하는 것이 아닌, Visitor 클래스로 뺀 뒤 Concrete Element에서는 특정 `visit` 메소드 중 하나를 선택해서 호출해주는 `accept(Visitor v)` 메소드만 구현 
			```pseudo
			method accept(Visitor v) is
				v.doForCity(this) // 적합한 visitor 메소드를 선택하여 호출 
				// …
			```
- ❤️ **장점**
	1. [[SOLID 원칙, 객체지향 설계 원칙#OCP (Open Close Principle, 개방-폐쇄 원칙)]] ⬅ 다른 Concrete Element를 변경하지 않으면서, 몇몇 Concrete Element와 동작하는 새 Visitor 도입 가능
	2. [[SOLID 원칙, 객체지향 설계 원칙#SRP (Single Responsibility Principle, 단일 책임 원칙)]] ⬅ 각 Concrete Element에 대한 같은 행동을 한 클래스로 이동 가능 
	3. 다양한 Concrete Element와 작업하며 여러 유용한 데이터 축적 가능 
		- 복잡한 객체 구조를 순회하며, 각 객체에 대해 Visitor 패턴을 적용하려는 경우 
- 💔 **단점**
	1. Concrete Element가 추가 및 제거될 때마다 Visitor를 업데이트해야 함 (최소한으로 변경하긴 했지만, 그럼에도 기존 Concrete Element 클래스들이 변경되어야 함)
	2. Concrete Visitor에서 작업하기 위해 필요한, Concrete Element의 `private`에 접근하기 위한 권한이 부족할 수 있음