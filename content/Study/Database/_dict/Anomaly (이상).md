#DataBase #SQLD 

📌 [[Normalization (정규화)]]을 거치지 않은 DB에서, 테이블 조작 시 발생하는 예기치 않은 현상
1. **Insert (삽입)**: 생성을 위해 존재하지 않는 데이터를 가짜로 생성해주어야 함 
2. **Update (갱신)**: [[Pitfalls (함정)#Redundancy (중복성)]] 데이터 중 일부만 수정 ➡ [[Pitfalls (함정)#Inconsistency (비일관성, 불일치)]] 발생
3. **Delete (삭제)**: 삭제되면 안 되는 데이터까지 함께 삭제됨