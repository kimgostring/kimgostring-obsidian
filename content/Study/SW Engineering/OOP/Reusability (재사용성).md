#OOP
[객체 지향의 상속 문제점과 합성 (Composition) 이해하기](https://inpa.tistory.com/entry/OOP-%F0%9F%92%A0-%EA%B0%9D%EC%B2%B4-%EC%A7%80%ED%96%A5%EC%9D%98-%EC%83%81%EC%86%8D-%EB%AC%B8%EC%A0%9C%EC%A0%90%EA%B3%BC-%ED%95%A9%EC%84%B1Composition-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0)

## 장점
1. 이미 검증된 기존 코드를 통해 새 프로그램을 만듦 ➡️ **기존 코드는 에러 없음 보장 ([[Consistency (일관성)]]), 디버깅 시간 단축**
2. 생산성 증가 ➡ **Less Money/Time Cost**
3. [[Extensibility (확장성)]]
4. 중복 코드가 적음 ➡ 수정 용이, [[Maintainability (유지보수성)]]

## 방법

|                        | [[Inheritance (상속)]]                       | [[Mixin (믹스인)]] | ⭐ [[Composition (합성)]]                  |
| ---------------------- | ------------------------------------------ | --------------- | --------------------------------------- |
| **관계**                 | [[Inheritance (상속)#IS-A Relationship]]<br> |                 | [[Composition (합성)#HAS-A Relationship]] |
| **관계 결정 시점**           | Static, 컴파일 타임에 결정                         | Static          | Dynamic, 런타임에 결정                        |
| **[[Coupling (결합도)]]** | 높음                                         |                 | ⭐ **낮음, 구현이 아닌 [[Interface]]에 의존**      |
| **주체**                 | 클래스                                        |                 | 객체                                      |
| **재사용되는 부분**           | Parent 클래스의 구현                             |                 | 포함되는 쪽의 public [[Interface]]            |
