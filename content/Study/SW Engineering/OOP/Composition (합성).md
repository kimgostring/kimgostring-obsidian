#OOP #Design 

📌 중복 코드를 **객체를 참조하는 멤버 변수를 통해 주입 받아 재사용**하는 방법 
= Forwarding (포워딩)
- **목적**: [[Coupling (결합도)]]을 낮추고, [[Reusability (재사용성)]]를 높임 ➡️ Cost 

## Black-box Reuse
📌 객체의 내부가 공개되지 않고 **[[Interface]]를 통해서만 재사용하는 것**, 합성을 통한 재사용
↔ [[Inheritance (상속)#White-box Reuse]]

## HAS-A Relationship
= [[UML#HAS-A Relationship]]

## 장점
1. **[[Coupling (결합도)]] 감소**
2. [[Interface]]를 통한 재사용 ➡ [[Data Abstraction, Encapsulation (캡슐화), Information Hiding (정보 은닉)]] 유지
3. 동일 타입의 객체로 **런타임에 Dynamic하게 결정** 가능
4. 클래스 계층 구조를 소규모로 유지 ➡ Less [[SW Complexity (SW Crisis)]]