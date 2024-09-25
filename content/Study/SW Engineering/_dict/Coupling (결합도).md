#OOP

📌 특정 책임을 수행하는 능력이 **다른 컴포넌트의 Task**에 의존하는 정도, **다른 컴포넌트에 대해 알고 있는 지식의 정도**
= 서로 다른 모듈 간 의존성을 갖는 정도 
- [[Dependency (의존성)]] 관리의 잘못으로 Coupling이 지저분할 경우 ➡ **스파게티 코드**
- 💔 **단점**
	1. **Propagation (전파)** ➡ Bad [[Maintainability (유지보수성)]]
	2. 재사용을 위해서는 의존 관계를 끊어내는 등의 작업 필요 ➡ Bad [[Reusability (재사용성)]]
	3. 한 Module을 이해하기 위해 다른 Module들 이해 필요  ➡ Bad [[Readability (가독성)]]