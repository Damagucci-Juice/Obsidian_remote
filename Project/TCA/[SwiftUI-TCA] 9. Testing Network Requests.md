#Test 
**목차** 
- [[#이전 아티클|이전 아티클]]
- [[#Step0. 들어가기 전에|Step0. 들어가기 전에]]
- [[#Step1. 테스트 메서드 선언|Step1. 테스트 메서드 선언]]
- [[#Step2.|Step2.]]

# 이전 아티클
[[[SwiftUI-TCA] 1. Reducer 생성]]
[[[SwiftUI-TCA] 2. SwiftUI의 View와 통합]]
[[[SwiftUI-TCA] 3. 앱과 연결]]
[[[SwiftUI-TCA] 4. Side effects 추가]]
[[[SwiftUI-TCA] 5. 네트워크 요청 수행]]
[[[SwiftUI-TCA] 6. 타이머 관리]]
[[[SwiftUI-TCA] 7. State를 Test 하기]]
[[[SwiftUI-TCA] 8. Effect(Timer)를  Test 하기]]



> 외부 시스템에 의존하는 Network Request를 테스트하는 방법을 배우기

# Step0. 들어가기 전에
- 네트워크 요청도 Side Effect의 일종
	- 서버 상황이나 네트워크 연결 상태에 따라 결과값이 달라지기 때문 
- NumbersAPI를 단순한 방식으로 테스트해보고 무엇이 잘못되는지 확인해보기

# Step1. 테스트 메서드 선언
```swift
import ComposableArchitecture
import XCTest


@MainActor
final class CounterFeatureTests: XCTestCase {
  func testNumberFact() async {
    let store = TestStore(initialState: CounterFeature.State()) {
      CounterFeature()
    }
  }
}
```
- 테스트할 스토어 선언
- 유저가 Fact 버튼을 누르는 흐름을 에뮬레이팅하길 원함
- progress indicator가 보이고
- 잠시 뒤에 숫자에 관련한 사실이 피드백됨

# Step2. `.factButtonTapped` 으로 액션을 보내 사용자가 버튼을 누른 것을 가정
```swift 
import ComposableArchitecture
import XCTest


@MainActor
final class CounterFeatureTests: XCTestCase {
  func testNumberFact() async {
    let store = TestStore(initialState: CounterFeature.State()) {
      CounterFeature()
    }


    await store.send(.factButtonTapped) {
      $0.isLoading = true
    }
    // ❌ An effect returned for this action is still running. 
    // It must complete before the end of the test. …
  }
}
```
- 이렇게 하면 테스트가 실행될 것 같지만 실패함 
- 전에 배운 것과 마찬가지로 액션에서 파생되는 모든 Effect가 테스트가 완료가 되어야함
- 네트워크 요청이 완료되기 전에 테스트가 끝나서 실패로 처리됨

# Step3. 테스트 수정
```swift
import ComposableArchitecture
import XCTest


@MainActor
final class CounterFeatureTests: XCTestCase {
  func testNumberFact() async {
    let store = TestStore(initialState: CounterFeature.State()) {
      CounterFeature()
    }


    await store.send(.factButtonTapped) {
      $0.isLoading = true
    }
    await store.receive(\.factResponse, timeout: .seconds(1)) {
      $0.isLoading = false
      $0.fact = "???"
    }
  }
}
```
- 테스트를 수정하기 위해 네트워크 요청이 완료되길 기다리고 fact 응답액션을 받아야함
- 서버로 부터 오는 fact를 어떻게 검증할 것인지가 관건
- 매번 숫자에 대한 사실을 요청할 때마다 숫자가 달라서 다른 답이 올 것임

# Step4. 테스트 실행 
```swift
    await store.receive(\.factResponse, timeout: .seconds(1)) {
      $0.isLoading = false
      $0.fact = "???"
    }
    // ❌ A state change does not match expectation: …
    //
    //       CounterFeature.State(
    //         count: 0,
    //     −   fact: "???",
    //     +   fact: "0 is the atomic number of the theoretical element tetraneutron.",
    //         isLoading: false,
    //         isTimerRunning: false
    //       )
  }
```
- 테스트를 실행하면 fact값을 우리가 예측할 수 없어서 테스트가 실패됨
- 