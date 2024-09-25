#OOP

📌 외부에서 사용하기 위해 약속된 문법 
- **Signature (선언문)**: **함수의 이름과 파라미터, 리턴값**, Interface의 문법
	- 디버깅 가능한 최소 구성 요건
- ❤️ **[[Dependency (의존성)]] Invert (반전), Break (단절)** 와 같은 관리 ➡ 적절한 [[Coupling (결합도)]] 유지

## Realization (실체화) 관계
📌 **Interface (기능, 행위)를 통해 그룹화**할 수 있는 관계

## 문법
#Java 
[[Abstract Class (추상 클래스)]]처럼 참조 변수로만 활용 가능, 보통 Interface를 사용하는 것이 더 나은 선택
1. `interface 인터페이스명 { };`  
	- `public static final` 멤버 변수, Method Signature만 포함 가능
2. `class 클래스명 implements 인터페이스명1 [, 인터페이스명2, ...] { };`
	- **다중 구현** 가능 
#Java8 
- **기본 메소드 (Default Method)**: Interface 내에서 기본 구현 정의 가능 
	- `default 함수원형/정의;`
	- **기본 메소드 충돌**: 동일한 이름의 메소드를 갖는 Interface를 다중 구현할 때, 기본 메소드가 하나 이상인 경우 
		- 이런 상황을 안 만드는 게 최선이지만, 불가피할 경우 명시적 [[Polymorphism (다형성)#Overriding (오버라이딩)]] 필요
	- ❤️ 클래스 간 **계층 구조의 단순화** ➡ Minimize [[SW Complexity (SW Crisis)]] 
#CPP 
**[[Interface]] 문법 따로 X** ➡ [[Virtual Function (가상 함수)#순수 가상 함수]]를 인터페이스로 활용해야 함 