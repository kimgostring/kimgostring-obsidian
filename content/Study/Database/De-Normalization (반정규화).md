#DataBase #SQLD 

📌 **DB 조회 성능 향상** 및 개발/운영의 단순화를 위해, 데이터 중복을 허용하거나 통합, 분리하여 **[[SQL#Join]]을 줄이는** 성능 향상 기법 
=  反정규화, [[Normalization (정규화)]]를 되돌리는 것 
↔ 비정규화, 정규화를 수행하지 않은 것 
- 모든 정규화를 마친 뒤, 조회 성능 이슈가 있을 경우에만 고려
- 💔 **단점**
	1. 입력, 수정, 삭제 성능의 저하
	2. [[Pitfalls (함정)#Inflexibility (비유연성)]]
	3. [[Transaction (트랜잭션)#Consistency (일관성, 정합성)|Consistency (일관성, 정합성)]], [[Integrity Constraint (무결성 제약)#Integrity (무결성)|Integrity (무결성)]] 문제

## Table De-Normalization 
1. **테이블 병합**: Process 상 [[SQL#Join]]이 많을 때
	- 💔 1:多의 경우, **1쪽의 일반 [[Data Model#Attribute|Attribute]] 속성값들이 중복** 저장 ➡ 1에 해당하는 Attribute 수가 많을수록 X
2. **테이블 분할**
	- 수직 ([[Data Model#Attribute|Attribute]] 분리)
		- 분리된 Entity 간 1:1 관계 성립
		- [[Transaction (트랜잭션)]] 처리 유형 파악 필요 
		- ❤️ 사용 빈도가 낮은 속성, 대부분 `null`을 속성값으로 갖는 속성
	- 수평 (속성 기준으로 [[Data Model#Instance|Instance]] 분리 = **[[Partitioning (파티셔닝)]]**, 물리적 분리)
		- 분리된 Entity 간 관계 X
3. **테이블 추가** 
	- 중복 (원격 **[[SQL#Join]] 제거**)
	- 통계 (통계치를 미리 계산, 저장)
	- 이력 (Process에 필요한 과거 속성값 저장, Master Table의 속성값 가져와 저장)
	- 부분 (한 Table의 일부 속성만을 가지는 Table 저장)

## Column De-Normalization
1. **중복 컬럼 추가**: Process 상 **[[SQL#Join]]** 이 많을 때
	- 🔎 최근 상품 가격 
2. **파생 컬럼 추가**: 부하가 예상되는 계산값을 미리 컬럼으로 계산, 저장
	- 🔎 상품 재고, 할인가 
3. **이력 테이블 컬럼 추가**: 이력 테이블의 조회 기준 컬럼을 미리 추가 
	- 🔎 최신 데이터 여부, 시작/종료일
4. PK에 의한 컬럼 추가: 복합키일 경우, [[SQL#Join]]의 단순화를 위해 단일 인공키를 PK로 추가
5. 응용 시스템 오작동을 위한 이전 데이터 보관 컬럼 추가: 과거 데이터를 임시적으로 중복 보관 

## Relationship De-Normalization
= **중복 관계 추가**
- Process 상 [[SQL#Join]]이 많을 때
- ❤️ **[[Integrity Constraint (무결성 제약)#Integrity (무결성)|Integrity (무결성)]] 문제 없음**, 이를 지키면서 처리 성능 향상 가능 