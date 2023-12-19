#Test 
**목차** 
- [[#이전 아티클|이전 아티클]]
- [[#Step0. 들어가기 전에|Step0. 들어가기 전에]]
- [[#Step1. 테스트 메서드 선언|Step1. 테스트 메서드 선언]]
- [[#Step2.|Step2.]]

# 이전 아티클
[[[SwiftUI-TCA] 1. Reducer 생성]]
[[[SwiftUI-TCA] 2. SwiftUI의 View와 통합]]
[[[SwiftUI-TCA] 3. 앱과 연결]]
[[[SwiftUI-TCA] 4. Side effects 추가]]
[[[SwiftUI-TCA] 5. 네트워크 요청 수행]]
[[[SwiftUI-TCA] 6. 타이머 관리]]
[[[SwiftUI-TCA] 7. State를 Test 하기]]
[[[SwiftUI-TCA] 8. Effect(Timer)를  Test 하기]]



> 외부 시스템에 의존하는 Network Request를 테스트하는 방법을 배우기

# Step0. 들어가기 전에
- 네트워크 요청도 Side Effect의 일종
	- 서버 상황이나 네트워크 연결 상태에 따라 결과값이 달라지기 때문 
- NumbersAPI를 단순한 방식으로 테스트해보고 무엇이 잘못되는지 확인해보기

# Step1. 테스트 메서드 선언
```swift
import ComposableArchitecture
import XCTest


@MainActor
final class CounterFeatureTests: XCTestCase {
  func testNumberFact() async {
    let store = TestStore(initialState: CounterFeature.State()) {
      CounterFeature()
    }
  }
}
```
- 테스트할 스토어 선언
- 유저가 Fact 버튼을 누르는 흐름을 에뮬레이팅하길 원함
- progress indicator가 보이고
- 잠시 뒤에 숫자에 관련한 사실이 피드백됨

# Step2. fact tappingㅇ