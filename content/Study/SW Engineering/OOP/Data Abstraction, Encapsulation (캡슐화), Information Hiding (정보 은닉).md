#OOP 

📌 **복잡한 구현(How, Private view)은 감추고**, [[Interface]](What, Public view)는 공개하는 것 
⊂ [[Abstraction (추상화)]]
- 외부에서는 **오직 [[Interface]]를 통해서만 내부 데이터 통제** 가능 

## 장점
1. **[[Safety (안전성)]]**: 외부에서 함부로 Implemetation에 접근하지 못함
	- ↔️ 구현이 감춰지지 않았다면, 이해하기 위해 모든 접근 경로를 알고 있어야 함 ➡️ [[Readability (가독성)]]와도 이어짐
	- 함부로 변경 불가 ➡ [[Reliable (신뢰성)]]와도 이어짐 
2. ⭐ **[[Readability (가독성)]]**: 사용자가 복잡한 How를 볼 필요 없이, **What만 보고 쉽게 이해하고 쉽게 사용** 
3. ⭐ **[[Independence (독립성)]]**: 구현이 변경되더라도 외부에서는 전혀 알 수 없음
	- ⭐ **Propagation(전파)의 완충제**
		- 변하지 않는 것을 [[Interface]]에, 변하는 것을 Implement하여 새 Class로 캡슐화 
		- **Client에서 Implement가 아닌 [[Interface]]에 대해 [[UML#Dependency (의존)]]를 가지도록 함** ➡ 구현이 변화하더라도 Client에 영향 주지 않음 
	- [[Interface]]는 바꿀 수 없지만, 서로 독립적이라면 **[[Interface]]에 영향을 주지 않으므로 구현을 자유롭게 수정할 수 있음**
4. [[Maintainability (유지보수성)]]

## ADT (Abstract Data Type)
📌 추상 자료형, **구현과는 독립적**인 데이터 및 관련 연산의 집합
= Data + Operations **without Implementation**
= **하나의 문법적인 단위**에 관련 데이터와 op를 [[Data Abstraction, Encapsulation (캡슐화), Information Hiding (정보 은닉)]]한 것
- Class의 Idea 제시해 준 개념
### Class
📌 대부분의 [[OOP]] 언어에서, ADT를 정의하기 위한 문법
= Attribute + Behavior
= [[#ADT (Abstract Data Type)]] + [[Inheritance (상속)]] + [[Polymorphism (다형성)]]
↔ #C Structure, 동작 없이 데이터만 존재하는 묶음, Class가 더 확장된 개념임 
- **설계 지침**
	1. 하나의 클래스는 하나의 Entity를 기술 
	2. 책임 분리
	3. [[Reusability (재사용성)]] 고려
		- **특정한 가정 피하기** ➡ 순서 등, 외부 요인에 독립적으로 설계 
		- 지나친 최적화 피하기
### Object
📌 프로그램에서 표현된 하나의 Entity (개체)
[객지프 > OOP02-2](onenote:https://livecauac-my.sharepoint.com/personal/kay0315a_cau_ac_kr/Documents/Documents/2020%202학기%20객지프/PPT.one#OOP02-2&section-id={8BFD497C-C0A9-4038-86D3-841CFE660FB7}&page-id={5DD441D6-0057-4C2F-95B4-A494A5DAE780}&object-id={DFB8673E-466B-4EDD-9815-BD7E1EEB3EC1}&4E) ([웹 보기](https://livecauac-my.sharepoint.com/personal/kay0315a_cau_ac_kr/_layouts/OneNote.aspx?id=%2Fpersonal%2Fkay0315a_cau_ac_kr%2FDocuments%2FDocuments%2F2020%202%ED%95%99%EA%B8%B0%20%EA%B0%9D%EC%A7%80%ED%94%84&wd=target%28PPT.one%7C8BFD497C-C0A9-4038-86D3-841CFE660FB7%2FOOP02-2%7C5DD441D6-0057-4C2F-95B4-A494A5DAE780%2F%29))
![[Encapsulation.png]]
📌 Instance of Class
= Internal Implementation (Data + OP) + **Outer [[Interface]]**
- 객체 별로 멤버 변수를 가짐
- 클래스로 정의된 모든 객체가 메소드, (`static`으로 정의된) 클래스 메소드 및 멤버 변수를 공유
- **[[Message Passing]]** ➡️ 외부에서는 객체의 Method Call을 통해서만 작업 요청 가능 
### Visibility Modifier
#Java #CPP
📌 멤버의 가시성을 지정하는 방법, 클래스의 [[Interface]]와 구현 부분을 구분하는 방법
1. **default**: 같은 패키지 내에서 접근 허용 
2. `public` (모든 패키지 허용) ↔ **`private` (해당 클래스 내에서만 허용)**
	- Inner Class일 경우 `private`으로 정의 가능  
3. **`protected`**: `private`처럼 동작하되, 해당 클래스를 [[Inheritance (상속)]]받은 Child 클래스에서는 접근 가능
### Getter (Accessor), Setter (Mutator)
📌 **`private` 멤버 변수에 접근 제약**을 설정하기 위한 메소드 ➡ [[Consistency (일관성)]]
- Getter로는 Read 권한을, Setter로는 Write 권한을 줄 수 있음