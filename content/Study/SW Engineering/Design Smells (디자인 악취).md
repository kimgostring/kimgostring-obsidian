#Design 

[소공 > SOLID > Design Smells](onenote:https://d.docs.live.net/1bc46ecce059cbf9/Documents/2022%201학기%20소공/PPT.one#23%20SOLID&section-id={617B379E-0044-4355-A551-3465FD92561C}&page-id={F248513A-DE58-4A94-A1ED-66780D0CB8EA}&object-id={C886C2FF-CDBC-0F8F-13FD-AD3F4ED347B4}&22)  ([웹 보기](https://onedrive.live.com/view.aspx?resid=1BC46ECCE059CBF9%21263&id=documents&wd=target%28PPT.one%7C617B379E-0044-4355-A551-3465FD92561C%2F23%20SOLID%7CF248513A-DE58-4A94-A1ED-66780D0CB8EA%2F%29))
📌 나쁜 디자인을 알아볼 수 있는 다양한 신호 및 증상을 통칭하는 것, SW의 변경 및 [[Reusability (재사용성)]], [[Extensibility (확장성)]], [[Maintainability (유지보수성)]]에 악영향을 끼침
- **원인**: 잘못 관리된 [[Dependency (의존성)]] ➡ **지저분한 [[Coupling (결합도)]]**, 스파게티 코드
- **Dependency 관리 방법**
	1. [[Interface]] ➡ **[[Dependency (의존성)]] [[SOLID 원칙, 객체지향 설계 원칙#Inversion (역전)]], Break (단절)** 
	2. [[Polymorphism (다형성)]] ➡ Module이 동적으로 함수를 #Todo 

## Rigidity (경직성)
📌 시스템을 변경하기 어려움, 어떤 부분을 변경하면 다른 [[Dependency (의존성)]] Module들에까지 영향을 일으켜 **Change Propagation이 과도하게 발생**, 때문에 많은 부분을 함께 변경해야 하는 증상 
### Change Propagation
= 변화의 전파 

## Fragility (취약성, 깨지기 쉬움)
📌 **어떤 부분의 변경이 관련 없는 다른 부분들에 고장을 야기**하는 증상
↔ [[Credibility (신용성, 신뢰성)]]

## Immobility (부동성)
📌 시스템의 어떤 부분이 **다른 부분들과 [[Coupling (결합도)]]되어 있어 여러 Component로 나누기 힘든, 때문에 재사용이 어려운** 증상 
↔ [[Reusability (재사용성)]]

## Viscosity (점성, 점착성)
📌 원래 설계를 유지하며 시스템을 확장, 변경하기 어려운 증상
1. **디자인 점착성**: 시스템에 그냥 코드를 추가하는 것보다 Hack(꼼수)을 추가하는 것이 더 쉬울 때
2. **환경 점착성**: 개발환경이 느리고 비효율적일 때 (🔎 오래 걸리는 컴파일)

## Needless Complexity (불필요한 복잡성)
📌 언젠가의 유용함을 위해, 당장 필요하지 않은 구조들을 과하게 적용하여 불필요하게 복잡해진 상황

## Needless Repetition (불필요한 반복)
📌 Cut & Paste를 통해 코드를 작성했기 때문에 코드 중복이 많은 상황, 로직이 변경되면 동일한 코드를 여러 곳에서 수정해야 하는 상황

## Opacity (불투명성)
📌 작성자의 의도를 잘 표현하지 못한, 이해하기 어려운 증상