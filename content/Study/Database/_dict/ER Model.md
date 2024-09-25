#DataBase #SQLD 

📌 Entity Relation Model, 현실 세계가 각각의 Entity와 그들 간의 Relation으로 구성된다고 보는 모델링 기법
- [[ERD]]를 활용하여 시각적으로 표현됨 

## 구성 요소
### (Strong) Entity Set
= ∑ [[#Entity]], 같은 Type의 Entity의 Set
- 주로 추가적 속성을 가지는 명사 모델링에 사용
- **Weak Entity Set**: **존재 자체가 다른 Strong Entity Set에 의존하는** Entity Set ➡ **Identifying Relationship**을 가져야 함 
	- 🔎 교과 과정표에 있는 수업만 개설될 수 있음
	- Weak의 PK = Strong의 PK + Weak의 Discriminator (구분자)
#### Entity
📌 응용 분야 현실 세계의 **고유 식별성**을 갖는 Object
= ∑ [[#Attribute]]
### Relationship Set
= ∑ Relationship
- **특징**
	1. 주로 **동작 모델링**에 사용 
	2. **[[#Attribute]]** 를 가질 수 있음
	3. **Role**: 연결된 Entity Set에 Lable처럼 이름을 붙일 수 있음 ➡ [[Integrity Constraint (무결성 제약)]] 명확하게 표현 가능 
	4. **Degree**: Relationship Set에 연결된 Entity Set의 개수 
	5. **Mapping Cardinality (관계차수, 참여도)**: 한 Entity에서 Relationship을 통해 연결될 수 있는 Entity의 개수 제약
		- **1 : 1** ➡ PK = 어느 한 쪽의 PK/후보키
		- **1 : 多** ➡ PK = 多쪽의 PK/후보키
		- **多 : 多** ➡ PK = 양쪽의 PK/후보키의 Union
#### Relationship
📌 [[#Entity]] 간의 관계 
### Attribute
1. **Composite (합성)**: 다른 속성들이 계층적으로 묶여 형성된 하나의 속성, 구성 요소를 분해해서 처리해야 할 요건이 존재하는 경우  
	```
	  Composite속성명
		  Component속성명1
		  Component속성명2
	```
2. **Multi-value (다중값)**: 여러 개 존재할 수 있는 속성, `{ 속성명 }`
3. **Derived (도출)**: DB에 직접 저장하지 않아도 계산될 수 있는 속성, `속성명()`

## Specialization
📌 **Top-Down 설계 방식, 상위 Entity Set의 속성과 Relationship Set을 상속**받아, [[Inheritance (상속)#IS-A Relationship]] 관계에 있는 하위 Entity Set을 설계하는 방법 
- **분류**
	1. **Overlapping**: 같은 Entity Set을 상속받는 하위 Entity Set들 간 교집합이 존재하는 경우
	2. **Disjoint**: 교집합이 존재하지 않는 경우 
		- Total (= ∑ 하위 Entity Set)
			- ERD 상에 표현 필요 
		- Patrial (!= ∑ 하위 Entity Set)
- **방법**
	1. 상위 Entity Set의 PK만 포함, 나머지 상속받은 속성은 하위 Entity Set에서 제외 ➡ [[Pitfalls (함정)#Redundancy (중복성)]]은 없지만, 매번 [[SQL#Join]] 연산이 필요 
	2. 하위 Entity Set에 중복 저장

## Generalization 
📌 Bottom-up 설계 방식, [[#Specialization]]와 반대 순서일 뿐 현실 세계의 같은 개념을 같은 방식(ERD)으로 표현 

## Aggregation (집합체)
📌 복잡한 구조의 Relationship을 하나의 추상화된 Entity Set으로 간주하는 설계 방식, 여러 Set들을 하나의 큰 개념으로 생각
- Relationship이 너무 복잡하고 [[Pitfalls (함정)#Redundancy (중복성)]]를 야기하는 경우, 이를 해결할 수 있음 