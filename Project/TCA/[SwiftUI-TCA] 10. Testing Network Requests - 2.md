# 이전 아티클
- [[[SwiftUI-TCA] 1. Reducer 생성]]
- [[[SwiftUI-TCA] 2. SwiftUI의 View와 통합]]
- [[[SwiftUI-TCA] 3. 앱과 연결]]
- [[[SwiftUI-TCA] 4. Side effects 추가]]
- [[[SwiftUI-TCA] 5. 네트워크 요청 수행]]
- [[[SwiftUI-TCA] 6. 타이머 관리]]
- [[[SwiftUI-TCA] 7. State를 Test 하기]]
- [[[SwiftUI-TCA] 8. Effect(Timer)를  Test 하기]]
- [[[SwiftUI-TCA] 9. Testing Network Requests - 1]]

> - 이전 장에서 네트워크 요청에 대한 테스트는 컨트롤이 불가능한 외부 의존성 때문에 전통적인 방식의 테스트가 어렵다는 것을 확인 했음
> - 수행도 오래 걸려 까다로움
> - 외부 [의존성](https://pointfreeco.github.io/swift-composable-architecture/main/documentation/composablearchitecture/dependencymanagement)에 대해서 의존성을 조절하는 방법을 권장

# Step1. 의존성 관리 구조체 선언
```swift
import ComposableArchitecture


struct NumberFactClient {
  var fetch: (Int) async throws -> String
}
```
- TCA를 import 해줌
- 이 구조체는 기능안에서의 어떠한 의존성이든 조절할 수 있는 필수적인 도구에 접근하도록 함
- 의존성을 추상화하는 인터페이스를 모델링하는 것이 의존성을 조절하는 첫 번째 스텝
- 정수를 받아서 문자열을 반환하는 엔드포인트를 던지는 단일 async 메서드가 케이스
# Step2. 의존성 인터페이스 모델링
```swift
import ComposableArchitecture
import Foundation


struct NumberFactClient {
  var fetch: (Int) async throws -> String
}


extension NumberFactClient: DependencyKey {
  static let liveValue = Self(
    fetch: { number in
      let (data, _) = try await URLSession.shared
        .data(from: URL(string: "http://numbersapi.com/\(number)")!)
      return String(decoding: data, as: UTF8.self)
    }
  )
}
```
- 시뮬레이터나 장치에서 구동을 했을 때 의존성이 "생존"했을 때의 버전의 사용을 허가함
- 물론 테스트 동안 더 섬세한 조절이 들어간 버전도 사용가능함
>[!tip]
>- 물론 프로토콜이 의존성의 인터페이스를 추상화하는 지금까지의 가장 대중적인 방법이기는 하나, 유일한 방법은 아니다. 
>- 인터페이스를 대표하는 변경가능한 프로퍼티가 있는 구조체를 사용하기를 권장한다.
>- 그 구조체의 값을 프로토콜을 준수하는 것으로 나타낸다. 
>- 원한다면 프로토콜을 의존성을 위해 사용할 순 있지만, 구조체로 의존성을 선언하는 것에 대해 더 관심이 있다면, [일련의 비디오](https://www.pointfree.co/collections/dependencies)를 보시라

- 다음으로라이브러리에 만든 의존성을 등록해야함
- 클라이언트가 `liveValue`라는 프로퍼티를 제공하길 요구하는 `DependencyKey` 프로토콜을 채택
	- 이 값은 시뮬레이터나 디바이스에서 기능을 동작시킬 때 사용되는 값임
	- 또한 이 프로퍼티는 "생존"한 네트워크 요청을 만들기에 적절한 장소

> [!note]
> - TCA에서 의존성 관리 시스템은 기술적으로 다른 swift-dependencies라는 라이브러리에 의해서 제공됨
> - 분리되어서 UIKit과 SwiftUI에서도 사용하기 편리하게 되어 더 분명해짐

# Step3. 
```swift
import ComposableArchitecture
import Foundation


struct NumberFactClient {
  var fetch: (Int) async throws -> String
}


extension NumberFactClient: DependencyKey {
  static let liveValue = Self(
    fetch: { number in
      let (data, _) = try await URLSession.shared
        .data(from: URL(string: "http://numbersapi.com/\(number)")!)
      return String(decoding: data, as: UTF8.self)
    }
  )
}


extension DependencyValues {
  var numberFact: NumberFactClient {
    get { self[NumberFactClient.self] }
    set { self[NumberFactClient.self] = newValue }
  }
}
```
- 라이브러리에 의존성을 등록하는 두번째 스텝은 getter와 setter가 있는 `DependencyValues`에 연산 프로퍼티를 추가하는 것
- 이는 리듀서에서 `@Dependency(\.numberFact)` 와 같은 문법을 사용할 수 있게 해줌 
- 이것이 의존성 앞에 조절 가능한 인터페이스를 넣는데 필요한 모든것임
- 이런 앞선 작업들로 기능에 의존성을 사용할 수 있음
- 테스트안에서 테스트 친화적인 의존성의 버전을 사용하기 시작함

>[!note]
> - 라이브러리에 의존성을 등록하는 것은 SwiftUI에 환경 변수값을 등록하는 것과 다름
> - defaultValue를 제공하는 `EnvironmentKey`를 채택하길 요구하고 EnvironmentValues에 extension을 통해서 연산 프로퍼티를 제공해야함

# Step4.리듀서에 생성한 의존성 추가
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
  @Dependency(\.numberFact) var numberFact


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
          try await send(.factResponse(self.numberFact.fetch(count)))
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
- `CounterFeature.swift`에 `@Dependency` 프로퍼티 래퍼를 이용해 새로운 의존성을 추가
- 여기선 number fact client
- 그러고 나서 `factButtonTapped`로부터 반환되는 effect에서 살아있는 네트워크 요청을 만들기 위해 URLSession에 접근하는 대신  `numberFact` 의존성을 사용
- 이를 통해 기능에서 행태에 대해 즉각적이고 쉽게 유닛테스트를 작성 가능 
	- 네트워크 이용을 피하면서 즉각적이고 결정론적으로 시험을 통과

# Step5. 테스트 실행
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
    // ❌ @Dependency(\.numberFact) has no test implementation, but was
    //    accessed from a test context:
    //
    //   Location:
    //     TCATest/CounterFeature.swift:70
    //   Dependency:
    //     NumberFactClient
    //
    // Dependencies registered with the library are not allowed to use
    // their default, live implementations when run from tests.
  }
}
```
- 별다른 변경 사항 없이 테스트를 돌려도 돌려짐
- 단 테스트 실패가 뜸
- 실패 메시지가 재밌는데 "이 기능이 살아있는 의존성을 사용한다"라고 알려줌
- 우연히 실수로 네트워크 요청(디스크에 파일 쓰기, 분석을 추적)을 만들때마다 테스트에서 그러지 말라고 알려줌
- 우리는 테스트에서 