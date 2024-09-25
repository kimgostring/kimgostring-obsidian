#DataBase 

📌 현실 세계에 존재하는 Entity의 제약, **[[Transaction (트랜잭션)#Consistency (일관성, 정합성)]], [[#Integrity (무결성)]]을 유지하기 위한 장치** 
= [[SQL#Constraint]]
1. `컬럼 not null`
2. `unique (키)` (`null`은 허용)
3. PK, FK
4. `check (조건)`
5. `create assertion 이름 check (조건)`
	- 테이블 생성 시 제약을 추가하는 것이 아닌, 별도 구문으로 `check`보다 더 일반화된 제약 생성 가능 
	- 업데이트 시 매번 Check 필요 ➡ 조건 복잡할수록 시스템에 큰 부담

## Integrity (무결성)
📌 #Todo

## Key
1. **Composite Key (합성 키)**: 여러 개의 속성을 합친 키
2. **Super Key (슈퍼키)**: **고유 식별성**을 갖는 키
	- 고유 식별성은 특정 시점의 Instance가 아닌 **속성의 성격을 통해 판단**해야 함 
3. **Candidate Key (후보키)**: 슈퍼키 中 **고유 식별성에 필수인 최소 속성으로만 구성된** 키
	- 기본키와 달리 `null` 또한 허용
	- `unique (키)` 고유 식별성 제약 추가 가능 
4. **Primary Key (기본키)**: 후보키 中 선택된 하나
5. **Foreign Key (외래키)**: 다른 테이블의 기본키를 참조하는 키 
### Referential Integrity Constraint (참조 무결성)
📌 PK에 등장하지 않은 `null`을 제외한 값을 FK로 가질 수 없는 특성 
- 참조당하는 PK의 경우 삭제/변경에 대한 제약이, 참조하는 FK의 경우 추가에 대한 제약이 생김 
	- 기본적으로 제약을 어기는 수정은 Reject되지만, 테이블 생성 시 외래키 자리에 `null` 또는 `default`를 넣을 수 있도록 설정 가능