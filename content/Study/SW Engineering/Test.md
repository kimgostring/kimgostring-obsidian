#Test
[따라하며 배우는 TDD 개발 > 단위 테스트](https://www.notion.so/sparkly-detail-801/TDD-inflearn-8106e1fb15a04c4794e664dc9b6c73f2?pvs=4#4c476dd322c84de4808e478e42797a22)

## Unit (Module) Test
📌 개발한 개별 코드 단위(모듈)를 테스트하는 것, 메소드를 테스트하는 메소드
## Integration Test
📌 단위 테스트를 통해 각 모듈이 잘 작동되는 것을 확인한 이후, 모듈을 통합하는 단계에서 수행하는 테스트
- **Slowly Integrate**: 구현되지 않은 부분은 [[Stub]]로 대체해 가며, 모듈을 하나씩 통합 
	↔️ 한 번에 합칠 경우 **디버깅이 어려움**, 어떤 모듈 간 결합 부분에서 문제가 생긴 것인지 파악하기 힘듦