#DataBase #MultiCore 
[CAP 정리란?](https://www.ibm.com/kr-ko/topics/cap-theorem)

📌 [[Distributed System (분산 시스템)]]은 Consistency, Availability, Partition 중 **2개의 요건만 충족 가능**하다는 이론

## 구성 요소
![[CAP.png]]
### Consistency (일관성)
📌 동시에 접근하는 여러 사용자가 동일한 데이터를 볼 수 있는 특성, 쓰기 작업이 모든 노드로 동시에 복제되어야 만족 가능 
↔ **궁극적/최종 일관성**: 쓰기 작업이 느슨하게 처리됨 (시간의 흐름에 따라 여러 노드에 전파)
### Availability (가용성), 내고장성
📌 모든 **가동 중단을 견뎌 내는 특성**, 하나 이상의 노드에 장애가 발생하더라도 정상 동작하는 (요청에 대해 성공적으로 응답하는) 특성 
- **가동 중단**: 사용자가 시스템을 사용할 수 없는 기간  
- **Replication (복제)**을 통해 만족 가능 (데이터 유실 대처 가능) ↔ 복제본 간 동기화가 필요하므로, [[#Consistency (일관성)]] 유지 어려워짐
### Partition Tolerance (분할 허용성)
📌 [[Distributed System (분산 시스템)]]의 네트워크 문제 상황에서도 시스템이 잘 동작하는 특성 
