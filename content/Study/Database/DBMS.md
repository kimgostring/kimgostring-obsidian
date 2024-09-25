#DataBase

📌 DB Management System, 연관 있는 **큰 사이즈의 데이터**들을 **관리**하고 **많은 사용자**들이 동시에 접근할 수 있도록 도와주는 SW 도구의 집합
= DB (연관된 데이터의 Set) + Management Programs (for 多  Users)
- 프로그램이 데이터 File에 직접 접근하는 것이 아닌, **프로그램과 File 사이 DBMS를 두고 [[Interface]]로 소통** 

## 목적 
⭐ **유저(응용 개발자)에게 데이터에 대한 Abstract [[View]] 제공** (using [[Data Model]]) ➡ [[Maintainability (유지보수성)]] 
1. **업데이트 [[Transaction (트랜잭션)]] 보장** ➡ [[Transaction (트랜잭션)#Consistency (일관성, 정합성)]] 유지 
2. [[Concurrency (동시성)]] ➡ 많은 사용자들의 동시 접근 허용
	- ❤️ **빠른 성능**
	- 💔 [[Transaction (트랜잭션)#Consistency (일관성, 정합성)]] 유지 어려움
3. Authorization (접근 권한)에 따라 접근 제한 ➡ [[Study/Database/_dict/Security (보안성)|Security (보안성)]]

## 종류
[SQL vs NoSQL](https://www.ibm.com/blog/sql-vs-nosql/)
[RDBMS의 한계와 NoSQL을 사용하는 이유](https://sujl95.tistory.com/81)

|             | [[RDBMS]]                                                                                    | [[NoSQL]]                                                                                                           |
| ----------- | -------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| **[[CAP]]** | **[[CAP#Consistency (일관성)]]**, [[CAP#Availability (가용성)]]                                    | **[[CAP#Partition Tolerance (분할 허용성)]] + α**                                                                        |
| **확장**      | 수직 확장                                                                                        | [[수평 확장]]                                                                                                           |
| **설계**      | [[Normalization (정규화)]], [[ER Model]]                                                        | [[De-Normalization (반정규화)]], Access Pattern 분석                                                                      |
| **속성**      | [[Transaction (트랜잭션)#ACID]]<br>➡ **신뢰성**, [[Integrity Constraint (무결성 제약)]]                  | **분산화 특성**                                                                                                          |
| **장점**      | 데이터 중복이 없음 <br>1. **[[CAP#Consistency (일관성)]]** <br>2. **[[Integrity Constraint (무결성 제약)]]** | 1. [[CAP#Availability (가용성)]] <br>2. **[[수평 확장]]성**<br>3. 데이터 중복 ➡ **[[NoSQL#BigData]]** 성능<br>4. 비정형 데이터, 가변적인 스키마 |
| **단점**      | 1. [[SQL#Join]] 성능<br>2. 수평 확장의 어려움<br>3. [[NoSQL#Big Data]] 성능                              | **데이터 중복** <br>1. [[CAP#Consistency (일관성)]] <br>2. 용량 증가                                                            |

## DB Engine
= Query Processor + Storage Manager
![[DBMS 구조 (Engine).png]]
### Query Processor (질의 처리기)
1. **DDL Interpreter**
2. **DML Compiler**
	![[Query Processing.png]]
	- [[#Query Language(질의어), Declarative DML (선언적 DML)]] 파싱, 번역 ➡ [[Relational Algebra (관계 대수)]] 표현으로 변환 ➡ Metadata를 활용하여 [[Query Optimization (질의 최적화)]] ➡ Execution Plan
3. **Query Evaluation Engine**
	- 컴파일된 DML (Execution Plan) 실행 ➡ Result Table
### Storage Manager
📌 DB에 저장된 데이터 (OS File System) ↔ 응용/쿼리 간 [[Interface]]를 제공하는 모듈 
1. ⭐ **[[Transaction (트랜잭션)#Transaction (트랜젝션)]] Management**
	- [[Failure (고장)#Recovery (복구)]] 
	- [[Concurrency (동시성)]] 제어
2.   **Buffer Manager**	
