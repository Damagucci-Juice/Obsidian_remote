# 이전 아티클
[[[SwiftUI-TCA] 1. Reducer 생성]]
[[[SwiftUI-TCA] 2. SwiftUI의 View와 통합]]
[[[SwiftUI-TCA] 3. 앱과 연결]]
[[[SwiftUI-TCA] 4. Side effects 추가]]

> Effect를 이용해 네트워크 요청을 수행하는 흐름을 배움

# 들어가기전에 Effect
- Side-Effect: 원인과 결과가 항상 일정하지는 않은 행위
	- 로그인 여부에 따라서 페이지를 보여주지 않는 것
	- 네트워크 장애로 인해 외부에서 데이터를 다운로드하지 못하는 것 등
- Reducer에서 직접 수행 불가
- 리듀서에서 Action에 따라 State를 변경하고 나면 Effect를 반환해야함 

2023.12.15 작성중 ...