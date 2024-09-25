#Design #DesignPattern 

[팩토리 비교](https://refactoring.guru/ko/design-patterns/factory-comparison)
📌 무언가 (일반적으로는 객체) 를 생성하는 함수, 메소드, 클래스
- 🔎 GUI를 생성하는 함수/메소드, 사용자를 생성하는 클래스, 클래스 생성자를 호출하는 [[#정적 생성/팩토리 메소드]], [[GoF 디자인 패턴#생성 (Creational)]] 패턴

## 생성 메소드
📌 객체들을 생성하는 메소드, **생성자 호출을 둘러싼 Wrapper**  
- 생성 메소드 ⊃ [[GoF 디자인 패턴#Factory Method, Virtual Constructor]]
	- 즉, **생성 메소드 != 팩토리 메소드**
```
class Number {
    private $value;

    public function __construct($value) {
        $this->value = $value;
    }

    public function next() {
        return new Number ($this->value + 1);
    }
}
```

## 정적 생성 메소드
📌 `static`으로 선언된 [[#생성 메소드]]
= 생성 메소드가 정의된 **클래스를 통해 호출** 가능 (클래스 메소드)
- ↔ [[GoF 디자인 패턴#Factory Method, Virtual Constructor]], **[[Inheritance (상속)]]에 의존** (`static`은 상속 불가)
	- 즉, **정적 생성 메소드는 절대로 팩토리 메소드일 수 없음**
- **상황**
	1. Signature가 일치하는 여러 생성자가 필요할 때 ➡ 이름이 다른 여러 `static` 메소드를 생성하고, 메소드 내에서 생성자 호출 
	2. 새 객체를 생성하는 대신 기존 객체를 재사용하려 할 때 (🔎 [[GoF 디자인 패턴#Singleton]])

## 단순 팩토리 패턴
📌 [[GoF 디자인 패턴#Factory Method, Virtual Constructor]] 또는 [[GoF 디자인 패턴#Abstract Factory]] 패턴을 도입하기 전 중간 단계
- (주로) **단일 클래스의 단일 메소드**
	- 시간이 지날수록 메소드가 커짐 ➡ 메소드의 일부를 자식 클래스로 추출 ➡ ... ➡ [[GoF 디자인 패턴#Factory Method, Virtual Constructor]]  
```
class UserFactory {
    public static function create($type) {
        switch ($type) {
            case 'user': return new User();
            case 'customer': return new Customer();
            case 'admin': return new Admin();
            default:
                throw new Exception('Wrong user type passed.');
        }
    }
}
```

## 팩토리 메소드 패턴 vs 추상 팩토리 패턴

|        | [[GoF 디자인 패턴#Factory Method, Virtual Constructor]]                                     | [[GoF 디자인 패턴#Abstract Factory]]     |
| ------ | -------------------------------------------------------------------------------------- | ----------------------------------- |
| **정의** | 기본 객체 생성을 위한 [[Interface]]가 존재하고, **자식 클래스들이 [[Interface]]를 확장하여 생성될 객체의 유형을 변경**하는 패턴 | **관련 객체들의 Family**를 생성할 수 있도록 하는 패턴 |





