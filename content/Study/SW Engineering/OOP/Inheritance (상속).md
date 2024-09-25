#OOP

📌 Child (Sub, Derive) = `private` 제외 Parent (Super, Base) + α
- **목적**: [[#IS-A Relationship]] **!= 단순 코드 [[Reusability (재사용성)]]** 
	- [[SOLID 원칙, 객체지향 설계 원칙#LSP (Liskov Substitution Principle, 리스코프 치환 원칙)]]

## White-box Reuse
📌 상속을 통한 재사용, Child 클래스에서 **Parent 클래스의 내부를 확인 가능**
↔ [[Composition (합성)#Black-box Reuse]]

## IS-A Relationship
📌 **A `is-a` B** = A (Child) `⊂` B (Parent) 
= Hierarchical Structure (계층 구조)
= 자식 클래스는 그들의 **부모 클래스를 의미/내용적으로 Substitution (대체)** 할 수 있어야 함 
!= 프로그램/컴파일 관점에서 부모 클래스 자리에 전달할 수 있어야 함 (이는 [[OOP]] 언어라면 당연한 것)
- B가 기대되는 곳에 A를 대신 전달해도 아무런 문제 X (Altering 없이 동작), B의 역할을 모조리 A가 다 할 수 있음 
↔ **[[Composition (합성)#HAS-A Relationship]]**
- 상속으로 구현할 수도 있지만, [[Composition (합성)]]으로 구현하는 게 더 좋음 

## Subtyping vs Implementation Inheritance

|        | Subtyping, [[Interface]] Inheritance | Implementation/Code Inheritance          |
| ------ | ------------------------------------ | ---------------------------------------- |
| **목적** | [[#IS-A Relationship]] 형성            | **의미적 관계 X, 구현의 [[Reusability (재사용성)]]** |
| **구현** | Inheritance                          | [[Composition (합성)]]부터 고려                |

## 장점
**[[Reusability (재사용성)]]** 
1. **Code Reuse**
	- [[Extensibility (확장성)]], [[Polymorphism (다형성)]]: 기존 코드의 변화 없이, 기능을 추가/재정의하여 새 클래스를 만들 수 있음 ➡ 개발 시간 단축 
	- [[Reliable (신뢰성)]]: 이미 잘 테스트된 코드 ➡ 디버깅 시간 단축 
	- [[Maintainability (유지보수성)]]: 코드의 중복 제거 
	- [[Interface]] [[Consistency (일관성)]]
2. ⭐ **Concept Reuse = IS-A 관계 구현** 
	-  **General한 개념 + α ➡ Specific한 개념** 정의
	- 공통 코드가 없더라도, 클래스를 동일한 개념으로 묶어 정의를 공유할 수 있음

## 단점
**해결 방법 = [[Composition (합성)#장점]]**
1. ⭐ **클래스 간 [[Coupling (결합도)]] 증가**
	- **Less [[Data Abstraction, Encapsulation (캡슐화), Information Hiding (정보 은닉)]]**: `protected`, Parent의 내부가 Child에게 공개됨 
	- **Bad [[Extensibility (확장성)]], [[Flexibility (유연성)]]**: Child는 Parent의 구현에 종속됨, **Parent의 변경/오류가 Child로 전파** ➡ 변경/오류 범위 커짐
	- **Bad [[Readability (가독성)]]**: Child 클래스를 이해하기 위해서는 Parent 클래스에 대해 먼저 알아야 함 
	- **클래스/조합 폭발 (Class/Combination Explosion)**: 필요 이상으로 많은 클래스를 정의하게 되는 문제  
		- [[SW Complexity (SW Crisis)]] 증가
		- **원인**: 상속 관계는 **컴파일 타임**에 결정되기 때문
2. 불필요한 기능 상속
3. [[Binding#Dynamic Binding (동적 바인딩)]] Overhead ➡ 느린 속도
4. 단일 상속만 지원하는 경우, 오히려 중복 코드가 늘어날 수 있음

## 다중 상속 문제 (Diamond Prob)
📌 #Todo

## 문법
#Java 
> "내가 자바를 만들면서 가장 후회하는 일은 상속을 만든 점이다" 
> -  제임스 고슬링(James Arthur Gosling), Java 창시자
- `class 클래스/인터페이스명 extends 클래스/인터페이스명 { };`
- **단일 상속**만 지원

#CPP 
- `class 클래스명 : 접근제한자 클래스명1, [접근제한자 클래스명2, ...] { };`
- **다중 상속** 지원