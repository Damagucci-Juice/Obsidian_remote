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

# Step0. 상태 변화를 테스트
- 오직 Reducer만 테스트하면 기능은 보장됨
	1. 액션이 보내졌을 때 state가 어떤식으로 변화하는지만 확인
	2. Effect가 어떻게 실행되고 실행의 결과가 시스템에 어떻게 전달되는지 확인
- Reducer가 순수 함수로 형성되기 때문에 State의 변화는 테스트하기 가장 쉬운 부분
- [`TestStore`](https://pointfreeco.github.io/swift-composable-architecture/main/documentation/composablearchitecture/teststore) 가 이 프로세스를 간소화 해놓았음 
	- 액션을 보내기, 간단한 assertion(검증) 작성, 검증이 실패했을 때 깔끔한 실패 메시지를 보여준다든지 하는 모든 것들을 모니터링하는 테스트 가능한 런타임

# Step1. 카운터의 더하기 빼기 기능 테스트
```swift
import ComposableArchitecture
import XCTest


@MainActor
final class CounterFeatureTests: XCTestCase {
  func testCounter() async {


  }
}
```
- 기본적인 구조는 위와 같음 
- 테스트가 선제적으로 `async`를 채택하고 있음
- TCA에서 비동기를 활용하고 있기 때문

# Step2. TestStore 생성
```swift
import ComposableArchitecture
import XCTest


@MainActor
final class CounterFeatureTests: XCTestCase {
  func testCounter() async {
    let store = TestStore(initialState: CounterFeature.State()) {
      CounterFeature()
    }
  }
}
```
- TestStore를 생성
	- 액션이 시스템에 보내짐에 따라서 기능이 어떤식으로 변화하하는지 행태를 검증하기 쉽게 해주는 도구
- Store를 생성하는 것과 마찬가지로 시작 기본 상태를 제공
- 후행 클로저에 이 기능을 수행할 리듀서를 명시

# Step3. 액션 보내기 
```swift
import ComposableArchitecture
import XCTest


@MainActor
final class CounterFeatureTests: XCTestCase {
  func testCounter() async {
    let store = TestStore(initialState: CounterFeature.State()) {
      CounterFeature()
    }


    await store.send(.incrementButtonTapped)
    await store.send(.decrementButtonTapped)
  }
}
```
- 사용자가 실제 하는 것처럼 에뮬레이팅하여 스토어에 액션을 보낼 수 있음
- 더하고 빼기 기능을 한번씩 수행
>[!note]
>- 테스트스토어의 `send(_:assert:file:line:)`메서드는 비동기 코드
>- 대부분의 기능이 비동기적인 사이드 이펙트를 포함하고 있어 TestStore 또한 이런 이펙트를 추적하기 위해 비동기 문맥을 사용

# Step4. 테스트 실행하기 