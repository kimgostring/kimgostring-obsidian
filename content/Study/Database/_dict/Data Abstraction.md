#DataBase #SQLD 

📌 유저(응용 개발자)로부터 현실 세계 데이터의 복잡함을 감추는 것 
~= [[Abstraction (추상화)#Data Abstraction]]
1. **Physical Level**: Storages 관점, File에 저장된 Record 관점
	- 🔎 자료 구조, ...
2. **Logical Level**: DB에 저장된 Tables 관점
	- ⭐ **Physical Data Independence**: 복잡한 Physical Level이 사용자로부터 감춰짐, **Logical한 변화 없이 Physical 수정 가능**
	↔ Without [[DBMS]], 응용 로직에 섞여 있으므로 Physical Level이 바뀌면 응용도 수정 필요
3. **[[View]] Level**: **Sub Schema = Tables의 Subset**, Logical Level 中 **응용에 필요 없는 부분이 감춰진** 관점 ➡ [[Study/Database/_dict/Security (보안성)|Security (보안성)]]
	- 특정 Task, 특정 사용자 위주로 [[View]] 생성
	- 가장 높은 추상화 Level

## ANSI-SPARC
📌 American National Standards Institute - Standards Planning and Requirements Committee, (1975) **DBMS의 추상적인 설계 표준**, Schema를 3단계로 나눔 
- **목적**: ⭐**데이터의 [[#Dependency (독립성)]] 보장** ⬅ 사용자의 관점과 실제 표현되는 물리적 방식의 분리 
	~= [[View]]의 존재 이유 
1. **External Schema (외부 스키마)** = [[View]] Level
	- 각/여러 사용자 관점
2. **Conceptual Schema (개념 스키마)** = Community View Level
	- 모든 사용자의 Schema를 통합한 조직 전체 관점
	- DB에 저장되는 모든 데이터 및 관계 정의 
3. **Internal Schema (내부 스키마)** = Physical Level (DB)
	- 🔎 물리적인 저장 구조, 컬럼 정의, 인덱스, ...

## Dependency (독립성)
📌 Low Abstraction Level이 변화하더라도, High Abstraction Level에 영향을 주지 않음 
1. **Logical**: Conceptual Schema의 변화 ➡ External에 영향 X
2. **Physical**: Internal Schema의 변화 ➡ Conceptual, External에 영향 X