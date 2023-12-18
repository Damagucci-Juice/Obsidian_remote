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
- 리듀서에서 Action에 따라 State를 변경하고 나면 Effect를 반환
- Effect:  Store에 의해 수행되는 비동기 작업 단위
	- 무엇이 외부 시스템과 상호작용하고 처리된 데이터를 리듀서에 피드백하는 역할

# Step1. run으로 선언
```swift
import ComposableArchitecture


@Reducer
struct CounterFeature {
  struct State: Equatable {
    var count = 0
    var fact: String?
    var isLoading = false
  }


  enum Action {
    case decrementButtonTapped
    case factButtonTapped
    case incrementButtonTapped
  }


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
        return .run { send in
          // ✅ Do async work in here, and send actions back into the system.
        }
        
      case .incrementButtonTapped:
        state.count += 1
        state.fact = nil
        return .none
      }
    }
  }
}
```

- 주된 방법은`run(priority:Operation:catch:fileID:Line:)`이라는 Effect 의 Static 메서드로 선언
- 어떠한 비동기 작업도 수행할 수 있는 컨텍스트를 제공
- `Send`를 통해서 시스템에 Action을 다시 피드백할 수 있도록하는 것도 가능

# Step2. 
```swift
import ComposableArchitecture


@Reducer
struct CounterFeature {
  struct State: Equatable {
    var count = 0
    var fact: String?
    var isLoading = false
  }
  
  enum Action {
    case decrementButtonTapped
    case factButtonTapped
    case incrementButtonTapped
  }
  
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
        }
        
      case .incrementButtonTapped:
        state.count += 1
        state.fact = nil
        return .none
      }
    }
  }
}
```
- .run의 후행 클로저는 네트워크 요청을 하기에 최적의 장소
- numbersapi.com은 Https 지원 안해서 Http를 허용하는 Info.plist 작성

# Step3. 
```swift
import ComposableArchitecture


@Reducer
struct CounterFeature {
  struct State: Equatable {
    var count = 0
    var fact: String?
    var isLoading = false
  }


  enum Action {
    case decrementButtonTapped
    case factButtonTapped
    case incrementButtonTapped
  }


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
          state.fact = fact
          // 🛑 Mutable capture of 'inout' parameter 'state' is not allowed in
          //    concurrently-executing code
        }
        
      case .incrementButtonTapped:
        state.count += 1
        state.fact = nil
        return .none
      }
    }
  }
}
```
- state.fact를 .run안에서 변경하는 것은 불가
- `sendable` 클로저가 `inout` 상태를 캡쳐하는 것은 불가능하기 때문에 컴파일러가 변경을 막는다. 
- 이 점이 State 변경을 Reducer안에 Effect에서 막은 이유

# Step4. 가져온 정보를 action을 통해 다시 받기 
```swift
import ComposableArchitecture


@Reducer
struct CounterFeature {
  struct State: Equatable {
    var count = 0
    var fact: String?
    var isLoading = false
  }


  enum Action {
    case decrementButtonTapped
    case factButtonTapped
    case factResponse(String)
    case incrementButtonTapped
  }


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
      }
    }
  }
}
```
- 네트워크 요청에 응답 정보를 effect로 부터 reducer로 가져오기 위해서 `factResponse`라는 Action이 필요
- 이 액션은 연관 값(Associated value)를 스트링으로 가지고 있을 것
- 비동기 작업후에 Effect안에서 이 액션을 send 하고 나서,
- 이 액션 안에서 isLoading과 fact를 업데이트 가능

>[!warning]
> 현재는 에러를 무시하지만, 더 완성도 있는 앱이 되기 위해서 에러 핸들링을 해야하는데, 이 때 `TaskResult` 반환되는 에러에 적절한 행동을 정의

2023.12.15 작성중 ...