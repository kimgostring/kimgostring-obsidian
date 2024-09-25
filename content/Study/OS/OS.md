#OS

📌응용의 실행을 컨트롤하고, 응용과 컴퓨터 [[HW Component]] 사이의 [[Interface]] 역할을 하는 프로그램 
- 컴퓨터에서 가장 중요한 SW, 컴퓨터는 OS가 없으면 무용지물 

## 특징
1. Convenience (편리성)
2. Efficiency (효율성)
3. Ability to Evolve (개선 가능성)

## Micro-Kernal
📌 핵심 요소만 Kernal에 넣고, 나머지 기능은 각각 모듈화하여 외부에 분리한 것 
1. [[Memory Management]]
2. [[IPC (Inter-Process Communication)]] ➡ Information Protection & Security
3. ⭐ [[Scheduling]]
- **장점**
	1. 단순한 구현
	2. [[Flexibility (유연성)]] ➡ OS에 따라 필요한 구조를 유연하게 잡을 수 있음 
	3. [[Portability (이식성)]]

## 발전
1. (Serial Processing) 사용자가 직접 컴퓨터 시간 예약
2. (Simple Batch System) **Monitor** ~= OS, Batch = 여러 작업을 묶어 일괄 수행
3. ([[Multi-Programming]] Batch System) 
4. ([[Time-Sharing, 시분할]] 시스템) [[Processor, CPU, Core]]는 [[공유/경쟁 자원]]이므로, **독점하지 못하도록 최대 시간 제한**이 지나면 Program 제어권 넘김 
	- **목적**: 응답 시간 축소, Terminal로 컴퓨터와 소통하는 사용자 입장에서는 **독점하는 듯한 착각** [[Concurrency (동시성)]]
		- [[Context Switching]]이 많이 발생 ➡ 효율/활용은 오히려 별로
	↔ **이전까지의 목적**: 낭비되는 **[[Processor, CPU, Core]] 효율/활용 극대화**
