#OOP #Design

📌 Unified Modeling Language, 소프트웨어 아키텍처, 데이터베이스와 같은 복잡한 시스템을 시각화하고 구성 요소의 관계, 특징, 동작을 쉽게 이해할 수 있도록 구현하는 표준화된 방법

## 순서
1. 명사 중 일부를 Class로 뽑아내기
	- 추가적인 특성을 가지지 않거나, 관계의 Role을 명시한 것 제외
2. 남은 명사, 형용사를 Attribute로 넣어주기
3. 동사는 Operation으로 넣어주기
4. 제약 사항을 통해 Class 간 Relationship 연결

## 문법
### Class Diagram
![[Class Diagram.png]]
- **Name**: PascalCase
- **Attribute**: `[ + | - | # ] 필드명: 타입` 
	- **Access Modifier**: `+` (Public), `-` (Private), `#` (Protected)
- **OP**: `[ + | - | # ] 메소드명(파라미터1: 타입1, ...): 리턴타입`
- **Static (Class variable/OP)**: 밑줄
- **[[Abstract Class (추상 클래스)]]**
	![[Abstract Class.png]]
	- **Class**: 클래스명 위에 `<<abstract>>` Stereotype, *기울임체*, 클래스명 위/아래에 `{ abstract }` Property
	- **OP**: 리턴 타입 뒤에 `{ abstract }`, *기울임체*
- **[[Interface]]**
	📌 Public Abstract OPs의 모임에 이름을 붙인 것
	1. **Provided Interface**: 누군가에 의해 구현을 제공받는 Interface
		![[Provided Interface.png]]
		🔎 `Observer`
		- Ball (Lollipop) Symbol
		- 인터페이스명 위에 `<<interface>>` Stereotype
			- 구현을 제공하는 클래스로부터 Implements Arrow로 연결될 수 있음 (🔎 `TimerObserver`)
	2. **Required Interface**: Provided Interface에 대한 [[#Dependency]]를 갖는, 즉 정상 동작을 위해서는 Provided Interface의 구현이 요구되는 Class
		![[Required Interface.png]]
	    🔎 `Timer`
	     - Socket Symbol
	     - Provided Interface (Ball Symbol, `<<interface>>` Stereotype이 붙은 인터페이스) 에 대한 [[#Dependency]] Arrow
### Object Diagram
![[Object.png]]
- **이름에 밑줄** 있을 경우, 클래스명이 아닌 **객체명** = ⭐**특정 순간의 Snapshot**
	- 타입이 아닌 서로 다른 값을 갖게 됨
- **Name**: camelCase
	↔ [[#Class Diagram]], PascalCase
	- 클래스명을 명시하고 싶을 경우 `objectName:ClassName`
	- 객체명을 생략하고 싶을 경우 (= 무명 객체) `:ClassName`
- **Attribute**: `필드명 = 값`
	↔ [[#Class Diagram]], `[ + | - | # ] 필드명: 타입` 

## Relationship (연관성)
📌 서로 다른 두 개 이상의 객체(클래스)가 **상호 참조하는 관계**
![[Class Relationship.png]]
- ⭐ **화살표 Tail(= Source) 쪽에서 Head(= Target)의 정보** 알 수 있음
	= Source가 Target의 정보를 알고 있음
	= Target이 Source에게 정보를 줌
	- **Change Propagation (전파)**: **Target이 변하면 Source도 영향 받음** (가리켜지고 있는 게 변하면 가리키는 쪽도 함께 변해야 함) ➡ [[Data Abstraction, Encapsulation (캡슐화), Information Hiding (정보 은닉)]]으로 해결 가능 

|           |                           | Default                     |                          |                                                        |                                                                         |                                                             |     |
| --------- | ------------------------- | --------------------------- | ------------------------ | ------------------------------------------------------ | ----------------------------------------------------------------------- | ----------------------------------------------------------- | --- |
| 🔽        | **Dependency**            | **Association**             | **(Shared) Aggregation** | **[[Composition (합성)]]**                               | **[[Interface#Realization (실체화) 관계\|Realization]]<br>(Implementation)** | **[[Inheritance (상속)]]<br>(Generalization, Specification)** | 🔼  |
|           | <---                      | <――                         | 전체 ◇- 부분                 | 전체 ◆- 부분                                               | 인터페이스 ◁--- 구현                                                           | 부모 ◁―― 자식                                                   |     |
|           | **의존**                    | **연관**                      | **집합**                   | **합성, 포함**                                             | **실체화**                                                                 | **일반화, 특수화**                                                |     |
| **협업 시간** | 짧음 (일시적 [[Binding]])      | **지속**                      | 지속                       | 지속                                                     | **영원**                                                                  | 영원                                                          |     |
| **구현**    | 파라미터, 리턴 타입 (메소드 내에서만 사용) | **멤버 변수**                   | 멤버 변수                    | 멤버 변수                                                  |                                                                         |                                                             |     |
| **결정 시점** | 런타임                       | 런타임                         | 런타임                      | 런타임                                                    | **컴파일 타임**                                                              | 컴파일 타임                                                      |     |
| **관계**    | 대등, 독립                    | **IS-MEMBER-OF**,<br>대등, 독립 | **IS-PART-OF**, 독립       | **[[Composition (합성)#HAS-A Relationship\|HAS-A]]**, 의존 |                                                                         | **[[Inheritance (상속)#IS-A Relationship\|IS-A]]**,<br>의존     |     |

- [[#Association (연관)]] ⊂ Aggregation ⊂ Shared Aggregation, Composition
### Dependency (의존)
📌 한 Class의 안에서 다른 Class가 **파라미터, 리턴 타입, 메소드 내에서만 사용** 등으로 잠깐 [[Binding]]되어 쓰이는 관계
- **<---** 
	- (보통 생략) `<<use>>` Stereotype
### Association (연관)
📌 한 Class가 다른 Class에 대한 **참조를 멤버 변수**로 갖는 관계 

![[Binary Association.png]]
- **실선 < 화살표** 
	1. **Navigability**: `[ < | > | X |   ]`, 상대 Instance에 대한 참조 존재하는지 여부, Tail에서 Head의 정보 접근 가능
		- 참조를 통해 Visible한 멤버에만 접근 가능
		- **양쪽 화살표 명시하지 않은** 것도 정보 (= 정보 없음을 의도적으로 표현한 것), but 관행적으로는 **양방향 화살표**와 동일
			↔ 한쪽 화살표만 존재, 관행적으로 없는 쪽은 X로 해석 
		- 상세 설계라면 가능한 정확한 정보 주었을 것 (즉, 완성되지 않았을 확률 높음), 분석 단계에서는 Association 유무만 고려해도 됨
	2. **Association Name**: 동사, 관계가 생긴 이유(존재/행동)
		- **Reading Direction**: `[ ◀ | ▶ |   ]`, 해석 방향, 시작 방향이 주어
	3. **Multiplicity**: `[ n | n..m | n, m, ... |   ]`, 특정 Snapshot에서 반대편의 Instance 한 개에 대응되는 Instance가 동시에 최대 몇 개 연결될 수 있는지 
	4. **Role**: 명사, 해당 Class/Object가 어떠한 자격으로 해당 Relationship에 임하는지 
		= 참조 변수의 이름
		![[UML Association Relationship Role.png]]
		- **Access Modifier**: `[ + | - | # |   ]`
		- Unary Association에서는 필수 (특히, 1:多일 때)
			![[Unary Association.png]]
			- Class Diagram만으로는 정확하게 표현 불가, 주석 필요 (🔎 자신과의 결혼도 허용됨)
#### Association Class
📌 Association에 Attribute를 추가하기 위한 방법
- **多:多 관계일 때 필수**, 아닐 때는 그냥 多쪽에 Attribute 추가해주면 됨
- Association 실선에 **점선**으로 연결
- Default = Link/Relationship 중복 불가 
	= 한 조합은 하나의 Link/Relationship만 생성
	↔ **중복 허용**: `{ non-unique }` Property
		![[Association Class.png]]
#### Qualified Association
📌 多쪽의 속성 중, 多쪽의 Object를 단 하나로 한정지을 수 있는 Qualifier를 반대쪽에 명시하는 것
![[Qualified Association.png]]
- **Qualifier (한정사)**: 多쪽의 Unique한 Attribute 중 하나, 각 Instance 지칭 가능
- ❤️ 부가 정보 제공
### (Shared) Aggregation (집합)
📌 Association의 일종, [[#IS-PART-OF Relationship]]을 가지는 특수 경우 
- **전체 ◇- 부분** (Hollow Diamond)
#### IS-PART-OF Relationship
📌 **부분이 전체에 독립**적인 관계, 한 쪽의 부품 관계
= 전체가 없어지더라도, **부분이 독립적으로 존재** 가능 
- **한 부분이 `0..*`개의 전체에 속할 수 있음** 
	↔ [[#Composition (합성, 포함)]], `0..1`
- 🔎 전공(전체) ◇- 과목(부분), 수업 ◇- 학생, 컴퓨터 ◇- 키보드, 모니터, 프린터...
	- 전공이 없어지더라도 과목 남아있을 수도
	- 수업 없어진다고 학생이 막 사라지지는 않음 
	- 컴퓨터가 사라진다고 해도, 키보드나 모니터, 프린터는 다른 시스템에서도 사용 및 공유 가능 
1. **Transitive (이행적)**: `B is-part-of A` && `C is-part-of B` ⇒ `C is-part-of A`
2. **Asymmetric**: `B is-part-of A` && `A is-part-of B`는 항상 `False` 
	= 동시에 서로의 PART가 될 수는 없음 
➡ Directed Acyclic Graph, Deque
### Composition (합성, 포함)
📌 [[#(Shared) Aggregation (집합)|Aggregation]] 중, [[#HAS-A Relationship]]을 가지는 관계
= [[Composition (합성)]]
- **전체 ◆- 부분** (Solid Diamond)
#### HAS-A Relationship
= [[Composition (합성)#HAS-A Relationship|HAS-A Relationship]]
📌 **부분이 전체에 의존**하는 관계
= 부분의 수명이 전체에 의존
- ⭐ **한 부분은 무조건 `0..1`개의 전체에 속함**
	- [[#(Shared) Aggregation (집합)|Aggregation]]에서 제약조건 추가됨, **더 강한 결합**
- 🔎 건물 ◆- 강의실 ◆- 빔 프로젝터, 나무 ◆- 나뭇가지, 자동차 ◆- 타이어
	- 건물을 부수면 당연히 강의실도 사라질 것
	- 나무가 사라지면 나뭇가지는 사라짐 
	- 자동차와 타이어 간의 모델링 
		![[Shared Aggregation Example.png]]
		- 이는 Aggregation으로도 모델링 가능, **의도/강제하고 싶은 상황 살펴보고 선택** 
1. 부분이 전체의 Lifetime에 의존 ➡ 전체가 소멸하면 부분도 소멸
	↔ Aggregation, 부분은 독립적으로 존재 가능, 전체와 별개의 Lifetime 가짐 
2. **한 부분은 `0..1`개의 전체에 속해야 함** 
	↔ Aggregation, 한 부분이 여러 전체에 동시에 속할 수 있음
### Realization (실체화)
= Implementation 
-  **인터페이스 ◁--- 구현**
### Generalization (일반화)
= [[Inheritance (상속)]]
= Generalization
- **부모 ◁―― 자식**
- 다중 상속 허용
