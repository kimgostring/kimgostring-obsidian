#DataBase 

📌 다양한 Evaluation Plan 대안 중, 가장 낮은 Cost의 것을 선택하는 것
- 실제 Cost는 실행 전에 알 수 없음, [[Data Dictionary#Metadata]] **통계치를 통해 추정**하는 것뿐
	- DB의 **가장 큰 성능 병목은 [[Memory#Secondary Memory]] [[I/O]]**이므로, [[Memory#Main Memory]] Time은 무시
	- Seek Time, [[I/O]]할 Block의 개수 등 고려
	- WC ➡ 항상 찾으려는 Block이 Buffer에 없고, 최소의 Buffer만 사용 가능
- [[DBMS#Query Processor (질의 처리기)]] 中 **DML Compiler**에서 수행
- [[DBMS#Declarative DML (선언적 DML)]]이므로, 효율이 다른 여러 대안이 존재할 수 있음 

## Selection
### File Scan (Linear Search)
⏰ `# All Blocks`
### [[Index (인덱스)]] Scan
1. Primary Index: ⏰ `h + 1`, ⏰ `h + # Blocks` 
2. Secondary Index: ⏰ `h + 1`, **⏰ `h + # Records`**
	- Record가 뿔뿔이 흩어져 있는 WC의 경우, **오히려 [[#File Scan (Linear Search)]]이 나을 수도**
3. Primary Index + Comparison
	- **`< 조건`: 인덱스 없이 맨 앞에서부터 보면 됨** 
	- `> 조건`: ⏰ `h + # Blocks` 
4. Secondary Index + Comparison:**⏰ `h + # Records`**

## Sorting
#Algorithm 
- **DB는 대용량 ➡ Internal Sorting 불가**
- 