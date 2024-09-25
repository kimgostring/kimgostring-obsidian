#DataBase 

Modeling 결과는 유일하지 않으나, 나쁜 설계는 피해야 함

## Redundancy (중복성)
📌 동일한 데이터가 중복 저장되는 특성
- 💔 [[Transaction (트랜잭션)#Consistency (일관성, 정합성)]]을 유지하며 수정하기 어려움 ([[#Inconsistency (비일관성, 불일치)]] 문제 발생할 수 있음)

## Incompleteness (불완전성)
📌 현실 세계에는 존재하는 데이터를 설계된 스키마에 표현할 방법이 없는 특성
- 🔎 Insertion Anomaly, Deletion Anomaly, ...

## Inconsistency (비일관성, 불일치)
↔ [[Transaction (트랜잭션)#Consistency (일관성, 정합성)]]
- 🔎 **데이터 중복**, 연관성 고려 X 한쪽만 수정하는 경우

## Inflexibility (비유연성)
📌응용의 사소한 변화에도 데이터 모델이 수시로 변경되어야 하는 특성 
- 🔎 응용과 데이터 간 높은 의존도/연계성
- 💔 유지보수 어려움 ⬅ 응용의 변화가 데이터로 자주 전파  

## `null`
📌 데이터/값이 없음
- 과도하게 사용하면 좋지 않음, 혼란 초래
	- 가로 연산 (= 한 [[#Instance]] 내): 무조건 `null`
	- 세로 연산 (= 여러 Instance 간, **집계 함수**): **`null`인 [[Data Model#Instance|Instance]] 제외** 후 집계