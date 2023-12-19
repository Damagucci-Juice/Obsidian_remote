# 이전 아티클
[[[SwiftUI-TCA] 1. Reducer 생성]]
[[[SwiftUI-TCA] 2. SwiftUI의 View와 통합]]
[[[SwiftUI-TCA] 3. 앱과 연결]]
[[[SwiftUI-TCA] 4. Side effects 추가]]
[[[SwiftUI-TCA] 5. 네트워크 요청 수행]]
[[[SwiftUI-TCA] 6. 타이머 관리]]

> - TCA의 장점인 "뷰와 완벽히 분리되고 테스트가 가능하다"에서 테스트를 해보기 
> - 테스트 코드 작성
> - 테스트에서 Effect 돌리기

# 0. 상태 변화를 테스트
- 오직 Reducer만 테스트하면 기능은 보장됨
- 이말은 액션이 보내졌을 때 state가 어떤식으로 변화하는지만 확인하면 된다는 말
- 

