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
> `Store` 는 `let`으로만 선언가능
> 뷰에 관찰될 필요는 없다

# Step3.
```swift
struct CounterView: View {
	let store: StoreOf<CounterFeature>
	
	var body: some View {
VStack { Text("0") .font(.largeTitle) .padding() .background(Color.black.opacity(0.1)) .cornerRadius(10) HStack { Button("-") { } .font(.largeTitle) .padding() .background(Color.black.opacity(0.1)) .cornerRadius(10) Button("+") { } .font(.largeTitle) .padding() .background(Color.black.opacity(0.1)) .cornerRadius(10) } }
	}
}
```

2023-12-14 작성중 ...