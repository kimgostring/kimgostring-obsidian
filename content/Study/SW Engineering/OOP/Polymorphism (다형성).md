#OOP

📌 **같은 이름의 Method가 여러 의미**를 가질 수 있도록 허용하는 것
= Poly (많은) + morph (형태) + ism (특성)
= 한 개의 이름이 많은 형태를 가질 수 있는 특성 

## Overloading (오버로딩)
= **Compile time**, Static Polymorphism
📌 함수에 넘겨 준 **인자의 개수 및 타입**을 통해 실행할 함수를 유추하는 방법 
- **Ambiguous Invocation**: 컴파일러가 Most Specific Match에 실패한 경우 (두 가지 이상과 일치하는 경우)
- [[Binding#Static Binding (정적 바인딩)]] ➡ 동적 바인딩을 사용하고 싶을 경우, 
### Double Dispatch (이중 디스패치)
[GoF > 비지터 패턴과 이중 디스패치](https://refactoring.guru/ko/design-patterns/visitor-double-dispatch)
📌 Overloading된 메소드와 함께 [[Binding#Dynamic Binding (동적 바인딩)]]을 사용할 수 있는 요령
- 🔎 [[GoF 디자인 패턴#Visitor]]
### 구현
1. 이름이 같고 파라미터(타입, 개수)가 다른 함수 여러 개 정의 
2. #CPP **함수 템플릿**: 함수의 일반화된 선언 (in Generic Programming), 다른 타입의 인수를 같은 알고리즘을 통해 처리하고 싶을 때 사용 
	[TCP School > 함수 템플릿](https://www.tcpschool.com/cpp/cpp_template_function)
	- **`template <typename 임의타입명> \n 함수선언`**: 함수 내부에서 `임의타입명`을 통해 인수로 들어 온 타입을 명시할 수 있음 
	- **`template <> 함수선언`**: 템플릿이 정의된 `함수선언`에 대해, 특정 타입에 대한 특수화된 정의 선언 가능 = **명시적 특수화 (Explicit Specialization)**
3. #CPP **클래스 템플릿**
	[TCP School > 클래스 템플릿](https://www.tcpschool.com/cpp/cpp_template_class)
	- **`template <typename 임의타입명1, typename 임의타입명2> class 클래스명<임의타입명1, 명시타입명> { }`**: 템플릿 인수가 두 개 이상일 때, 일부에 대해서만 특수화 가능 = **부분 특수화 (Partial Specialization)**
	- #CPP11 **`typedef<명시타입명, 임의타입명2> 새클래스명`**: 특수화 시, 새 이름으로 선언 가능 
4. #CPP **연산자 오버로딩**: 연산자 기호 앞에 `operator`를 붙인 이름의 함수를 클래스 내부에서 `friend`로 정의한 뒤, 파라미터와 반환값이 클래스 타입이고 이름이 같은 함수를 클래스 바깥에서 정의
	- **`friend`**: 외부 함수 또는 특정 클래스 타입의 외부 객체에서, public이 아닌 멤버에 접근하도록 허용하는 키워드
		1. **`friend class 외부클래스명`**: `외부클래스명`이 클래스의 모든 멤버에 접근 가능 
		2. **`friend 외부함수선언`**: `외부함수`에서 클래스의 모든 멤버에 접근 가능

## Overriding (오버라이딩)
= **Runtime**, Dynamic Polymorphism
📌 **부모 타입의 참조 변수**를 통해 Method를 실행하면, **Runtime에 가리키고 있는 자식 타입의 내부 구현이 실행**되는 기능 
- **[[Inheritance (상속)]] 필요** 
	- ⭐ 부모 타입의 참조 변수는 **어떤 자식 타입이든 가리킬 수 있음** (= 자식은 **어떤 부모 자리에도 사용될 수 있음**)
- 맞는 Method가 [[Binding#Dynamic Binding (동적 바인딩)]] ➡ ❤️ [[Reusability (재사용성)]]
	- Method 호출되면, 호출된 객체의 클래스에서부터 Method 정의 확인 ➡ 없는 경우, 바로 위 부모 클래스에서 Method 정의 확인 ➡ ...
	- 실행 가능한 Method가 반드시 존재하게 됨 (Abstract Method가 존재할 경우, Concrete한 클래스 정의가 불가능하므로)
### 구현
1. #Java [[Inheritance (상속)]] + 재구현
2. #CPP [[Inheritance (상속)]] + [[Virtual Function (가상 함수)]] 재구현
