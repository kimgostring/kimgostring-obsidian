#DataBase #SQLD 

📌 **현실 세계**의 데이터를 **간략화**하여 일정한 표기법에 따라 표현하기 위한 개념적인 도구의 집합 

## 특징
1. [[#Data Abstraction]] (추상화)
2. Simplification (단순화)
3. Clarity (명확화)
↔ 지양해야 할 점: [[#Pitfalls (함정)]]

## 관점 
1. Data, What
2. Process, How 
3. Data vs Process, Interaction, Relationship (데이터와 프로세스가 서로 연관성이 표현되는 상관 관점)

## Modeling 단계
1. Conceptual (개념적): [[#Data Abstraction]] Level이 가장 높음, 업무 중심적이고 포괄적, 전사적 (= 기업 전체 관점) 
	- 핵심 [[#Entity]] 추출
2. Logical (논리적): Model의 Key, 속성, 관계 등을 모두 표현, 가장 높은 재사용성
	- [[Normalization (정규화)]] 이루어지는 단계 
	- 🔎 [[#Entity]], [[#Instance]], [[#Attribute]], ...
3. Physical (물리적): 실제 DB로 구현 가능하도록 성능, 가용성 등 물리적 성격 고려, HW에 DB를 실제로 올림

## 구성 요소
1. Data
2. Data Relationships (관계)
3. Data Semantics (의미)
4. [[Integrity Constraint (무결성 제약)]]

## 종류
1. [[Relational Model]]
2. [[ER Model]], [[Super-Subtype Model, Extended ER Model]]
3. [[Data Abstraction, Encapsulation (캡슐화), Information Hiding (정보 은닉)#Object]]-based Data Model
4. Semi-structed Data Model (반구조적 데이터 모델)
	- [[NoSQL]] DBMS에서 사용 (🔎 XML, JSON)
5. Network Model (Graph), Hierarchical Model (Tree)

## 데이터 구성 요소
### Entity
📌 독립체, 응용 분야 현실 세계의 고유 식별성을 갖는 객체, **데이터를 용도별로 분류한 그룹** 
**= Table**, [[ER Model#(Strong) Entity Set]]
↔ [[ER Model#Entity]]
- **특징**
	1. 업무에 필요, 실제로 Process에서 활용
	2. 식별자를 통해 각 [[Instance]]의 고유 식별성 보장
	3. 2개 이상의 [[Instance]] 
	4. 2개 이상의 [[#Attributes]]
	5. 다른 Entity와 1개 이상의 Relationship
- **분류**
	1. **유/무형**
		- 유형 (물리적 형태 O)
		- 개념 (X)
		- 사건 (Event)
	2. **발생 시점**
		- 기본 (독립적 생성)
			- Process에 원래 존재, 자식 [[#Entity]] 가질 수 있음
		- 중심 (기본에서 파생됨, 행위를 파생)
			- **Process에서 중심적인 역할, 데이터 많이 발생**, Process 과정 중 하나 
		- 행위 (Active, 2개 이상에서 파생)
- **명명**
	1. 띄어쓰기 X 단수 명사
	2. 한글 약어 X
	3. 영문 대문자
	4. 다른 Entity와 의미상 중복 X 
### Instance
**= Row**, [[ER Model#Entity]], [[Relational Model#Instance]]
- **특징**
	1. 고유 식별성
	2. 2개 이상의 [[#Attribute]]
### Attribute
📌 [[#Instance]]의 특징을 설명해줄 수 있는 최소 단위 
**= Column**, [[ER Model#Attribute]]
- **특징**
	1. 업무에 필요, 실제로 Process에서 활용
	2. **의미상 더 쪼갤 수 없는 단위**, 최소 데이터 Level
	3. **한 [[Relational Model#Instance|Instance]]에서, 한 Attribute에 대해 하나의 속성값**만 가질 수 있음 ⬅ [[Normalization (정규화)#제 1정규형 (1NF)]]
- **분류** 
	1. **특성**
		- **기본 (Basic)**: Process 분석을 통해 바로 정의
		- **설계 (Designed)**: Process에는 없으나, 설계 과정 중 도출
			- 🔎 고유번호, 현실에는 없으나 고유 식별성을 보장하기 위해 도입 
		- **파생 (Derived)**: 자주 쓰이는 다른 Attribute의 속성값을 조합하여 미리 계산/가공 
			- ❤️ 성능, 편의성 
			- 💔 [[Transaction (트랜잭션)#Consistency (일관성, 정합성)]] 깨질 가능성 ➡ 꼭 필요할 때만 
	2. **구성 방식**
		- **PK (Primary Key)**: [[#Entity]]의 [[#Instance]]에 고유 식별성 부여
		- **FK (Foreign Key)**: 다른 [[#Entity]]의 PK 참조 (또는 `null`), 다른 Entity와 [[#Relationship]]을 맺게 해주는 매개체 
		- **일반**: PK, FK를 제외한 나머지 
	3. **분해 가능 여부**
		- **단일**: 하나의 의미로 구성
		- **복합**: 여러 개의 의미로 구성 (🔎 주소 = 시 + 구 + 동)
		- **다중값**: 한 속성이 여러 개의 값 가짐 ➡ [[Normalization (정규화)#제 1정규형 (1NF)]]
- **[[ERD]] 표기**
	1. **IE**: 맨 위가 PK 
	2. **Barker**
		- `#`: PK
		- `*`: Required
		- `○`: Optional (`null` 허용)
#### Domain
📌 [[#Attribute]]가 가질 수 있는 속성값의 범위, 특정 속성에 등장할 수 있는 **모든 가능한 속성값들의 Set** 
- **Type, Data Size**를 통해 정의
### Relationship 
📌[[#Entity]] 간 관계
= [[ER Model#Relationship]]
- **종류**
	1. 연관성
		- 존재 (🔎 소속된다)
			- [[UML#Association (연관)]], 항상 이용하는 관계, 멤버 변수 
		- 행위 (🔎 응모한다, 주문된다)
			- [[UML#Dependency (의존)]], 상대 Class의 행위에 의해 관계 형성, 행위(Operation, Method) 파라미터
		- [[ERD]]에서는 이를 구분하지 않음, [[UML]]#Class DIagram에서는 구분
	2. 식별자
		![[식별자 vs 비식별자 관계.png]]
		- **식별자** (Identification, 부모의 PK가 자식의 PK에 포함, 1:1 또는 1:多)
			- **부모-자식 관계**: 부모 [[#Entity]]가 있어야지만 자식 Entity 생김
				- ❤️ 데이터 [[Integrity Constraint (무결성 제약)#Integrity (무결성)]]
				- 💔 구조 변경 어려움, 요구사항 변경 반영 어려움
			- [[ERD]]에서 실선 (항상 연결), 부모-자식 항상 유지됨, [[SQL#Join]] 최소화 가능 
		- **비식별자** (Non-Identification, 부모의 PK가 자식의 일반 [[#Attribute]])
			- [[ERD]]에서 점선 (선택적 연결)
- **표기**
	- **관계명 (Membership)**: 각 [[#Entity]] 관점에서 하나씩, 현재형 
	- **관계차수 (Cardinality)**: 1:1, 1:多, 多:多
	- **관계선택사양, 관계선택성 (Optionality)**: 필수, 선택 
### Identifier, Key
📌 식별자, 각 [[#Instance]]를 구분 가능하게 해주는 대표 [[#Attribute]]
= [[Integrity Constraint (무결성 제약)#Key]]
- **분류**
	1. **대표성 여부**
		- 주 (Primary, 대표 식별자, 다른 [[#Entity]]와 참조 관계)
		- 보조 (Alternate, 식별성 있으나 대표 X, 참조 관계 X) 
	2. **스스로 생성 여부**
		- 내부 (Internal, 다른 [[#Entity]]에서 참조해온 것 X) 
		- 외부 (Foreign, FK, 다른 [[#Entity]]와의 연결고리) 
	3. **단일 속성 여부**
		- 단일 (Single, [[#Attribute]] 하나로 구성)
		- 복합 (Composite, 2개 이상의 [[#Attribute]]로)
	4. **대체 여부**
		- **원조, 본질** (Original, Process에 존재, 가공 X)
		- **대리, 인조** (Surrogate, 2개 이상의 [[#Attribute]]를 하나의 Attribute로 인위적 가공)
			- 💔 **단점**
				1. [[Pitfalls (함정)#Redundancy (중복성)]]
				2. 불필요한 [[Index (인덱스)]] 생성 ➡ 공간 낭비, [[SQL#DML]] 성능 저하
				3. 개발 편의성 떨어짐
#### 주식별자
⊃ PK (Primary Key, 기본키)
- **특징**
	1. **유일성**: Unique, 각 [[#Instance]]에 고유 식별성 부여 
	2. **최소성**: Candidate Key여야 함, 즉 Super Key 중 최소 개수의 [[#Attribute]]로 구성된 Key들 중 하나여야 함 
	3. **불변성**: 속성값이 최대한 변하지 않아야 함
	4. **존재성**: 속성값으로 `null` X 