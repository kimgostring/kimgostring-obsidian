#Design #ArchitecturePattern 

[MDN > MVC](https://developer.mozilla.org/ko/docs/Glossary/MVC)
📌 Model-View-Controller Pattern, GUI로 구성된 SW에 좋은 [[Maintainability (유지보수성)]]을 갖기 위한 하나의 해결책
![[MVC.png]]
- **목적**: [[SW Complexity (SW Crisis)]] 
	- ⭐ **UI의 변화가 Non-UI = [[#Model]]에게 전파되지 않도록** 하기 위함 ➡ Non-UI의 경우 UI의 [[Interface]]만 알고 **UI의 Module과는 독립성** 가짐 (UI는 Non-UI 알아도 상관 없음)
	1. Non-UI에 비해 **UI는 변화가 잦음**
	2. **같은 데이터를 다른 형태의 [[#View]]로** 보여줘야 하는 상황 존재
	3. UI 속도 빨라야 한다는 제약 존재
- **방법 및 목표**: [[SOLID 원칙, 객체지향 설계 원칙#Separation of Concerns (관심사의 분리)]] ➡ 핵심 비즈니스 모델, 데이터의 표현, 컨트롤 로직을 분리 
- ❤️ **장점**
	1. Minimize [[Coupling (결합도)]]
	2. [[Reusability (재사용성)]], [[Extensibility (확장성)]]
	3. [[Maintainability (유지보수성)]]
	4. [[Readability (가독성)]]
- 💔 **단점**
	1. Model과 View의 의존성을 완전히 
	2. **Massive View Controller**: 대규모 애플리케이션에서, 다수의 [[#Model]], [[#View]]가 Controller와 복잡하게 연결되는 것 
		![[Massive View Controller.png]]

## 구성
MVC를 사용할 경우, 전체 System이 크게 세 파트로 나뉘어짐 
### Model
📌 **데이터, 비즈니스 로직**을 담고 있는 객체
- **디자인 패턴**: [[GoF 디자인 패턴#Observer]] 
	- [[#View]]가 렌더링에 필요한 **Model의 State를 구독**받게 됨 
		- Model = Publisher/Subject
		- [[#View]] = Subscriber/Observer
- **제약**
	1. UI에 독립적 ➡ [[Study/SW Engineering/Test#Unit (Module) Test]] 쉬움  
	2. 빠른 속도 ↔ Model 수행 완료될 때까지 [[#View]]가 멈추게 됨
### View
📌 [[#Model]]의 데이터를 사용자에게 보여주는 객체 
- **수동적** ➡ [[#Controller]]에게 사용자 인풋 해석을 위임, [[#Model]]의 State 변화에 따라 렌더링 
- **패턴**: [[GoF 디자인 패턴#Strategy]] 
### Controller
📌 사용자 인풋을 받아 [[#Model]]을 업데이트하고, 해당 데이터를 [[#View]]로 전달하는 상호 작용을 위한 객체 
- [[#Model]]의 API를 적절히 호출 
- UI 기능 당 하나씩 생성
- **패턴**: [[GoF 디자인 패턴#Composite (복합체)]]

## Litmus Test
📌 UI Layer에서 [[#View]]와 [[#Controller]]의 구분이 모호할 때 사용하는 방법