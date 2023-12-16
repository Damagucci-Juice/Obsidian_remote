# 이전 아티클
[[[SwiftUI-TCA] 1. Reducer 생성]]
[[[SwiftUI-TCA] 2. SwiftUI의 View와 통합]]
[[[SwiftUI-TCA] 3. 앱과 연결]]

> 작성한 기능이 바깥 세상과 소통하는 법을 배우고 바깥 데이터로부터 기능에 피드백주기

# 0. Side effect란? 
- 기능 개발에서 가장 중요한 측면
- ex) 외부 API와 소통, 파일 시스템과 상호작용, 시간에 기반한 비동시 코드 동작 
- State에서 변화는 간단
	- 같은 액션과 스테이트를 가지고 있는 동일한 리듀서를 실행하면 항상 같은 결과를 얻음 
- Effect는 네트워크 연결, 디스크 권한 허용 등 외부 세계에 항상 영향을 받음
- Effect는 실행시마다 다른 결과값을 얻음 
- 왜 리듀서를 채택하는 것만으로 Effect와 관련한 작업을 직접 수행하지 못하는지 확인하고, Effect를 수행하는 라이브러리가 무엇이 있는지 알아보기
# Step1. 
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
        Button("Fact") {
          viewStore.send(.factButtonTapped)
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
- 현재 Counter가 나타내는 숫자에 대한 사실을 네트워크에서 요청하는 버튼을 만듦
- 리듀서에서 요청해서 뷰에 보여주는 방식과 뷰에서 바로 요청하는 방식이 나뉨 
- 여기선 뷰에서 바로 요청하는 방식 

# Step2. 
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
- 화면 밑에 프로그레스 뷰를 넣을 예정 
- 아직 `CounterFeature`에서 isLoading, fact를 넣지는 않았음

# Step3. 


2023-12-15 작성중...