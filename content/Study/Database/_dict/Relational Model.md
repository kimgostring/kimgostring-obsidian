#DataBase #RDB 

📌 모든 데이터를 2차원 Table 형태로 표현한 뒤, 각 Table 간의 Relationnship을 정의하는 Model 

## 구성 요소
Data 
= ∑ (**Table = Relation**)
= ∑ Pair of (Schema, Instance)
### Schema
= ∑ Columns, 테이블 구조, **거의 변경되지 않음**
- **Column = Attribute (속성) ~= (in File) Field**
	- [[Data Model#Domain|Domain]] 안의 값 중 하나를 가지게 됨
- [[Data Model#Data Abstraction]] ➡ 필요 속성만 표현
### Instance
=  ∑ Rows, **특정 시점**에 대한 데이터의 값 (Snapshot), **시간에 따라 변화**
 - **Row = Tuple ~= (in File) Record**: 현실 세계의 데이터 하나
 - **속성 값으로 단일 값**만 가능 ➡ [[Normalization (정규화)#제1 정규형 (1NF)]]

## Design Phase
1. Initial Phase
	- **Requirement Analysis (요건 분석)**
2. Second Phase
	- **Conceptual Modeling (개념적 설계)**: Requirement ➡ [[ER Model]]
	- 현실 세계의 복잡도로 ➡ 곧바로 [[Relational Model]] 설계하기 어려움, [[Data Model#Data Abstraction]]한 형태의 스키마를 거쳐 설계해야 함 
3. Final Phase
	- **Logical**
		1. [[ER Model]] ➡ [[Relational Model]]
			- (Identifying Relationship Set 제외) 1 Set ➡ 1 Table
				- 이렇게 할 경우 너무 많은 Table이 생겨 [[Data Model#Redundancy (중복성)]] 발생, **多 : 多를 제외한 Relation Set은 1쪽의 PK를 가리키는 FK를 多쪽에 추가**하여 표현 가능  
					- Relation이 없는 多쪽 Entity의 경우 `null` 들어가게 됨, `null`이 많으면 오히려 별개의 Table을 만드는 게 나을 수도 
			- (도출 속성 제외) Leaf Attribute ➡ 1 Attribute
				- RDB, Flatten하므로 계층 구조 표현 불가 
			- 1 다중값 속성 ➡ 1 Table
				- [[Normalization (정규화)#제 1정규형 (1NF)]]에 위배
				- PK = 원본 테이블의 PK + 다중값 속성
		2. [[Normalization (정규화)]]
		3. Refinement
		4. [[Relational Model]] 완성 
	- **Physical**: Storage에 어떤 파일 구조로 데이터를 저장하고, 접근 경로를 어떻게 구성할지 결정