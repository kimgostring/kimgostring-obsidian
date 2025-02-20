#DataBase #SQLD 

📌 데이터를 조작하기 위한 **하나의 논리적 기능을 수행**하는 여러 물리적 연산의 집합, 하나의 연속적인 업무 단위 
= 더는 **쪼갤 수 없는 작업 단위** 

## 필수적 관계
- 두 [[Data Model#Entity|Entity]] 간의 관계가 서로 필수적일 때 하나의 트랜잭션 형성
↔ 선택적 관계, 서로 독립적 수행 가능 

## ACID
📌 **[[RDBMS]]의 특징**, 트랜잭션이 안전하게 수행된다는 것을 보장하기 위한 네 가지 속성
- 일부 [[NoSQL]]도 이를 만족할 수 있음 
### Atomicity (원자성)
📌 더 이상 나눌 수 없는 특성, 모든 트랜잭션은 전부 성공하거나 전부 실패해야 함 
= **All or Nothing**
- **방해 요소**
	- [[Failure (고장)]]
### Consistency (일관성, 정합성)
📌 트랜잭션이 완료된 전후로 데이터가 일관적이어야 한다는 특성, 여전히 [[Integrity Constraint (무결성 제약)]]을 만족해야 함 
### Isolation (고립성, 독립성)
📌 각 트랜잭션은 서로에게 영향을 미칠 수 없는 특성
### Durability (지속성, 영속성)
📌 한 번 끝난 트랜잭션의 결과가 영구히 저장되어야 한다는 특성
1. 성공한 트랜잭션은 Log에 남겨진 뒤 [[#Commit]]되어야 함
2. 시스템 [[Failure (고장)]]에도 불구하고 [[#Rollback]] 가능해야 함 

## Commit

## Rollback
