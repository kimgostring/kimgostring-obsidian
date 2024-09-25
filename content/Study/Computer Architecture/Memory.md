#ComputerArchitecture 
[컴퓨터 구조 > lec5](onenote:https://livecauac-my.sharepoint.com/personal/kay0315a_cau_ac_kr/Documents/Documents/2020%202학기%20컴구/PPT.one#comp_arch-lec5&section-id={84B3F286-8DCA-461B-913F-CDE7D43B4260}&page-id={70A16885-3956-48AE-8258-6A72E7985B62}&object-id={72DA5D3C-F0C3-4693-A636-89C301ECA0EB}&2B)  ([웹 보기](https://livecauac-my.sharepoint.com/personal/kay0315a_cau_ac_kr/_layouts/OneNote.aspx?id=%2Fpersonal%2Fkay0315a_cau_ac_kr%2FDocuments%2FDocuments%2F2020%202%ED%95%99%EA%B8%B0%20%EC%BB%B4%EA%B5%AC&wd=target%28PPT.one%7C84B3F286-8DCA-461B-913F-CDE7D43B4260%2Fcomp_arch-lec5%7C70A16885-3956-48AE-8258-6A72E7985B62%2F%29))

## Memory Hierarchy
📌 [[#Principle of Locality (지역성의 원칙)]]를 활용하여, 최소 비용으로 최대 효율을 내기 위한 Memory 구조 
- **목적**: Big Fast Memory를 쓰고 싶으나 Cost의 한계 ➡ Hierarchy를 통해 **저렴한 Cost로 Big Fast Memory를 쓰는 것 같은 환상**을 줄 수 있음 
	- 저렴한 비용에 많은 내용 저장하면서, 속도도 충분히 빠름
- [[Processor, CPU, Core]]는 가장 상위 Level의 Memory하고만 소통, **인접한 Level에만 Copying 요청 가능** 
	- **Block**: Copying의 데이터 단위 (보통 n `word`)
	- **Hit**: 데이터가 해당 Level에 있는 경우
		- Hit Ratio = `# Hits / # Accesses`
	- **Miss**: 없는 경우
		1. 인접 하위 Level에 데이터 요청
		2. 받아 온 데이터를 현재 Level에 Write
		3. [[Processor, CPU, Core]]가 [[#Cache]]에 접근하여 다시 작업 시도 
		- **Block Size가 클 경우,**
			1. [[#Principle of Locality (지역성의 원칙)]]로 인해 Miss Ratio ⬇
			2. 공간 수가 적어 Swap이 자주 발생하고, 적은 부분의 데이터를 위해 큰 Block을 복사해야 하므로 Penalty ⬆ 
		- **Miss Penalty, Memory Stall Time**: Miss로 인해, Lower Level에서부터 Block을 찾아 위로 올리기까지 걸리는 시간 
		- SW의 Memory Access Pattern에 따라서도 Miss 횟수가 다를 수 있음, 성능을 위해서는 개발자가 이를 고려해야 함
	- **Memory Stall (정지)**: Memory로부터 데이터를 가져오는 동안 [[Processor, CPU, Core]]가 기다리는 것 
### Principle of Locality (지역성의 원칙)
📌 Program은 특정 순간에 전체 메모리 중 오직 Clustered된 일부분에만 접근한다는 원칙
1. **시간 지역성 (Temporal Locality)**: **최근**에 접근한 메모리는 또 사용하게 될 확률 높음 
	- 🔎 Loop
2. **공간 지역성 (Spatial Locality)**: 최근에 접근한 메모리 **근처**의 메모리에 접근하게 될 확률 높음 
	- 🔎 Sequential Instruction, Array
### Cache
📌 가장 [[Processor, CPU, Core]]와 가까운 Top Level에 위치한 Memory, [[Processor, CPU, Core]] Chip에 내장
= SRAM (Static RAM)
- **RAM**: Random Access Memory
	- Random ➡ Block 저장 **위치에 상관 없이 Fixed Access Time**
		1. **Direct-mapped**: [[Hash]] 활용
			- Hash Fn = `원본Block주소 % Block개수`
				- 공간이 다 비어 있어도 해시값 위치가 차 있으면 Swap 필요 ➡ **공간 낭비**
			- 해시값은 원본 Block 주소의 하위 Bits이므로, 상위 Bits만 Tag로 저장해 둔다면 `Tag값|Hash값`을 통해 원본 Block의 주소 확인 가능 
			- 해시값 위치에 값이 존재하고, Tag값이 원본 Block 주소의 하위 Bits와 일치할 경우 Hit
		2. **Associative**
			- Logic Circuit (Cost) ➡ 공간 낭비 개선
			2. **Fully**: 아무 공간에나 다 넣을 수 있음 
			3. **M-way Set**: 해시값으로 Set을 찾은 뒤, Set 안의 M개 공간 중 아무 곳에나 넣을 수 있음  
		↔ Magnetic Disk/Tape
	- **Volatile (휘발성)**
- **[[#Main Memory]]와 항상 값이 일치**해야 함 ➡ [[Consistency (일관성)]]
	1. **Write-Through (동시 저장)**: Write 시마다, 항상 Cache와 Main Memory 동시에 수정
		- 시간이 오래 걸린다는 단점 ➡ Write Buffer에 적어둔 뒤 Main Memory Write 요청
		- **Write Around**: Write Miss일 때, 어차피 Cache와 Memory 둘 다에 값을 새로 쓸 것이므로 Fetch하지 않는 방식 
			- Write-Back에서는 불가 ([[Consistency (일관성)]])
	2. ⭐ **Write-Back**: Cache만 업데이트하고, **Swap될 때 Main Memory 수정** 
		- Cache 업데이트 시 `dirty = 1`처럼 표시, 이후 Swap되어 Cache에서 버려질 때 Main Memory 수정 
		- 보통 더 **빠름** 
### Main Memory
= DRAM (Dynamic RAM), Real/Primary Memory
- **DDR DRAM**: Double Data Rate, 1 Clock동안 Up/Down 순간을 활용하여 총 두 번 작업이 가능한 DRAM 
### Secondary Memory
1. **SSD (Flash Storage)**
	- 🔎 USB, CD, DVD, Blu-Ray
	- **Non-volatile**
	- 전기를 활용하기 때문에 HDD에 비해 속도가 빠르지만, **사용할수록 점점 저장 능력 상실** ➡ Disk 대체품으로 사용하기에는 어려움 
2. **HDD (Magnetic Disk)**
	- **Non-volatile**
	- Mechanical Device, **기계적 동작** ➡ 전기에 비해 속도 훨씬 느림, But [[#Principle of Locality (지역성의 원칙)]], [[OS]] [[Scheduling]]이 이를 보완할 수 있음, 비싼 경우 [[#Cache]]를 내장하고 있기도 함 
		![[HDD.png]]
		- **Seek Time**: Arm을 움직여 Track 찾는 시간
		- **Rotational Latency (회전 지연)**: Disk Platter가 회전하여 알맞은 Sector 찾는 시간  
### Magnetic Tape
= Third Memory, Backup Memory