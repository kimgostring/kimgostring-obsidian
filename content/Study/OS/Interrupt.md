#OS 
[운영체제 > Ch01-22S](onenote:https://d.docs.live.net/1bc46ecce059cbf9/Documents/2022%201학기%20운체/PPT.one#Ch01-22S&section-id={C8EE2FB0-AE98-4957-B90F-BF4A93D308B5}&page-id={B13A66C9-B048-4DF1-AAA9-008C0AB94F8F}&object-id={05058CDB-483F-0DBF-2F74-35B003A7E694}&78)  ([웹 보기](https://onedrive.live.com/view.aspx?resid=1BC46ECCE059CBF9%21275&id=documents&wd=target%28PPT.one%7CC8EE2FB0-AE98-4957-B90F-BF4A93D308B5%2FCh01-22S%7CB13A66C9-B048-4DF1-AAA9-008C0AB94F8F%2F%29))
📌 [[Memory]] [[I/O]]와 같이 [[Processor, CPU, Core]]보다 느린 다른 모듈의 종료를 기다려야 할 때, 
- **목적**: [[HW Component#Processor]]의 Utilization (활용) 증가

# 종류
1. **Program**: 프로그램의 잘못된 연산/참조 
2. **Timer (Overflow)**: 
[[HW Component#Processor]]의 매 Clock Pulse를 카운트하다 Overflow 발생
3. **[[I/O]]**: I/O 작업 정상/비정상 종료 
4. **HW Failure**: HW 장치 오류, Parity Error

## Multiple Interrupts
Interrupt Handler가 실행 중일 때 또 다른 Interrupt가 들어올 경우, 
1. **Disable Second Interrupt**: Interrupt Handler에서는 [[Processor, CPU, Core#Instruction Cycle]]에서 Interrupt Check를 하지 않다가, Handler의 마지막 Instruction이 끝난 뒤 Check하여 실행 
2. **Priority Scheme**: Handler [[Processor, CPU, Core#Instruction Cycle]]에서도 Interrupt Check를 하고, 인식된 경우 현재 실행 중인 것과 새로 감지된 것의 **우선 순위를 비교**하여 순서에 맞게 Interrupt 처리