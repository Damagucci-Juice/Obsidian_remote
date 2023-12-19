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
    await store.send(.toggleTimerButtonTapped) {
      $0.isTimerRunning = false
    }
  }
}
```
- 시작된 effect가 종료되는 것까지 확인하면 테스트가 완료됨 
- 상태를 확인하는 것뿐 아니라 실질적으로 시간이 지남에 따라서 counter가 증가하는 것을 확인해야함
	- `receive(_:timeout:assert:file:line:)`을 통해서 확인
	- 액션을 받고 그 안에서 상태가 어떻게 변화하는지 검증하는 도구 

# Step4. 시간 경과를 테스트 
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
    await store.receive(\.timerTick) {
      $0.count = 1
    }
    await store.send(.toggleTimerButtonTapped) {
      $0.isTimerRunning = false
    }
  }
}
```
- timerTick을 받는 새로운 assertion을 추가
- counter 가 1로 증가할 것을 검증
>[!note]
> key path를 사용해 이펙트에서 받을 액션 열거형의 케이스를 구분 
- 테스트를 실행하면 1초 이상 걸리면서, 어떨 때는 통과하고 어떨 때는 떨어지는 것을 확인

# Step5. 타이머 테스트가 실패하는 이유
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
    await store.receive(\.timerTick) {
      $0.count = 1
    }
    // ✅ Test Suite 'Selected tests' passed at 2023-08-04 11:17:44.823.
    //        Executed 1 test, with 0 failures (0 unexpected) in 1.044 (1.046) seconds
    //    or:
    // ❌ Expected to receive an action, but received none after 0.1 seconds.
    await store.send(.toggleTimerButtonTapped) {
      $0.isTimerRunning = false
    }
  }
}


```
- 이게 발생하는 이유는 타이머는 액션을 방출하기 위해 온전한 1초가 걸림
- test store는 단지 액션을 받든지 말든지 약간의 시간만을 기다리기 때문
- 테스트가 1초를 온전히 기다리는 경우도 있고 그렇지 않은 경우도 있다는 말

# Step6. TestStore가 더 기다리게 하는 방법
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
    await store.receive(\.timerTick, timeout: .seconds(2)) {
      $0.count = 1
    }
    await store.send(.toggleTimerButtonTapped) {
      $0.isTimerRunning = false
    }
  }
}
```
- `timeout`을 이용해 온전히 기다리게 하는 방법
- Test.sleep이 엄밀한 도구가 아니라 1초 이상을 기다려야 함
- 이렇게 하면 테스트를 통과할 것
- 만약에 10초 뒤에를 테스트하고 싶다면 우리는 꼼짝없이 10초를 기다려야함
- 이게 진짜 원하는게 맞는가? 
- 실제 시간을 기다리도록 하는 전역적이고 조정 불가능한 `Task.sleep` 함수 대신 다른것을 써야함 
- `SwiftClock`을 사용
	- 시뮬레이터와 장치에서 돌아가는 연속적인 시계를 제공
	- 테스트에서는 조절 가능한 test clock을 사용
	- 다행히 TCA는 조절 가능한 시계도 기본으로 제공

# Step7. TCA 타이머 추가
```swift 
import ComposableArchitecture


@Reducer
struct CounterFeature {
  struct State: Equatable {
    var count = 0
    var fact: String?
    var isLoading = false
    var isTimerRunning = false
  }


  enum Action {
    case decrementButtonTapped
    case factButtonTapped
    case factResponse(String)
    case incrementButtonTapped
    case timerTick
    case toggleTimerButtonTapped
  }


  enum CancelID { case timer }


  @Dependency(\.continuousClock) var clock


  var body: some ReducerOf<Self> {
    Reduce { state, action in
      switch action {
      case .decrementButtonTapped:
        state.count -= 1
        state.fact = nil
        return .none
        
      case .factButtonTapped:
        state.fact = nil
        state.isLoading = true
        return .run { [count = state.count] send in
          let (data, _) = try await URLSession.shared
            .data(from: URL(string: "http://numbersapi.com/\(count)")!)
          let fact = String(decoding: data, as: UTF8.self)
          await send(.factResponse(fact))
        }
        
      case let .factResponse(fact):
        state.fact = fact
        state.isLoading = false
        return .none
        
      case .incrementButtonTapped:
        state.count += 1
        state.fact = nil
        return .none
        
      case .timerTick:
        state.count += 1
        state.fact = nil
        return .none
        
      case .toggleTimerButtonTapped:
        state.isTimerRunning.toggle()
        if state.isTimerRunning {
          return .run { send in
            for await _ in self.clock.timer(interval: .seconds(1)) {
              await send(.timerTick)
            }
          }
          .cancellable(id: CancelID.timer)
        } else {
          return .cancel(id: CancelID.timer)
        }
      }
    }
  }
}
```
- `CounterFeature.swift` 파일에서 리듀서에 연속 시계에 대한 종속성을 추가 
- 후에 실행하면 Task.sleep 대신 Clock을 이용하고 있는 것을 확인
- 앞단에서 시간에 기반한 약간의 비동기 작업 덕분에 이제 즉각적이고 결정론적으로 간단한 테스트가 즉시 통과하는 것을 볼 수 있음
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
    await store.receive(\.timerTick, timeout: .seconds(2)) {
      $0.count = 1
    }
    await store.send(.toggleTimerButtonTapped) {
      $0.isTimerRunning = false
    }
  }
}
```

# Step8. 테스트에 의존성 시계 추가
```swift
import ComposableArchitecture
import XCTest


@MainActor
final class CounterFeatureTests: XCTestCase {
  func testTimer() async {
    let clock = TestClock()


    let store = TestStore(initialState: CounterFeature.State()) {
      CounterFeature()
    } withDependencies: {
      $0.continuousClock = clock
    }


    await store.send(.toggleTimerButtonTapped) {
      $0.isTimerRunning = true
    }
    await clock.advance(by: .seconds(1))
    await store.receive(\.timerTick) {
      $0.count = 1
    }
    await store.send(.toggleTimerButtonTapped) {
      $0.isTimerRunning = false
    }
  }
}
```
- testTimer 메서드 상단에 의존성 테스트 시계 선언하고 의존성 주입
- 기능의 리듀서에서 사용할 것으로 예상되는 시계
- 이렇게 하면 테스트가 timerTick 액션을 받기 전에 실험용 시계에 1초 증가하라고 명령 가능
- 즉시 통과하고, 100% 통과할 것이라 자신 