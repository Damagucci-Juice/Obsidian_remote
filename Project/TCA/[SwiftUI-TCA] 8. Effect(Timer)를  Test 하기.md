#Test 
# 이전 아티클
[[[SwiftUI-TCA] 1. Reducer 생성]]
[[[SwiftUI-TCA] 2. SwiftUI의 View와 통합]]
[[[SwiftUI-TCA] 3. 앱과 연결]]
[[[SwiftUI-TCA] 4. Side effects 추가]]
[[[SwiftUI-TCA] 5. 네트워크 요청 수행]]
[[[SwiftUI-TCA] 6. 타이머 관리]]
[[[SwiftUI-TCA] 7. State를 Test 하기]]

> - 이전 장에서 살펴본 State를 테스트는 테스트 영역 중에서 가장 간단한 구조였음
> - 스토어에 의해서 처리되고 난 다음에 Effect를 검증하는 법을 배움
> - 외부에 의존성을 가진 테스트(Network Request)는 하기 어려우니 이번 장에선 Timer를 테스트 해보기(어쨌든 Effect)

