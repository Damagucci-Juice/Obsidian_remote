# 이전 아티클
[[[SwiftUI-TCA] 1. Reducer 생성]]
[[[SwiftUI-TCA] 2. SwiftUI의 View와 통합]]
[[[SwiftUI-TCA] 3. 앱과 연결]]
[[[SwiftUI-TCA] 4. Side effects 추가]]
[[[SwiftUI-TCA] 5. 네트워크 요청 수행]]

> 타이머를 만들고 1초마다 counter를 1씩 더하기

# Step1. UI에서 타이머 버튼 추가
```swift 
struct CounterView: View {
  let store: StoreOf<CounterFeature>


  var body: some View {
    WithViewStore(self.store, observe: { $0 }) { viewStore in
      VStack {
        Text("\(viewStore.count)")
          .font(.largeTitle)
          .padding()
          .background(Color.black.opacity(0.1))
          .cornerRadius(10)
        HStack {
          Button("-") {
            viewStore.send(.decrementButtonTapped)
          }
          .font(.largeTitle)
          .padding()
          .background(Color.black.opacity(0.1))
          .cornerRadius(10)


          Button("+") {
            viewStore.send(.incrementButtonTapped)
          }
          .font(.largeTitle)
          .padding()
          .background(Color.black.opacity(0.1))
          .cornerRadius(10)
        }
        Button(viewStore.isTimerRunning ? "Stop timer" : "Start timer") {
          viewStore.send(.toggleTimerButtonTapped)
        }
        .font(.largeTitle)
        .padding()
        .background(Color.black.opacity(0.1))
        .cornerRadius(10)


        Button("Fact") {
          viewStore.send(.factButtonTapped)
        }
        .font(.largeTitle)
        .padding()
        .background(Color.black.opacity(0.1))
        .cornerRadius(10)


        if viewStore.isLoading {
          ProgressView()
        } else if let fact = viewStore.fact {
          Text(fact)
            .font(.largeTitle)
            .multilineTextAlignment(.center)
            .padding()
        }
      }
    }
  }
}
```
- View -> reduce 순서, 다음 스텝에서 추가 예정
- `isTimerRunning` state에 따라서 Start Timer거나 Stop Timer라는 글자를 보여주는 버튼 생성

# Step2. Reducer에 타이머를 핸들링하는 상태와 액션 추가
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
    case toggleTimerButtonTapped
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
        
      case .toggleTimerButtonTapped:
        state.isTimerRunning.toggle()
        return .run { send in
        }
      }
    }
  }
}
```
- 리듀서에 `isTimerRunning` 상태와 `toggleTimerButton` 액션을 추가 
- 새로운 행동에 대한 기본 로직 스텁을 추가(`.run` 이펙트)  
	- 비동기 작업을 처리

# Step3. run 이펙트 안에 타이머 로직 추가 
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
    case toggleTimerButtonTapped
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
        
      case .toggleTimerButtonTapped:
        state.isTimerRunning.toggle()
        return .run { send in
          while true {
            try await Task.sleep(for: .seconds(1))
          }
        }
      }
    }
  }
}

```
- 다소 조잡한 방식의 타이머
- while 문 안에서 1초 동안 슬립을 하는 방식

# Step4. 1초 변화에 따른 액션 추가
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
        return .run { send in
          while true {
            try await Task.sleep(for: .seconds(1))
            await send(.timerTick)
          }
        }
      }
    }
  }
}
```
- 타이머에서 1초가 경과할 때마다 실행할 새로운 액션을 추가
- 이 액션에서 state의 counter에 1씩 더함 
- 여전히 버그는 있는데, Start Timer 버튼은 잘 동작하지만 Stop Timer는 동작하지 않음 

# Step5. Stop 액션 추가
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
            while true {
              try await Task.sleep(for: .seconds(1))
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
- `effectCancellation` 기능 활용
- Timer에 Cancellable을 채택시키면 ID를 부여 가능 
- 취소를 원할 때 `.cancel(id: CancelID.timer)`를 부름
- 이런 방식으로 오래 살아남는 이펙트를 관리(시작, 취소)할 수 있음

# 출처
https://pointfreeco.github.io/swift-composable-architecture/main/tutorials/composablearchitecture/01-02-addingsideeffects
