#Network

[컴통 > 4장-1.pptx](onenote:https://d.docs.live.net/1bc46ecce059cbf9/Documents/2022%202학기%20컴통/PPT.one#4장-1.pptx&section-id={1F5ACE3F-8FE5-40A3-8272-CA632BFD818C}&page-id={A6EE65F7-6D21-451B-954B-EA27357679F7}&object-id={273B12AA-9CA5-030C-1873-BF12174993C5}&DF)  ([웹 보기](https://onedrive.live.com/view.aspx?resid=1BC46ECCE059CBF9%21298&id=documents&wd=target%28PPT.one%7C1F5ACE3F-8FE5-40A3-8272-CA632BFD818C%2F4%EC%9E%A5-1.pptx%7CA6EE65F7-6D21-451B-954B-EA27357679F7%2F%29))
📌 Sub [[Network]], 기존 IP 주소 계층 구조에 추가된 새로운 단계로 **내부 [[Network]]에서 사용**됨
- 💔 **기존 IP 주소의 문제점**: [[Extensibility (확장성)]] 
	1. ⭐ **32 bits ➡ 길이가 짧음**
	2. **[[IP#Global Addressing Scheme]]**: IP 주소의 Class 종류가 크기별로 3가지, 가짓수 적음 ➡ **비효율적인 주소 공간, 낭비**
	3. 점점 새 네트워크가 늘어나게 됨 
		- [[IP#Routing Table]] Entry 개수가 너무 늘어나게 됨 ➡ [[IP#Forwarding]] Delay
		- 새 네트워크를 위한 ⭐**주소 공간 부족해짐**

## Subnet Mask
📌 **[[Network]] 내부**에서 IP 주소 中 **Network 주소의 길이를 정의**하기 위한 값
- **Dot Notation** 또는 IP 주소 뒤에 **`/1개수`** 를 붙여 명시
	- Class A = `/8`, B = `/16`, C = `/24` ↔ **Subnetting**
- **IP 주소와의 Bit And** 연산 ➡ Network 주소 확인 가능 
- **Virtual Lan**: 물리적 Network는 하나지만, 그 아래에 여러 개의 Sub Network를 만드는 
	- Subnet Mask를 1 늘리면 ➡ Host 부분까지 Network 주소로 간주하게 되므로 내부 Network가 아닌 [[IP#Router]]로 나가게 됨, 마치 다른 Network인 것처럼 동작

## 방법
### CIDR
📌 Classless Inter-Domain Routing, 클래스 없는 도메인 간 라우팅 기법
- ⭐ **Host 주소 공간을 활용**하여 Network 주소를 가변적으로 늘림 
	- [[IP#Forwarding]] 시 확인하는 IP Prefix = **Network 부분을 Subnet Mask와의 Bit And 연산**으로 확인
### FLSM
📌 **Fixed** Length Subnet Mask, Host를 **같은 고정 길이로 분할**하여 각 Subnet에 할당 (예전 방법) 
### VLSM
📌 **Variable** Length Subnet Mask, **각 Subnet에서 필요한 만큼** Host를 가변 길이로 분할하여 할당 (최근 방법)
- **Host 개수가 많은 순**으로 앞쪽 번호를 주게 됨