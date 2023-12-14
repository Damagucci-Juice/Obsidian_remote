# 이전 아티클
[[SwiftUI-TCA] 1. Reducer 생성]]
[[SwiftUI-TCA] 2. SwiftUI의 View와 통합]]

> TCA를 채택한 코드를 실행하기 위해 시작점을 변경하는 방법을 배움

# Step1. 
```swift
import SwiftUI

@main
struct MyApp: App {
  var body: some Scene {
    WindowGroup {
      ContentView()
    }
  }
}
```
- 앱을 생성하면 `App.swift` 파일의 기본 코드

# Step2. 
```swift
import ComposableArchitecture
import SwiftUI

@main
struct MyApp: App {
  var body: some Scene {
    WindowGroup {
      CounterView(
        store: Store(initialState: CounterFeature.State()) {
          CounterFeature()
        }
      )
    }
  }
}
```
- 프리뷰에서 했던 것처럼 CounterView에 Store를 제공해서 선언하여 시작점 변경

# Step3. 
```swift
import ComposableArchitecture
import SwiftUI

@main
struct MyApp: App {
  static let store = Store(initialState: CounterFeature.State()) {
    CounterFeature()
  }

  var body: some Scene {
    WindowGroup {
      CounterView(store: MyApp.store)
    }
  }
}
```
- 스토어가 한번만 생성되는 것이 중요
- 대부분의 경우 스토어는 `WindowGroup` 내에서 생성하면 족함
- 특정 경우 `Scene`에서 접근해야할 경우 `static let`으로 선언

# Step4. 
```swift
import ComposableArchitecture
import SwiftUI

@main
struct MyApp: App {
  static let store = Store(initialState: CounterFeature.State()) {
    CounterFeature()
      ._printChanges()
  }

  var body: some Scene {
    WindowGroup {
      CounterView(store: MyApp.store)
    }
  }
}
```
- `_printChanges(_:)`: 콘솔에 리듀서가 처리하는 모든 액션을 프린트함