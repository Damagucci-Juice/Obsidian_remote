# 이전 아티클
[[SwiftUI-TCA] 1. Reducer 생성]]

> 1탄에서 만든 reducer를 가지고, SwiftUI 뷰와 연결하는 방법을 배움 
- `Store`: 런타임을 나타냄
- `ViewStore`: 런타임의 관찰자를 나타냄

# Step1. 
```swift
import ComposableArchitecture
import SwiftUI

struct CounterView: View {
	  var body: some View {
		  EmptyView()
	  }
}
```
- 리듀서와 View 타입을 한 파일 안에 넣는 것이 개인적 선호
- 어떤 사람들은 분리하는 것을 선호하기도 함

# Step2. 
```swift
struct CounterView: View {
	let store: StoreOf<CounterFeature>
	
	var body: some View {
		EmptyView()
	}
}
```
- view에 `Store`를 생성
- 이전에 만든 reducer에 대해서 제네릭함
- 스토어는 기능에 대한 runtime을 의미
- 상태를 업데이트하기 위해 액션을 처리할 수 있는 객체를 의미
- Effect를 실행 가능하고 이펙트로부터 나오는 데이터에 대한 피드백을 시스템에 전달 가능
>[!tip]
> - `Store` 는 `let`으로만 선언가능
> - 뷰에서 관찰될 필요는 없다(`@StateObject`등 프로퍼티 래퍼를 통하지 않아도 된다는 말인듯(?))

# Step3.
```swift
struct CounterView: View {
  let store: StoreOf<CounterFeature>

  var body: some View {
    VStack {
      Text("0")
        .font(.largeTitle)
        .padding()
        .background(Color.black.opacity(0.1))
        .cornerRadius(10)
      HStack {
        Button("-") {
        }
        .font(.largeTitle)
        .padding()
        .background(Color.black.opacity(0.1))
        .cornerRadius(10)


        Button("+") {
        }
        .font(.largeTitle)
        .padding()
        .background(Color.black.opacity(0.1))
        .cornerRadius(10)
      }
    }
  }
}
```
- 버튼을 통해서 더하기 빼기를 하는 기본적인 뷰 계층
- 현재 Count를 나타냄
>[!note]
> - Store로 부터 상태를 직접 읽거나, 액션에 직접 명령을 하지 못한다. 
> - 행동(behavior)를 하기 위해 stubs를 제공함
> - `ViewStore`를 통해 실제 실행을 할 수 있음 

- ViewStore: SwiftUI의 뷰에서 편하게 Store를 관찰하는 객체
- 경량화된 문법을 제공
# Step4. 
```swift
extension CounterFeature.State: Equatable {}


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
      }
    }
  }
}
```
- ViewStore는 State가 `Equatable`을 채택해야 사용 가능
- 구조화된 ViewStore가 선언되면 버튼을 눌렀을 때 기능의 state와 action에 접근하는 것이 가능
>[!tip]
>- `observe: { $0 }`으로 스토어 안에 모든 상태(state)에 접근하고 있지만, 일반적으로 기능은 뷰에서 필요로하는 것 보다 많은 상태를 가진다.
>- 뷰가 자신의 일을 처리하기 위해 꼭 필요한 본질을 관찰하는지 알기 위해 [Performance](https://pointfreeco.github.io/swift-composable-architecture/main/documentation/composablearchitecture/performance/)아티클을 보세요.

# Step5. 프리뷰 생성
```swift
struct CounterPreview: PreviewProvider {
  static var previews: some View {
    CounterView(
      store: Store(initialState: CounterFeature.State()) {
        CounterFeature()
      }
    )
  }
}
```
- 실제 기능을 수행하기 위해 프리뷰를 제작
- `CounterView`를 선언 
- `StoreOf<CounterFeature>`에 기능이 시작하길 원하는 초기 상태를 제공하면서 선언
- 후행 클로저에 기능을 구동할 `Reducer`를 제공
- 이렇게 하면 프리뷰를 통해 counter 기능이 뷰로 수행되는 것 확인 가능
- 기능의 모든 상태와 행위가 `Reducer`에 포함되어 있어서 어떻게 수행하는지 변경하기 위해 다른 리듀서로도 프리뷰를 구동 가능(?)

# Step6. 프리뷰에서 리듀서 교체
```swift
struct CounterPreview: PreviewProvider {
  static var previews: some View {
    CounterView(
      store: Store(initialState: CounterFeature.State()) {
       // CounterFeature()
      }
    )
  }
}
```
- 리듀서를 제공하는 줄을 주석 처리
- 스토어는 아무 상태 변경이나 영향이 없는 리듀서를 받음 
- 어떠한 로직이나 행위에 대한 걱정 없이 시각적으로 프리뷰를 사용하도록 해줌

# [출처](https://pointfreeco.github.io/swift-composable-architecture/main/tutorials/composablearchitecture/01-01-yourfirstfeature#Create-a-reducer)