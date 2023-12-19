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
- 의존성을 추상화하는 인터페이스를 모델링하는 것이 의존성을 조절하는 첫 번째 스텝
- 정수를 받아서 문자열을 반환하는 엔드포인트를 던지는 단일 async가 케이스
- 시뮬레이터나 장치에서 구동을 했을 때 의존성이 "생존"했을 때의 버전의 사용을 허가함
- 물론 테스트 동안 더 섬세한 조절이 들어간 버전도 사용가능함

>[!tip]
>- 물론 프로토콜이 의존성의 인터페이스를 추상화하는 지금까지의 가장 대중적인 방법이기는 하나, 유일한 방법은 아니다. 
>- 인터페이스를 대표하는 변경가능한 프로퍼티가 있는 구조체를 사용하기를 권장한다.
>- 그 구