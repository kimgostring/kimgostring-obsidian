#DataBase #RDB #SQLD

📌 Structured Query Language (질의어), R[[DBMS]]에서 데이터를 다루기 위해 사용하는 언어 
- **종류**
	1. **[[#DML]]**
		- [[#Insert]]
		- [[#Update]]
		- [[#Delete]]
		- [[#Merge]]
		- [[#Select]]
	2. **[[#DDL]]**
		- [[#Create]]
		- [[#Alter]]
		- [[#Drop]]
		- [[#Truncate]]
	3. **[[#TCL]]**
		- [[#Commit]]
		- [[#Rollback]]
		- [[#Savepoint]]
	4. **[[#DCL]]**
		- [[#Create User]]
		- [[#Alter User]]
		- [[#Drop User]]
		- [[#Grant]]
		- [[#Revoke]]
- **[[#Advanced SQL]]**
	- [[#Subquery]]
	- [[#View]]
	- [[#Set Operation]]
	- [[#Window Function]]
	- [[#Top-N Query]]
	- [[#Hierarchy Query (계층 쿼리)]]

# DML
📌 Data **Manipulation** Language, DB **데이터를 읽거나 쓰기 위한** 문법
1. **Query Language (질의어), Declarative DML (선언적 DML)**: 유저가 **What**만 명시하고, How는 DBMS가 알아서 처리해 주는 DML
	= SQL
2. **Procedural DML (절차적 DML)**: 유저가 What + How 명시
## Insert
```SQL
INSERT INTO 테이블명 [ (컬럼명1, 컬럼명2, ...) ] VALUES (데이터1, 데이터2, ...);
```
- 명시되지 않은 컬럼은 ([[#Create]] 문에서 `DEFAULT` 값을 정의해둔 경우) 기본값이 채워지게 됨, 정의하지 않은 경우 `null`
	- PK, `NOT NULL` 등 [[Integrity Constraint (무결성 제약)]] 있는 경우 에러
- 컬럼명 `()` 리스트 생략할 경우, [[#Create]] Table 문으로 생성한 컬럼 **순서에 맞게 모든 데이터**를 나열해야 함
	- 개수 안 맞으면 그냥 에러 (`null`로 채워지지거나 뒤를 버리지 않음)

## Update
```SQL
UPDATE 테이블명 SET 컬럼명1 = 새값1, 컬럼명2 = 새값2, ... [ WHERE 조건절 ];
```
- Where절 없을 경우 모든 Row 변경 
- 조건에 맞는 Row 없어도 에러 X
- Result Table은 수정 후 Table, 즉 변경되지 않은 Row들까지 포함 

## Delete
```SQL
DELETE FROM 테이블명 [ WHERE 조건절 ];
```
- 전체 Table을 삭제하고 싶을 경우 (= Where절 없을 경우) 대신 [[#Truncate]]를 쓸 수도 있음
	- Delete를 쓰면 ❤️ 이후 [[#Commit]] 명령으로 Log를 남길 수 있어 [[Transaction (트랜잭션)#Rollback]] 가능, 💔시스템 부하됨
		↔ #SQLServer/MSSQL [[#DML]]도 Auto Commit 대상
- 조건에 맞는 Row 없어도 에러 X

## Merge
```SQL
MERGE INTO 테이블명
USING 비교테이블명
	ON 조건절
[ WHEN MATCHED THEN 
	UPDATE절
	[ DELETE절 ] ]
[ WHEN NOT MATCHED THEN
	INSERT절 ]
;
```
- `ON 조건절`에 따라 `테이블명`에 [[#Insert]], [[#Update]], [[#Delete]] 등을 한 번에 가능
	- `테이블명`과 `비교테이블명`을 Full Outer [[#Join]]하게 됨 

## Select
[[#DML]]이 아닌 별도로 DQL (Data Query Language) 으로 분류하기도 함
```SQL
SELECT [ DISTINCT | ALL ] 컬럼명1, ...
FROM 테이블명1, ...
[ WHERE 조건절 ];
```
- `컬럼명` 자리에 상수, 연산자 ([[#Operator]]), 함수 ([[#Function]]), [[#Aggregate Function (집계 함수)]] 등이 올 수 있음
- 없는 `컬럼명`이 들어가도 오류는 아님
- Default = `ALL`
	↔ **중복 제거**: `DISTINCT`
- **실행 순서**: `FROM` ➡ `WHERE` ➡ `GROUP BY` ➡ `HAVING` ➡ `SELECT` ➡ `ORDER BY`
	= `FROM`부터 쭉 실행하다, `ORDER BY` 나오기 전 `SELECT` 
### As (Alias)
```SQL
SELECT 컬럼명 [ AS ] 새이름
```
- ⭐ 어떤 위치에서든 **생략 가능**
- Result Table의 컬럼명을 `새이름`으로 변경 가능  
- **소문자로 출력하고 싶을 경우, `'소문자새이름'`처럼 문자열로** 넘겨주어야 함
	-  **문자열은 작은따옴표 (`'`) 로 감싸야** 함, **!= 큰따옴표 (`"`)**
	- 이 외의 모든 상황에서는 대문자로 출력 (🔎 `컬럼명`을 소문자로 생성했든, [[#Select]]문에 `컬럼명` 또는 `새이름`을 소문자로 적었든, ...)
### Operator
1. **숫자**: 사칙 연산자  `+`, `-`, `*`, `/`, `( )`
	- [[Pitfalls (함정)#`null`|null]]과의 연산 결과 무조건 `null`
2. **문자**: 합성 연산자 `||`
### Function
1. **숫자**
	- `ABS(수)`
	- `SIGN(수)`: 부호에 따라 `1`, `0`, `-1` 중 하나 반환
	- `ROUND(수 [, 자릿수])`: 자릿수까지 반올림, 음수일 경우 정수부 자릿수를 의미, Default = `0`
	- `TRUNC(수 [, 자릿수])`: 자릿수까지 버림, Default = `0`
	- `CEIL(수)`: 소수점 이하 올림
		↔ #SQLServer/MSSQL `CEILING`
	- `FLOOR(수)`: 소수점 이하 버림
	- `MOD(피제수, 제수)`
		- `제수`가 `0`이면 `피제수` 그대로 리턴
		- 나머지 부호는 `피제수`의 부호를 따름 
	- `POWER(m, n)`: `m^n`
	- `SQRT(수)`: 루트값 리턴
2. **문자**
	- `CHR(아스키코드)`
		↔ #SQLServer/MSSQL `CHAR`
	- `LOWER(대문자포함문자열)`: 대 ➡ 소
	- `UPPER(소문자포함문자열)`: 소 ➡ 대
	- `LTRIM(문자열 [, 특정문자집합])`: 인자가 하나뿐이면 왼쪽의 모든 공백 제거, 아니면 맨 왼쪽 문자부터 `특정문자집합`에 포함되어 있는지 확인하고 제거 (포함되어 있지 않다면 더 탐색하지 않고 그대로 종료)
		↔ #SQLServer/MSSQL 공백 제거만 가능 
	- `RTRIM(문자열 [, 특정문자열])`
	- `TRIM([위치 특정문자집합 FROM] 문자열)`: 인자가 하나뿐이면 문자열 양 옆의 공백 제거, 아니면 `위치`에서부터 `특정문자집합` 포함 여부 확인해가며 제거
		- **위치**: `LEADING`(왼쪽), `TRAILING`(오른쪽), `BOTH` (양쪽) 
	- `LPAD(문자열, n, 문자)`: `문자열` 맨 왼쪽에 `문자`를 `n`개 덧붙임 
	- `RPAD(문자열, n, 문자)`
	- `SUBSTR(문자열, 시작점 [, 길이])`: `시작점`은 1부터 시작 (**1-indexed**), 음수일 경우 맨 뒤에서부터
		↔ #SQLServer/MSSQL `SUBSTRING(문자열)`
	- `INSTR(문자열, 특정문자열 [, 시작점] [, n])`: `문자열`의 `시작점`에서부터, `n`번째로 발견된 `특정문자열`의 인덱스 반환 (1-indexed), 없으면 `0`
		- Default = `1`, `1`
		↔ #SQLServer/MSSQL `CHARINDEX`
	- `LENGTH(문자열)`
		↔ #SQLServer/MSSQL `LEN`
	- `REPLACE(문자열, 변경전문자열 [, 변경후문자열])`: `변경후문자열` 없을 경우 그냥 제거, 있을 경우 치환
	- `TRANSLATE(문자열, 변경전문자들, 변경후문자들)`: `변경전문자들`, `변경후문자들`을 같은 인덱스의 문자끼리 1:1 매핑시킨 뒤 `문자열` 내에서 치환
	- `CONCAT(문자열1, 문자열2)`
3. **날짜** 
	- `SYSDATE`: 현재의 날짜 및 시간 (🔎 `2021-09-12 22:08:08`, `nls_date_format`에 따라 형식 다름)
		↔ #SQLServer/MSSQL `GETDATE`
	- `EXTRACT(단위 FROM 날짜데이터)`: `날짜데이터`의 특정 `단위`에 대한 값만을 정수로 리턴
		- **단위**: `YEAR`, `MONTH`, `DAY`, `HOUR`, `MINUTE`, `SECOND`
		↔ #SQLServer/MSSQL `DATEPART(단위, 날짜데이터)`
	- `ADD_MONTHS(날짜데이터, 개월수)`
		- 해당 월에 같은 일자가 없으면, 해당 월의 가장 마지막 일자 리턴
		↔ #SQLServer/MSSQL `DATEADD(단위, 개월수, 날짜데이터)`
	- `LAST_DAY(날짜데이터)`: 해당 월의 마지막 일자 리턴
	- `NEXT_DAY(날짜데이터, 요일숫자)`: 해당 `날짜데이터` 이후 처음으로 돌아오는 `요일숫자`의 날짜 리턴
		- **요일숫자**: `1`(일), `2`(월), ...
	- `ROUND(날짜데이터, 단위)`: `단위`까지(이전 자리에서) 반올림 (즉, `단위`까지 표시)
		- **`단위`가 `MONTH`일 경우, `15`일까지는 같은 달의 `1`일, `16`일부터는 다음 달의 `1`일**  
	- `TRUNC(날짜데이터, 단위)`: `단위`까지 버림
4. **명시적 형변환**
	- `TO_NUMBER(문자열)`
	- `TO_CHAR(수/날짜 [, 포맷문자열])`
	- `TO_DATE(날짜문자열, 포맷문자열)`
	- `CAST(바꿀값 AS 타입)`: 바꿀값을 `타입`으로 변환 
	↔ 묵시적/암시적 형변환, [[#Create]]한 스키마와 [[#Insert]]한 데이터의 타입 다를 경우 발생 
		💔 성능 저하, 에러의 가능성
	↔ #SQLServer/MSSQL `CONVERT`, `CAST`, ...
5. **[[Pitfalls (함정)#`null`|null]]**
	- `NVL(값, 기본값)`: `값`이 `null`이 아니면 그대로 반환, `null`이면 `기본값` 대신 반환 
		- `값` 대신 `컬럼명`도 가능 (🔎 [[#Select]]절 안)
		↔ #SQLServer/MSSQL `ISNULL`
		↔ #MySQL `IFNULL`
	- `NVL2(값, null아닐때값, null일때값)`: `NVL`와 유사하되, 중간에 `null아닐때값` 끼워져 있음
	- `NULLIF(인수1, 인수2)`: `인수1 == 인수2`이면 `null` 반환, 아니면 `인수1` 반환
	- `COALESCE(인수1, 인수2, ...)`: `null`이 아닌 최초의 값 반환
		📌 합체하다, 병합하다  
### Case
```SQL
CASE 
	WHEN 조건1 THEN 값1 
	WHEN 조건2 THEN 값2
	...
	[ ELSE 기본값 ]
END [[ AS ] Alias ]

CASE 컬럼명
	WHEN 비교값1 THEN 값1 
	WHEN 비교값2 THEN 값2
	...
	[ ELSE 기본값 ]
END [[ AS ] Alias ]
```
- Else문 없을 경우 Default = `null`
- **조건이나 컬럼명 자리에 [[#As (Alias)]] 사용 불가** 
- [[#Select]]문의 컬럼명 자리를 Case 문으로 대체할 수 있음 
	```SQL
	SELECT CASE ... END [[ AS ] Alias ]
	```
- #Oracle DECODE [[#Function]]과 동일 
	```SQL
	DECODE(컬럼명, 비교값1, 값1, 비교값2, 값2, ... [, 기본값])
	```

## From
### As (Alias)
```SQL
FROM 테이블명 [ AS ] 별칭, ...
```
- 여러 테이블에 중복 컬럼명이 있어 `테이블명.컬럼명`으로 명시해야 할 때, 이를 짧게 줄여 쓸 수 있음
- 테이블의 Alias를 지정한 경우 **테이블명 대신 별칭을 사용**해야 함, 문법 에러
### Dual
```SQL
SELECT 간단한계산값 FROM DUAL; 
```
- Dummy Table, 간단한 계산값 하나를 확인하기 위해 사용
- 시스템 사용자 소유, 모든 사용자 접근 가능 

## Where
1. **비교 연산자**: `=`, `<`, `<=`, `>`, `>=`
	- **`null`과의 연산 결과 무조건 `False`** ➡ `IS NULL` 을 사용해야함
2. **부정 비교 연산자**: `!=`, `^=`, `<>`, `NOT 비교조건절`
3. **SQL 연산자**
	- `BETWEEN 값1 AND 값2`
		= `컬럼명 >= 값1 AND 컬럼명 <= 값2`
		- 범위에 `값1`, `값2` 포함
	- `LIKE 비교문자열 [ ESCAPE 이스케이프문자 ]`
		- **와일드카드**
			- **`%`**: 문자열이 있든 없든 일치
			- **`_`**: 글자수만 맞으면 일치
		- **`ESCAPE 이스케이프문자`**: `%`, `_` 앞에 `이스케이프문자`를 붙이면, 다른 의미 없이 문자 그대로 취급 
	- `IN (값1, 값2, ...)`
		= `(값1 OR 값2 OR ...)`
	- `값 비교연산자 SOME(값1, 값2, ...)`: 값들 중 하나라도 `비교연산자` 조건 만족하면 True 
		= `ANY`
		- Column이 하나인 [[#Subquery]]도 가능
	- `값 비교연산자 ALL(값1, 값2, ...)`
	- `EXIST(서브쿼리)`: `서브쿼리`에 Row 하나라도 존재하면 True
	- `UNIQUE(값1, 값2, ...)`: 중복 없으면 True 
	- **`IS NULL`**
4. **부정 SQL 연산자**
	- `NOT (조건절)/SQL연산자`
	- **`IS NOT NULL`**
5. **논리 연산자**
	- 무조건 `( )` > `NOT` > `AND` > `OR` 순

## Group By
```SQL
GROUP BY 기준컬럼명1, 기준컬럼명2, ...
[ HAVING 조건절 ]
```
- **[[#Select]] 절에 [[#Aggregate Function (집계 함수)]]가 아닌 컬럼명**을 선택하고 싶은 경우, **똑같은 컬럼명을 Group By 절에서 먼저 선택**해야 함  
### Having
- **[[#Group By]]가 없어도 Column이 1개인 경우** 단독 사용 가능 
### Group Function (그룹 함수)
📌데이터를 Group By하여 나타낼 수 있는 데이터를 구하는 함수 
#### Aggregate Function (집계 함수)
📌 여러 Row로부터 하나의 값을 집계, 도출해내는 함수 
- **`컬럼값 == null`인 Row는 제외**
- [[#Select]], [[#Having]] 절에서 사용 
- [[#Where]]절로 처리 가능한 경우, 집계 함수를 쓰면 💔성능상 불리 (필요없는 데이터까지 먼저 [[#Group By]]하게 되므로)
- **종류**
	1. `COUNT(컬럼명)`: `null`인 `컬럼명` 제외하고 계산
		- **`COUNT(*)`**: **`null` 포함** 모든 Row 
		- `COUNT(DISTINCT 컬럼명)`: **중복 제거** 후 계산
		- `COUNT(단일상수)` = `1`  
	2. `SUM(컬럼명)`
	3. `AVG(컬럼명)`
	4. `MIN(컬럼명)`
	5. `MAX(컬럼명)`
	6. `VARIANCE(컬럼명)`: 분산 = 표준편차^2
	7. `STDDEV(대상)`: 표준편차
#### Subtotal Function (소계, 총계 함수)
📌 [[#Aggregate Function (집계 함수)]]으로 도출해낸 컬럼값들에 대해, 소그룹 간 소계 및 전체 통계를 계산하는 함수
```SQL
GROUP BY 소계함수 (기준컬럼명1, 기준컬럼명2, ...)
```
- [[#Group By]] 절에서 사용
- **[[#Subquery]]와 [[#Union]] `ALL`** 을 통해 동일한 의미를 갖는 SQL문 작성 가능 
	```SQL
	SELECT 소그룹1컬럼1, ... , COUNT(*) FROM절 GROUP BY 소그룹1컬럼1, ...
	UNION ALL
	SELECT 소그룹2컬럼1, ... , COUNT(*) FROM절 GROUP BY 소그룹2컬럼1, ...
	```
	1. 소계함수의 결과로 등장하는 조합 각각을 하나의 [[#Subquery]]로 만들기
		- 각 Subquery에서 [[#Group By]] 활용
	2. 각 Subquery에 [[#Aggregate Function (집계 함수)]] `COUNT(*)` 추가 ➡ 소계 및 집계 계산
	3. 모든 Subquery를 [[#Set Operation]] `UNION ALL`로 연결
- **종류**
	1. **`ROLLUP (A, B, ... , K)`**: `A, B, ..., K`, `A, B, ..., K-1`, `A`로 그룹핑하여 소계를 계산하고, 전체 총 합계 계산 
		= ⬅ 방향으로, 소그룹에서 `기준컬럼명` 하나씩 빼가며 그룹핑 
		- 인수 순서 바뀌면 결과도 달라짐
			↔ `CUBE`, `GROUPING SETS`
		- 처음 생기는 `A, B, ..., K`의 경우, **원래 [[#Group By]] 절에 의해 생성되는 것과 동일한 Rows**
			↔ `GROUPING SETS`, 각 항목에 대한 소계만을 계산
		- **소계**: 각 소그룹 뒤에 Row 추가됨
			- `기준컬럼명`들을 `( )`로 묶어 하나의 기준으로 사용할 수 있음 (= 소그룹 생성 시, **`( )` 내의 `기준컬럼명`들을 쪼개지 않고 건너뜀**)
				- 🔎 `ROLLUP ((A, B), C)` ➡ `A, B, C`, `A, B`, 합계 
			- 소계값 및 소그룹의 기준이 되는 컬럼의 속성값들은 채워짐, 그 외 속성값은 `null` 
				- `null`이 아닌 다른 문자열을 채우고 싶을 경우, [[#Grouping]] 함수 활용 
		- **합계**: 맨 마지막에 Row 하나 더 추가됨, 합계 컬럼을 제외한 나머지 속성값은 `null`
	2. **`CUBE (A, B, ... , K)`**: 모든 조합으로 그룹핑하여 소계를 계산하고, 전체 총 합계 계산
		= `ROLLUP` + α 
		- 🔎 `CUBE (A, B, C)` ➡ `A, B, C`, `A, B`, `A, C`, `B, C`, `A`, `B`, `C`, 합계
	3. **`GROUPING SETS`**: **조합 만들지 않고, 각 항목에 대한 소계만**을 계산
		```SQL
		GROUP BY GROUPING SETS ([ 기준컬럼명 | ROLLUP | CUBE ]+ [, ( ) ])
		```
		-  **맨 마지막에 `( )`를 인자로** 넣어주거나, 한 인자에 `ROLLUP`을 씌워 총 합계 계산 가능
##### Grouping
```SQL
SELECT GROUPING(컬럼명1), GROUPING(컬럼명2), ... 
...
GROUP BY [ ROLLUP | CUBE | GROUPING SETS ] (컬럼명A, 컬럼명B, ...)
...
``` 
- [[#Select]] 절의 `컬럼명`을 Grouping 함수로 감쌀 경우, 
	1. **`GROUPING(컬럼명)`을 이름으로 하는 컬럼** Result Table에 생성 
	2. **소계/총합 Row에 `null` 대신`1`을, 그 외 Row에는** `0`을 채워넣게 됨  
- [[#Case]] 절과 함께 사용하여, `1` 대신 `치환문자열`을 넣어 소계/총합 Row를 확실히 구분함과 동시에 `컬럼명`과 `GROUPING(컬럼명)` 컬럼을 합칠 수 있음 
	```SQL
	SELECT 
		CASE GROUPING(컬럼명) 
			WHEN 1 THEN 치환문자열 ELSE 컬럼명
		END [[ AS ] 컬럼명 ],
		...
	...
	```
	- #Oracle `DECODE`
		```SQL
		SELECT DECODE(GROUPING(컬럼명), 1, 치환문자열, 컬럼명) [[ AS ] 컬럼명 ], ...
		...
		```

## Order By
```SQL
 ORDER BY 1차정렬기준컬럼(번호) [ DESC | ASC ] [ NULLS FIRST | NULLS LAST ], ...
```
- Default = `ASC`
- [[#Select]] 이후 실행 ➡ **[[#Select]]된 컬럼명 또는 번호**만 사용 가능
	- **번호**: [[#Select]]절에 기술된 순서대로 1, ...
- `null`
	- #Oracle 최댓값 취급, #SQLServer/MSSQL 최솟값 취급
	- `NULLS FIRST`, `NULLS LAST`로 순서 변경 가능 

## Join
📌 [[Normalization (정규화)]]된 Table을 연결하여, 관계 있는 데이터를 동시에 출력하기 위한 방법 
1. **(Inner) Join**
2. **Equi-Join**: Join의 조건이 `=`
	- 두 Table에 다 존재하는 컬럼명이더라도, [[#Select]] 시 `테이블명.컬럼명`처럼 제대로 명시 또는 [[#As (Alias)]] 활용 필요 
		↔ Natural Join, [[#Join ~ Using]]절
	↔ Non Equi-Join, 다른 조건으로 Join
3. **Natural Join**: 서로 같은 이름의 모든 속성 간 Equi-Join
	- Result Table에 조건이 되는 속성이 하나만 포함됨 
	- **[[#Select]] 시 `컬럼명`만** 적어야 함 (`테이블명.컬럼명`이나 [[#As (Alias)]] 불가) 
		~= [[#Join ~ Using]]절
		↔ Equi-Join
	- #Oracle [[#Join ~ Using]]절 ➡ 이름이 같은 컬럼 중 일부만 Join 조건으로 활용 가능
	- #SQLServer/MSSQL 지원 X
	↔ Equi-Join, 같은 이름의 속성으로 Join하더라도 양쪽 테이블의 속성 둘 다 포함 
4. **Outer Join**: 다른 쪽 Table에 **Join 짝이 있든 없든 무조건 Result Table에 포함**
	= Join 조건을 충족하지 않아도 결과에 포함될 수 있는 방식 
	- **목적**: **정보의 손실**을 피하기 위함
	- 원래 [[Relational Algebra (관계 대수)]]에는 없음, 편의를 위해 SQL 구문으로 제공 
	- Left, Right, Full ➡ **명시된 쪽 테이블 Tuple 中 Join 짝이 없는 것 포함**, 반대편 테이블의 속성을 모두 `null`로 채움
		- Full = Left ∪ Right (즉, 중복 제거됨)
	- #Oracle [[#Where]]절에 조건 명시할 때, **Outer Join 기준 반대쪽 (= `null`로 채워질 쪽) 컬럼명 뒤에 `(+)`** 붙이기 
		```SQL
		WHERE A.컬럼명 = B.컬럼명(+)
		```
		- 한쪽에만 `(+)` 가능 ➡ Full Outer Join 표현 불가 
		- 또는, [[#ANSI Join (표준 Join)]] 문법 쓰기
5. **Cross Join**: 조합할 수 있는 모든 조합 출력
	= Cartesian Product (카티션 곱)
	- [[#Where]]절 없이 [[#From]]에 여러 Table 명시
	- ([[#ANSI (Standard, 표준) Join]]) `CROSS JOIN`
### ANSI (Standard, 표준) Join
❤️ 벤더별 SQL 문법의 차이로 인한 호환성 문제 방지
↔ 그냥 [[#From]], [[#Where]]절 활용
```SQL
FROM 테이블명1 [ 종류 ] JOIN 테이블명2
ON 조건절
```
- 조건절을 [[#Where]] 대신 On절에 두기 
	- **차이점**: **Outer Join의 조건**이 되는 쪽 Table은 **On절과 관계 없이 항상 포함**
		1. 조건이 되는 쪽 Table의 모든 Row 추가
		2. On절의 조건에 부합할 경우, 조건이 되는 쪽 Row에 짝이 맞는 Row를 덧붙임 
		3. Join 종료, Join의 Result Table이 [[#From]]절 대체함 
		4. Where절 조건으로 필터링 
	- **Natural, Cross Join의 경우 On절 X**
- **종류**: `INNER`, `NATURAL`, [ `LEFT` | `RIGHT` | `FULL` ] `OUTER`, `CROSS`
	- Default = `INNER`
	- #SQLServer/MSSQL `NATURAL JOIN` 지원 X
#### Join ~ Using
```SQL
FROM 테이블명1 JOIN 테이블명2
USING (컬럼명1, 컬럼명2, ...)
```
- Natural Join과 유사하나, **이름이 같은 컬럼 중 일부만 Join 조건**으로 활용 가능
- Using절에 사용된 `컬럼명`의 경우, **[[#Select]]절에서 뽑을 때 `컬럼명` 그대로 써야** 함 (`테이블명.컬럼명`이나 [[#As (Alias)]] 활용 불가) 
	~= Natural Join
	↔  Equi-Join, [[#Select]] 시 `테이블명.컬럼명`처럼 제대로 명시 또는 [[#As (Alias)]] 활용
- #SQLServer/MSSQL 지원 X

### 구현
#### Loop Join
1. **Nested-Loop**: 한 Record에 대해, 다른 Table의 모든 Record와 조건 확인
	- ⏰ `(# Outer Records * # Inner Blocks) + # Outer Blocks`  
2. **Block Nested-Loop**: **한 Block 안의 Record에 대해**, 다른 Table의 모든 Record와 조건 확인
	- ⏰ `(# Outer Blocks * # Inner Blocks) + # Outer Blocks`  
3. **Indexed Nested-Loop**: Join 컬럼에 대한 [[Index (인덱스)]] 존재할 때
	- ⏰ `# Outer Blocks + (# Outer Records * 인덱스탐색시간)`  
#### Merge Join
- Equi-Join, Natural Join에만 가능
1. (정렬 안 된 경우) External Merge Sort 수행
	[데베시 > ch15.pptx](onenote:https://d.docs.live.net/1bc46ecce059cbf9/Documents/2023%201학기%20데베시/PPT.one#ch15.pptx&section-id={DE404829-01B0-44A0-983E-880787408227}&page-id={A71F4D96-C391-4AD6-9C8F-DD0F6F75A42D}&object-id={96F91BCF-9647-0950-0C0B-B88B00EA436C}&77)  ([웹 보기](https://onedrive.live.com/view.aspx?resid=1BC46ECCE059CBF9%21303&id=documents&wd=target%28PPT.one%7CDE404829-01B0-44A0-983E-880787408227%2Fch15.pptx%7CA71F4D96-C391-4AD6-9C8F-DD0F6F75A42D%2F%29))
	![[Merge Sort in Database.png]]
	1. **Initial Run 생성**: 한 Buffer Page에 한 번에 올릴 수 있는 크기를 올린 뒤 Internal Sorting을 통해 초기 Run 생성
	2. **Merge**: `k-1`개 Buffer에 각각 Run을 올린 뒤, 시작점을 포인터로 가리킨 뒤 비교해가며 Output Page에 순서대로 기록하여 `k-1`배 더 큰 크기의 Run 생성 🔃
		- 최소 3개의 Page 필요
		- [[Merge Sort#k-way]]
2. 정렬된 두 테이블의 시작점을 포인터로 가리킨 뒤 비교해가며, 조건에 맞는 경우 Output Page에 순서대로 기록  
#### Hash Join
- **전제**: 기술 발전으로 메모리 저렴해짐 ➡ 한 사용자가 많은 Page 독점 가능
- Equi-Join, Natural Join에만 가능
- 두 테이블을 동일한 Hash Fn으로 해싱하여 Partitioning한 뒤, Join 짝을 찾을 때 대응되는 Partiton만 확인  
	- In-memory Hash Index를 활용하면 성능이 더 좋음

# DDL
📌 Data **Definition** Language, **DB 스키마를 정의**하는 문법 
- [[Data Dictionary]]에 스키마를 저장하게 됨 
- 수행 전, 이전 작업 내용이 **Auto [[#Commit]]됨**
	↔ #SQLServer/MSSQL [[#DML]]도 Auto Commit
- [[#Rollback]] 불가능 
## Create
```SQL
CREATE TABLE 테이블명 (
	컬럼명1 데이터타입(크기) [ 제약조건 ] [ DEFAULT 값 ],
	... , 
	[ CONSTRAINT 컬럼명A 제약조건 ], 
	...
);
```
- `테이블명`은 고유해야 함
- 한 `테이블명` 안에서 `컬럼명`은 고유해야 함  
- `테이블명`, `컬럼명`은 숫자로 시작 불가
- 기본값 Default = `null`
### Data Type
1. **문자**
	- `CHAR`: 고정
		- 비교 연산 시, Trim한 후 비교  
	- `VARCHAR`: 가변
	- `CLOB`
	- 영어는 1 byte, 한글은 2~3 byte
2. **숫자**
	- `NUMBER`
3. **날짜**
	- `DATE`
### Constraint
= [[Integrity Constraint (무결성 제약)]]
- **정의 방법**
	1. **컬럼 제약**: 컬럼 정의 뒤에 덧붙이기
	2. **테이블 제약**: 컬럼 정의가 다 끝난 뒤 Constraint문 추가
		- 한 제약이 복수 개의 컬럼과 연관된 경우, 테이블 제약으로만 가능 
		- [[#Alter]] 문으로 변경 가능 
- **종류**
	1. **`PRIMARY KEY`**
		```SQL
		CONSTRAINT 제약명 PRIMARY KEY (컬럼명1, ...)
		```
	2. **Foreign Key `REFERNECES 참조테이블명(컬럼명)`**
		```SQL
		CONSTRAINT 제약명 FOREIGN KEY (컬럼명1, ...) REFERENCES 참조테이블명(컬럼명A, ...)
		```
		![[참조 무결성 규정 관련 옵션.png]]
	3. **`UNIQUE`**
		```SQL
		CONSTRAINT 제약명 UNIQUE(컬럼명1, ...)
		```
		- `null`은 중복 가능
	4. **`NOT NULL`**
		- 테이블 제약으로 정의 불가 
	5. **`CHECK(조건)`**: `조건절`을 통해 [[Data Model#Domain]] 명시 
		```SQL
		CONSTRAINT 제약명 CHECK(조건절)
		```
### CTAS
= Create Table As Select
```SQL
CREATE TABLE 테이블명 AS SELECT문;
```
- [[#Select]]문 [[#Subquery]]를 통해, 기존에 존재하는 Table로부터 새 Table 생성
- [[#Constraint]]는 `NOT NULL`만 복사됨 

## Alter
📌 컬럼 및 [[#Constraint]] 수정
```SQL
ALTER TABLE 테이블명 명령문;
```
- 명령 종류 중 **[[#DDL]]과 중복되는 이름의 경우 (🔎 [[#Drop]], [[#Rename]]), `이름 COLUMN`으로 시작**해야 함 
1. **Add**: 새 컬럼 추가 
	```SQL
	ALTER TABLE 테이블명 ADD 컬럼명 타입(크기);
	ALTER TABLE 테이블명 ADD (컬럼명1 타입(크기), 컬럼명2 타입(크기), ...);
	```
	- 추가할 컬럼이 하나뿐일 경우, 리스트 `( )` 있든 없든 상관 없음
2. **Drop Column**: 기존 컬럼 삭제 
	```SQL
	ALTER TABLE 테이블명 DROP COLUMN 컬럼명;
	```
	- 삭제된 컬럼 복구 불가
3. **Modify**: 기존 컬럼들 수정
	```SQL
	ALTER TABLE 테이블명 MODIFY 컬럼명 타입(크기) [ DEFAULT 값 ] [ 제약조건 ];
	ALTER TABLE 테이블명 MODIFY (컬럼명1 타입(크기) [ DEFAULT 값 ] [ 제약조건 ], ...);
	```
	- `DEFAULT`를 수정하더라도 기존 컬럼에는 변화 X
	↔ #SQLServer/MSSQL `ALTER COLUMN`
4. **Rename Column**: 컬럼명 변경
	```SQL
	ALTER TABLE 테이블명 RENAME COLUMN 기존컬럼명 TO 새이름;
	```
	↔ [[#Rename]], `테이블명` 변경
5. **Add Constraint**: [[#Constraint]] 추가
	```SQL
	ALTER TABLE 테이블명 ADD CONSTRAINT 제약명 제약조건;
	```

## Drop
```SQL
DROP TABLE 테이블명 [ 동작방법 CONSTRAINT ];
```
### Cascade
📌 종속
- 부모 Table의 데이터를 삭제하면, 자식 Table 데이터도 삭제되는 옵션
- `CASCADE CONSTRAINT`: 해당 테이블의 PK를 FK로 참조하는 다른 테이블 있을 경우, 해당 옵션을 붙이지 않고 시도할 경우 에러

## Rename
```SQL
RENAME 테이블명 TO 새이름;
```

## Truncate
```SQL
TRUNCATE TABLE 테이블명;
```
- `테이블명`의 데이터 모두 제거, 저장 공간 재사용되도록 초기화됨
	~= Where절 없는 [[#Delete]]

# TCL
📌 Transaction Control Language, [[Transaction (트랜잭션)]]을 제어하는 명령어 
## Commit
= [[Transaction (트랜잭션)#Commit]]
```SQL
COMMIT;
```
- [[#Insert]], [[#Delete]], [[#Update]] 후 변경된 내용을 확정
- 변경 후 오랜 시간 Commit 또는 Rollback하지 않으면, [[Concurrency (동시성)#Lock]]에 걸려 다른 사용자들의 접근이 어려울 수 있음 
- [[#DDL]] 실행 시 묵시적으로 Commit 실행 
- #SQLServer/MSSQL [[#DML]] 실행 시에도 묵시적 실행 

## Rollback
= [[Transaction (트랜잭션)#Rollback]]
```SQL
ROLLBACK [ TO 세이브포인트명 ];
```
- [[#Insert]], [[#Delete]], [[#Update]] 후 변경된 내용을 취소

## Savepoint
```SQL
SAVEPOINT 세이브포인트명;
```
- 추후 Rollback에 사용하면, `세이브포인트명`까지만 데이터가 복구되게 됨 

# DCL
📌 Data Control Language, User를 생성하고 데이터를 컨트롤할 수 있는 권한을 설정하는 명령어
- 하나의 DB는 여러 User 가질 수 있음, User마다 다른 Password 부여하게 됨 

## Create User
```SQL
CREATE USER 유저명 IDENTIFIED BY 비밀번호;
```
- 수행 전, [[#Grant]] 필요

## Alter User
```SQL
ALTER USER 유저명 IDENTIFIED BY 비밀번호; 
```
- 다른 User로 DB에 접근 

## Drop User
```SQL
DROP USER 유저명;
```

## Grant
```SQL
GRANT 권한1, 권한2, ... TO 유저명;
```
- **`권한`**: ([[#DDL]]) `명령어`, **([[#DML]]) `명령어 ON 테이블명`** 
	- **`ALL`**: 모든 권한 
	- 다른 유저의 `테이블명`의 경우, `다른유저명.테이블명`으로 가리킬 수 있음 
- `유저명` 대신 `역할명`도 가능

## Revoke
```SQL
REVOKE 권한 FROM 사용자명;
```
- `유저명`에게 `권한` 회수

## Role
📌 특정 권한들을 하나의 집합으로 묶어 이름 붙인 것
1. Role 생성
	```SQL
	CREATE ROLE 역할명;
	```
2. Role에 권한 부여
	```SQL
	GRANT 권한 TO 역할명;
	```
3. Role을 사용자에게 부여
	```SQL
	GRANT 역할명 TO 유저명;
	```

# Advanced SQL
## Subquery
📌 한 쿼리 안에 존재하는 또다른 쿼리
= [[#Select]] 문 내부의 [[#Select]], [[#From]], [[#Where]]/[[#Having]] 절에 포함된 또 다른 [[#Select]] 문
↔ Main Query, 맨 바깥의 쿼리 
### Scalar Subquery
- **컬럼** 대신 사용 = ⭐**하나만 [[#Select]]**
- 주로 [[#Select]] 절에 위치
### Inline View
- **테이블** 대신 사용
- [[#From]] 절에 위치
### Nested Subquery
- [[#Where]] 또는 [[#Having]] 절에 위치
- **종류**
	1. **Main Query와의 관계**
		= Subquery 내에 Main Query Table의 컬럼 포함 여부
		- 비연관 (Un-correlated)		
		- 연관 (Correlated)
			- SubQuery가 Main Query의 행 수만큼 실행, ❤️복잡한 일반 배치 프로그램 대체 가능, 💔실행 속도 느림 
	2. **반환하는 데이터 형태**
		![[중첩 서브쿼리.png]]
		- 단일 행 (Single Row): `0..1`개의 값
		- 다중 행 (Multi Row): `2..*`개의 값
		- 다중 컬럼 (Multi Column)

## View
= [[View]]
- **생성**: [[#Create]]
	```SQL
	[ CREATE | REPLACE ] VIEW 뷰이름 AS
		SELECT문
	;
	```
- **삭제**: [[#Drop]]
	```SQL
	DROP VIEW 뷰이름;
	```

## Set Operation
### Union
```SQL
테이블1 UNION [ ALL ] 테이블2 [ ORDER BY절 ];
```
- Default = 중복 제거
	- 두 테이블에 실제로 중복이 없는 경우, `ALL`을 붙이면 추가 과정 거치므로 성능상 불리 
- Result Relation의 **컬럼명은 `테이블1`** 을 따라감 
- ⚠ **주의**
	1. [[#Union 호환성]]
	2. 테이블1, 테이블2가 [[#Subquery]]일 경우, [[#Order By]]절 포함 불가 
		- ⭐ **[[#Order By]]는 맨 마지막에만** 가능 
#### Union 호환성
📌 두 테이블 간 [[#Set Operation]]이 가능하기 위한 조건 
1. 컬럼 수 일치
2. **같은 위치의 컬럼 간 [[Data Model#Domain]]** 일치
### Intersect
```SQL
테이블1 INTERSECT [ ALL ] 테이블2;
```
### Minus, Except
```SQL
테이블1 MINUS [ ALL ] 테이블2;
```
- `MINUS` = `EXCEPT`

## Window Function
📌 Row와 Row 간의 관계를 정의하기 위한 함수 
```SQL
윈도우함수([ 컬럼명 ]) OVER([ PARTITION BY 그룹기준 ] 
						[ ORDER BY 정렬기준 [ DESC ]]
						[ WINDOWING절 ]) [ AS ] 새이름
```
- **컬럼** 대신 사용 ([[#Select]] 절)
- **[[#Group By]] 문과 함께 사용 불가**
- **실행 순서**: `PARTITION` ➡ `ORDER BY` ➡ `WINDOWING` ➡ `윈도우함수`
	- ⚠ `정렬기준` != `컬럼명`일 경우, `MAX`/`MIN`이나 [[#Value Function (행 순서 함수)]]에서 결과가 의도와 다르게 나올 수 있음 ⬅ [[#Windowing]] Default = **`RANGE UNBOUNDED PRECENDING`** 
### Ranking Function (순위 함수)
```SQL
윈도우함수() OVER([ PARTITION BY 그룹기준 ] ORDER BY 정렬기준1, ... [ DESC ]) [ AS ] 새이름
```
1. **`RANK()`**: 공동 순위 있을 경우, 다음 순위를 건너뛰며 등수를 매김 (🔎 1 1 1 4 ...)
	- `ORDER BY`절의 `정렬기준`에 따라 순위 매김
	- `PARTITION BY`절 있을 경우, `그룹기준` 내에서 순위 매김 
2. **`DENSE_RANK()`**: 공동 순위가 있어도 다음 순위 건너뛰지 않음 (🔎 1 1 1 2 ...)
3. **`ROW_NUMBER()`**: 공동 순위 없음, `정렬기준`값이 같더라도 각기 다른 순위 부여 (🔎 1 2 3 4 ...)
	~= #Oracle `ROWNUM` ([[#Top-N Query]])
#### 비율 함수 
- SQLD 시험에는 해당 분류가 존재하나, 실제 대부분의 [[RDBMS]]에서는 [[#Ranking Function (순위 함수)]] 분류에 속함
```SQL
비율함수() OVER([ PARTITION BY 파티션기준 ]
				ORDER BY 정렬기준 [ DESC ]) [ AS ] 새이름
```
1. **`RATIO_TO_REPORT(컬럼명)`**: 파티션에서, `컬럼명`이 총 합계에서 차지하는 비율, `0 <= 결과 <= 1`
	= `컬럼명 / SUM(컬럼명)`
	```SQL
	RATIO_TO_REPORT(컬럼명) OVER([ PARTITION BY 파티션기준 ]) [ AS ] 새이름
	```
	- #SQLServer/MSSQL 지원 X
2. **`PERCENT_RANK()`**: 파티션의 맨 위 Row를 `0`, 맨 아래 Row를 `1`로 놓았을 때의 백분위 순위값, `0 <= 결과 <= 1`
	= `(RANK() OVER(정렬기준) - 1) / (COUNT(*) OVER() - 1)`
	- #SQLServer/MSSQL 지원 X
3.  **`CUME_DIST()`**: 파티션에서의 누적 백분율, **`0 < 결과 <= 1`**
	= `COUNT(*) OVER(정렬기준) / COUNT(*) OVER()`
	- #SQLServer/MSSQL 지원 X
4. **`NTILE(n)`**: 전체 Rows를 `n`등분했을 때, 현재 Row의 등급 계산, `1 <= 결과 <= n`
	- 딱 나누어 떨어지지 않는 경우, 앞쪽 등급(`1`)부터 하나씩 더 채워지게 됨 
### Aggregate Function (집계 함수) 
= [[#Aggregate Function (집계 함수)]]
```SQL 
집계함수(컬럼명) OVER([ PARTITION BY 그룹조건컬럼명 ]
				  [ ORDER BY 누적합정렬기준 [ DESC ] ]
				  [ WINDOWING절 ]) AS 새이름
```
- 주로 숫자 타입에 적용됨
	- **`MAX`, `MIN`, `COUNT` ➡ 문자, 날짜**에도 적용 가능 
- (`OVER` 없는 집계함수와의) **차이점**: Row 개수의 변화 X
	= `PARTITION BY`절의 `그룹기준`이 같은 Row들끼리는 서로 같은 값이 컬럼값으로 들어가게 됨
- `OVER()`의 경우, 그냥 `집계함수(컬럼명)`를 계산한 하나의 결과 상수값이 모든 Row에 들어감 
- ⚠ **`ORDER BY`절 있는 경우, [[#Windowing]]절의 Default를 잘 고려**해야 함 
1. `SUM`
	- #Oracle **`ORDER BY`절과 `RANGE UNBOUNDED PRECEDING`(= [[#Windowing]] 절)** 이 있는 경우, `누적합정렬기준` 순서대로 **누적합** 계산
		```SQL
		SUM(컬럼명) OVER(PARTITION BY 그룹조건컬럼명
		                ORDER BY 누적합정렬기준 [ DESC ]
		                RANGE UNBOUNDED PRECEDING) [ AS ] 새이름 
		```
		- `누적합정렬기준` = `컬럼명`일 경우, 
			1. `PARTITION` 구문 제거해야 함 
			2. `RANGE UNBOUNDED PRECEDING` 구문 생략 가능 
2. `MAX`
3. `MIN`
4. `AVG`
5. `COUNT`
#### Windowing 
📌 Window 절에서 **데이터의 범위를 지정**하기 위한 절 
```SQL
기준 BETWEEN 범위1 AND 범위2
```
- ⭐ **Default = `RANGE UNBOUNDED PRECENDING`** 
	= `RANGE` **`BETWEEN`** `UNBOUNDED PRECENDING` **`AND CURRERNT ROW`**
	= **첫 Row부터 현재 Row까지** 
- `기준 범위`처럼, **`BETWEEN` 없이 사용 가능한 경우 = `FOLLOWING` 제외**
	1. `기준 UNBOUNDED PRECENDING`
	2. `기준 n PRECENDING`
	3. `기준 CURRENT ROW`
- **기준**
	- `ROWS`: Row 자체의 위치 기준
	- `RANGE`: Row의 값 기준
- **범위**
	- `UNBOUNDED PRECENDING`: **맨 위의** Row  
		- **값이 크고 작은 건 모름**, `ORDER BY`의 `ASC`/`DESC`에 따라 따름 
	- `UNBOUNDED FOLLOWING`: 맨 아래의 Row
	- `CURRENT ROW`: 현재 행
	- `n PRECEDING`: 현재 행에서 **n 위의** Row
	- `n FOLLOWING`: 현재 행에서 n 아래의 Row
### Value Function (행 순서 함수)
```SQL
행순서함수(컬럼명) OVER([ PARTITION BY 그룹조건컬럼명 ]
					ORDER BY 정렬기준컬럼 [ DESC ]
					[ WINDOWING절 ]) [ AS ] 새이름
```
- #SQLServer/MSSQL 지원 X
1. **`FIRST_VALUE(컬럼명)`**: **맨 위 Row**의 컬럼값
2. **`LAST_VALUE(컬럼명)`**: 맨 아래 Row의 컬럼값
	- ⚠ **정렬 순서가 `ASC`** 인 경우 예상과 다르게 동작
		- **원인**: **[[#Windowing]]절의 Default**가 `RANGE UNBOUNDED PRECENDING` ➡ 즉, 첫 Row부터 현재 Row 중 맨 아래의 것을 선택하므로, **항상 자기 자신의 속성값 선택**하게 됨 
		- **해결**: `RANGE BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING` 명시 필요
3. **`LAG(컬럼명 [, n] [, 기본값])`**: **`n`만큼 위 Row**의 컬럼값, Default = `1`, `null`, 위쪽 Row라 값이 없는 경우 대신 `기본값` 채워지게 됨
	📌 LAG = 지연, 더디다 
4. **`LEAD(컬럼명 [, n] [, 기본값])`**: `n`만큼 아래 Row의 컬럼값

## Top-N Query
1. #Oracle **`ROWNUM`**: 맨 위 Row에서부터 순서대로 `1`, `2`, ... 번호가 매겨지는 Pseudo Column (가짜 컬럼)
	```SQL
	SELECT [ ROWNUM, ] ...
	FROM 정렬된테이블명
	[ WHERE ROWNUM < 값 | WHERE RUMNUM <= 값 ] 
	;
	```
	- **[[#From]]절에 [[#Order By]]로 정렬된 [[#Subquery]]** 를 두게 됨
		↔ ⚠ Main Query의 [[#Order By]]문에 정렬 순서를 둬 봤자 맨 마지막에 실행됨, 정렬 전 Rows에 `ROMNUM`을 붙이게 됨
	- [[#Where]]절 조건 구성에 `ROMNUM`을 포함시킬 경우, **`ROWNUM <` 또는 ` ROWNUM <=`만** 사용 가능 
		- [[#Select]]와 무관, Select절에 `ROWNUM` 포함 안 되어 있어도 됨
2. [[#Ranking Function (순위 함수)]] 활용
	```SQL
	SELECT절
	FROM (
	    SELECT 순위함수() OVER([ PARTITION BY 파티션기준 ]
				            ORDER BY 정렬기준 [ DESC ]) [ AS ] 새이름
	    ...
	)
	[ WHERE 새이름 < 값 | WHERE 새이름 <= 값 ]
	;
	```
### Top
#SQLServer/MSSQL 
```SQL
SELECT TOP(n) [ PERCENT ] [ WITH TIES ] 컬럼1, 컬럼2, ...
FROM절
...
``` 
- [[#Order By]]절 이후에 수행되므로, 정렬한 뒤 `n`개/`n`%개 만큼 출력하게 됨
- `PERCENT`: `n`개가 아닌 `n`%개의 Row 출력
- `WITH TIES`: `n`개를 넘더라도 모든 동점 출력  
### Limit
#Oracle 지원 X
```SQL
...
[ ORDER BY절 ]
LIMIT n 
[ OFFSET m ]
;
```
- [[#Order By]]절 이후에 수행
- `OFFSET`: `m` 다음 Row부터 출력, Default = `0` (1-indexed)
### Fetch First
```SQL
...
[ ORDER BY절 ]
[ OFFSET m ROWS ]
FETCH FIRST [ n ] ROW ONLY
;
```
- [[#Order By]]절 이후에 수행
- `n`, e의 Default = `1`

## Self Join
📌 나 자신과의 [[#Join]]
- 같은 테이블 여러 번 등장 ➡ **[[#Alias]] 필수**
- Depth가 깊어질 경우, [[#Hierarchy Query (계층 쿼리)]]를 쓰면 반복을 줄이고 간단하게 작성 가능 

## Hierarchy Query (계층 쿼리)
```SQL
SELECT절
FROM 테이블명
START WITH 조건절
CONNECT BY 조건절
[ ORDER SIBINIGS BY 컬럼명 ]
;
```
1. **`START WITH 조건절`**: `조건절`을 만족하는 Row를 Root 노드로 시작 
2. **`CONNECT BY`**: Root 노드에서부터, `조건절`에 만족하는 Row 없을 때까지 이어가며 자식 노드 생성 
	- **`PRIOR 컬럼명`**: 부모 노드 `컬럼명`의 속성값 반환
		- 🔎 `CONNECT BY PRIOR 부모의컬럼명 = 자식의컬럼명`
3. `SELECT` 
	- **`LEVEL`**: 현재 노드의 Depth, Root = `1`
	- **`SYS_CONNECT_BY_PATH(값, 구분자)`**: Root 노드에서부터 현재 노드까지의 경로를 `값 || 구분자 || 값 || ...` 으로 만들어 출력
		- `값`을 구성하기 위해 해당 노드의 컬럼값들 활용 가능
	- **`CONNECT_BY_ROOT 컬럼명`**: 현재 노드의 Root 노드 `컬럼명` 속성값 반환
	- **`CONNECT_BY_ISLEAF`**: Leaf 노드일 경우 `1`, 그 외 `0` 반환
4. **`ORDER SIBILING BY 컬럼명`**: Level로 1차 정렬한 뒤, `컬럼명`으로 2차 정렬
	↔ ⚠ [[#Order By]], 계층 구조와 상관없이 정렬

## Pivot
#Oracle #SQLServer/MSSQL 
[SQL활용-3(계층형 쿼리, PIVOT, 정규표현식)](https://sujee.tistory.com/48)
[예제1](https://m.blog.naver.com/isaac7263/222115359986), [예제2](https://blog.naver.com/ilifo_book/223461068051)
- [[#Group By]]절 위치에 사용
### Pivot
📌 Table 데이터를 요약하고, Row를 Column으로 바꾸어 재구성하는 명령어
= **한 컬럼의 여러 속성값(= Row) 각각이 컬럼명으로** 바뀜 
```SQL
SELECT절
FROM절
PIVOT (
	집계함수1(집계대상컬럼1) [[ AS ] 집계접두사1 ], ...
	FOR 컬럼명
	IN (컬럼값1 [[ AS ] 새컬럼명1 ], ...)
) [ AS 피봇테이블명 ]
[ ORDER BY절 ]
;
```
1. [[#From]]절
2. **`FOR 컬럼명`**: **새 컬럼명으로 사용하게 될 속성값(= Row)들의 `컬럼명` 명시** 
	- `컬럼명`이 여러 개일 경우, 리스트 `( )`가 올 수 있음 
		```SQL
		FOR (컬럼명1, 컬럼명2, ...)
		IN ((컬럼1값A, 컬럼2값A, ...) [[ AS ] 새컬럼명A ], (컬럼1값B, 컬럼2값B, ...) [[ AS ] 새컬럼명B ], ...)
		```
3. **`IN (컬럼값1 [[ AS ] 새컬럼명1 ], ...)`**: **새 컬럼명 정의**
	1. `집계접두사_새컬럼명`
	2. (`집계접두사` 없을 경우) `새컬럼명`
	3. (`집계접두사`, `새컬럼명` 없을 경우) `컬럼값`
	- 또는, [[#Select]]절의 [[#As (Alias)]]를 통해서도 변경 가능 
4. **[[#Aggregate Function (집계 함수)]]**: **새 컬럼에 저장할 속성값들(= Rows) 집계** 
5. [[#Order By]]절, [[#Select]]절
	- `피봇테이블명` 붙인 경우 해당 절들에서 사용 가능 
- **[[#Select]]절에 [[#Aggregate Function (집계 함수)|집계함수]] + [[#Case]]** 를 둔 것과 동일 
	```SQL
	SELECT 
	    집계함수(
	        CASE FOR컬럼명 
	            WHEN 컬럼값1 THEN 집계대상컬럼 
	        END) [[ AS ] 새컬럼명 ], 
	    [ 일반컬럼, ]
	    ...
	FROM절
	[ GROUP BY 일반컬럼 ]
	;
	```
### Unpivot
📌 Column을 Row로 바꾸는 명령어
= **여러 컬럼명이 새로운 컬럼의 속성값(= Row)으로** 바뀜 
```SQL
SELECT절
FROM절
UNPIVOT (
	기존속성값담을새컬럼명
	FOR 새컬럼명
	IN (기존컬럼명1 [[ AS ] 새값1 ], ...)
) [ AS 피봇테이블명 ]
[ ORDER BY절 ]
;
```
1. [[#From]]절
2. **`FOR 새컬럼명`**: **새 컬럼명 정의** 
	- 기존의 컬럼명들을 각기 다른 `새컬럼명`의 속성값으로 바꾸고 싶을 경우, 리스트 `( )`가 올 수 있음 
		```SQL
		FOR (새컬럼명1, 새컬럼명2, ...)
		```
		- **정의된 `새컬럼명` 개수 = `IN`절 각 `AS` 뒤 `( )` 안의 `새값` 개수** 
3. **`IN (기존컬럼명1 [[ AS ] 새값1 ], ...)`**: 기존 컬럼명 각각을 이용해 **새 속성값 정의**
	- 여러 `기존컬럼명`을 하나의 `새값`으로 매칭할 수 있음 
		```SQL
		IN ((기존컬럼1, 기존컬럼2, ...) AS 새값, ...)
		```
4. **`기존속성값담을새컬럼명`**: 새 속성값으로 사용된 기존 컬럼들의 속성값들을 담기 위한 새 컬럼명 정의
	- **정의된 `기존속성값담을새컬럼명` 개수 = `IN`절 각 `AS` 앞 `( )` 안의 `기존컬럼명` 개수** 
5. [[#Order By]]절, [[#Select]]절
- #Todo Cartesian Product

## Regular Expression (정규 표현식)
#Todo
[[SQL] SQL 정규표현식 가이드](https://webcodur.tistory.com/82)
[SQLD 정규표현식](https://blog.naver.com/ilifo_book/223462222925?trackingCode=rss)
1. `REGEXP_LIKE(문자열, 정규표현식 [, 옵션])`: 일치 여부 반환
2. `REGEXP_COUNT(문자열, 정규표현식 [, 시작점] [, 일치옵션])`: 일치 횟수 반환
3. `REGEXP_SUBSTR(문자열, 정규표현식 [, 시작점] [, 일치횟수] [, 서브표현식])`: 일치하는 부분 문자열 반환 
4. `REGEXP_REPLACE(문자열, 정규표현식, 변경문자열 [, 시작점] [, 일치횟수])`: 일치하는 부분을 `변경문자열`로 대체
5. `REGEXP_INSTR(문자열, 정규표현식 [, 시작점] [, 일치횟수] [, 반환옵션] [, 일치옵션] [, 서브표현식])`: 일치하는 위치의 시작 인덱스 반환 (1-indexed)
↔ #SQLServer/MSSQL 
1. `PATINDEX`
2. `LIKE` 