#Network 

📌 Internet Protocol, **인터네트워크 ([[Network]]의 연속)** 의 연동을 위해 만들어진 프로토콜
- [[OSI (7계층) 모델#3계층 Network Layer]] 위에서 [[IP]]로 표준화, 서로 다른 [[Network]] 간 표준 프로토콜로 통신 ➡ **아래 Layer에 융통성** 줌
	- **3계층에 가깝**지만, [[OSI (7계층) 모델]]을 생각할 당시엔 이런 개념 존재하지 않았음 
	- Internet 구조 = **모래시계 형태**: 어느 계층이든 프로토콜은 여러 종류 존재하지만, **IP는 유일하며 항상 필수로 만족해야 함**
- **목표**: 모든 통신 네트워크(🔎 ATM, Eth, wifi, zigbee)에서 통신 가능 = **⭐[[데이터그램]]** (저수준, 누구나 쉽게 만족 가능)

## 특징
- **비연결성 = [[데이터그램]] 기반**
- **Best-Effort (최선 노력 전달)**: 패킷의 손실, 도착 순서, 중복, 지연(타이밍) 등 **아무것도 보장해주지 않음** ➡ **Low [[Reliable (신뢰성)]]**
- 단편화
- 재조립

## Global Addressing Scheme
- **계층 구조**: Network 주소 + Host 주소, 같은 [[Network]] 간 항상 같은 Network 부분을 가짐
	1. **A**: Network (**0** + 7) + Host (24) ➡ **xxx**.xxx.xxx.xxx
	2. **B**: Network (**10** + 14) + Host (16) ➡ **xxx.xxx**.xxx.xxx
	3. **C**: Network (**110** + 21) + Host (8) ➡ **xxx.xxx.xxx**.xxx
	- Host 주소가 **모두 1(Broadcast)** 이거나 **모두 0(Network Default/대표 주소)** 인 값은 IP 주소로 사용 불가능
		- **Subnet Zero**: Subnet 부분의 Bit가 모두 0인 Subnet ➡ Network Default 주소와 구분 어려움 
			↔ **All-Ones Subnet**: 모두 1 ➡ Broadcast와 구분 어려움 
		- **`ip subnet-zero`**: Subnetting 시, Subnet Zero도 IP 주소로 사용할 수 있도록 설정하는 명령어 
- **Dot Notation**: 1 bytes씩 `.`로 끊어 10진수로 표기
	- 맨 앞의 값을 통해 Class 확인 가능 ➡ ~127, ~191, ~223, 255

## Packet

## Router
- **구조**
	![[라우터 내부 구조.png]]
	1. **[[#Routing]] Processor**: Input Port에 [[#Forwarding Table]] 구축
	2. **Switching Fabric**: Input/Output Port가 섬유처럼 High-Seed로 연결된 회로 덩어리, 빠른 속도로 [[Forwarding]]해주는 구조
	3. **Input/Output Port**: 포트마다 각각 **Queue** 존재 ➡ [[#Packet]] Delay, Loss의 위험성
### Forwarding
📌 Input Interface로 들어온 **패킷의 최종 목적지 주소에 따라 적절한 Output Interface로** 옮기는 일
- **Longest Prefix Matching**: Forwarding Table에서 가장 
#### Forwarding Table

| IP Network Address | [[Subnet#Subnet Mask]] | Next Hop                 |
| ------------------ | ---------------------- | ------------------------ |
| `xxx.xxx.xxx.xxx`  | `xxx.xxx.xxx.xxx`      | Interface `n`            |
| ...                |                        |                          |
| otherwise          |                        | Router `xxx.xxx.xxx.xxx` |
- Host(단말)의 경우, Entry 오직 2개뿐
### Routing
📌 [[#Forwarding Table]]의 Entry를 채우는 일 


