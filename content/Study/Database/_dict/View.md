#DataBase #SQLD 

📌 전체 설계된 Logical Model 중 **자주 사용되는 필요한 컬럼**만을 사용자에게 노출하기 위한 **Virtual Relation (가상 테이블)** 
= **특정 [[SQL#Select]] 문에 이름을 붙여 재사용**이 가능하도록 저장해 둔 객체
- **View의 정의만 [[Data Dictionary#Metadata]]로 저장**됨, 실제 Tuple 따로 저장 X
	- Query 시, Base Table에 대한 구문으로 자동 변환됨 
- Base Table과 동등한 자격 

## 특징
- [[Study/Database/_dict/Security (보안성)|Security (보안성)]]: 보안이 필요한 컬럼을 제외한 별도의 View 생성 ➡ ❤️**개인 정보 보호**
	- 사용자는 일반 Table과의 차이를 구분하지 못함, **숨겨진 Table이 DB에 있다는 사실 자체를 알 수 없음**
	↔ [[Transparency (투명성)]]
- **Isolation (독립성)**: 테이블 Schema가 변경될 경우, **응용의 변경 없이 View만 수정**하면 됨
- **Convenience (편리성)**: **자주 쓰는 Result Table**을 뷰이름으로 저장 ➡ 복잡한 [[SQL#Subquery]]를 뷰이름으로 대체 가능, ❤️**가독성, 편리**

## 문법
[[SQL#View]]