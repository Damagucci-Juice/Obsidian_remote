추천수 600 업
https://medium.com/@adha_fajri/clean-architecture-in-swiftui-b184e0e687f8
viewModel X
Combine X 
github star 7 
설명 햇갈림
https://gon125.github.io/posts/SwiftUI를-위한-클린-아키텍처/
github star 4.9k

https://paulallies.medium.com/clean-architecture-in-the-flavour-of-swiftui-5-5-8430786a83
- 이거 ViewModel쓰는 거임 

추천수 300 업 
https://medium.com/@walfandi/a-beginners-guide-to-clean-architecture-in-ios-building-better-apps-step-by-step-53e6ec8b3abd
설명 깔쌈
https://medium.com/@mehran.kmlf/clean-architecture-combine-for-swiftui-caf252c0f10c
# UIKit vs SwiftUI 
- UIKit: 발생한 이벤트에 대한 반응, 명령형
- SwiftUI: 함수가 상태를 구독하고 있다가 새로 그리는 형식, FlowChart 구독형, 선언형
	- 상태에 대한 참조를 유지하고 있다가 변화하면 새로 그림

# MVVM 패턴 적용 예시
- SwiftUI는 MVVM이 내재
	- `@State`가 예시(복잡한 경우 `@ObservableObejct`)
	- 외부 뷰모델에 종속적인게 아닌 뷰 자체의 상태를 볼 수 있도록 바인딩을 걸어줌 

# 코드 예시
별도의 뷰 모델을 만들어주는 것이 아니라, View 안에 nested type으로 뷰모델 클래스를 만들어줌 
```swift
// MARK: - Model, 데이터 컨테이너
struct Country {
    let name: String
}

// MARK: - SwiftUI 뷰
struct CountriesList: View {
    
    @ObservedObject var viewModel: ViewModel
    
    var body: some View {
        List(viewModel.countries) { country in
            Text(country.name)
        }
        .onAppear {
            self.viewModel.loadCountires()
        }
    }
    
}

// MARK: - ViewModel, 비지니스 로직(도메인)을 캡슐화
extension CountriesList {
    class ViewModel: ObservableObject {
        @Published private(set) var countries: [Country] = []
        
        private let service: WebService
        
        func loadCountires() {
            service.getCountries { [weak self] result in
                self?.countries = result.value ?? []
            }
        }
    }
}
```

# Coordinator
- VC가 새로운 VC로의 네비게이션 전환을 담당하는 강한 결합이 일어나는 것을 해소하기 위한 장치
- SwiftUI에서는 Router가 불필요하다고 저자는 말함
	- 단순히 `View`가 그리기 알고리즘에 불과하기 때문에 추출이 불가능해서 그러함

# 다른 대안(VIP, RIBs, VIP)는 SwiftUI에 적용이 불가한가? 
네

![cleanArchitecture](https://github.com/nalexn/blob_files/blob/master/images/swiftui_arc_001_d.png?raw=true)

# AppState
유저의 데이터, 인증 토큰, 스크린 내비게이션 상태(선택된 탭, 보이는 시트 등), 시스템 상태(활성, 백그라운드 등)와 같은 state들이 이에 해당합니다.

# 코드 테스트 커버리지 97% 돌파! 