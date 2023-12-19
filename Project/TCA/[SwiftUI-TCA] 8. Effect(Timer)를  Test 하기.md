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
> - 외부에 의존성을 가진 테스트(Network Request)는 하기 어려우니 이번 장에선 Timer를 테스트 해보기(타이머도 Effect의 일종)


# Step1. 타이머 테스트 메서드 선언
```swift
import ComposableArchitecture
import XCTest


@MainActor
final class CounterFeatureTests: XCTestCase {
  func testTimer() async {
    let store = TestStore(initialState: CounterFeature.State()) {
      CounterFeature()
    }
  }
}
```
- 마찬가지로 TestStore를 통해 얼개를 짬

# Step2. 타이머 테스트 로직
```swift
import ComposableArchitecture
import XCTest


@MainActor
final class CounterFeatureTests: XCTestCase {
  func testTimer() async {
    let store = TestStore(initialState: CounterFeature.State()) {
      CounterFeature()
    }


    await store.send(.toggleTimerButtonTapped) {
      $0.isTimerRunning = true
    }
    // ❌ An effect returned for this action is still running.
    //    It must complete before the end of the test. …
  }
}
```
- 사용자가 스타트 타이머 버튼을 누르고 몇 초 후 스탑 타이머 버튼을 누르면 counter의 상태를 검증
- 이를 액션의 `.toggleTimerButtonTapped`를 검증하면서 확인
- 이렇게 하면 아마 문제가 생길 것임 
- 실패하는 이유: TestStore는 기능이 시간에 따라서 어떻게 변화하는지 전체를 테스트하도록 강요하기 때문
- **테스트가 끝나기 전에 모든 Effect의 결과가 검증이 되어야 함**
	- 이러는 이유는 예상치 못한 effect가 버그를 반환하는 경우
	- 예상 못한 상태나 액션이 버그를 지니고 있을 수도 있어서
# Step3. 타이머 테스트 로직 완성
```swift 
```