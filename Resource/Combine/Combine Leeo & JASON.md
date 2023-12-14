> 비동기 코드의 선언적 접근, 코드의 간소화을 위한 프레임 워크 

> [!리이오의 노트]
> 
처음부터 비동기를 선언적으로 처리하는게 어려우니, 일단 MVC를 하고 거기서 반복되는 것들 모아 놓고 가다가 필요해지면 MVVM을 하는게 좋은 것처럼 일반적인 비동기 코드를 쓰다가 좀더 한 곳에서 처리하는게 필요해지면 Combine을 이용해보세요. 

![[직렬, 병렬.png]]

# 비동기 작업을 하는 여러 방법
- Grand Central Dispatch(GCD)
	- DispatchQueue를 사용해서 끝을 알려준다. 
- Delegates pattern
	- 작업의 결과를 알려준다.
- Notification Center
	- 알림을 주는 방법
- Closure: Completion Handler를 이용해서 작업의 끝을 알려준다. 
- Async/Await
... 많은 비동기 코드를 구현하는 방법들이 있다. Combine도 그 중 하나. 
# 중요한 개념 잡기
- Publisher
- Subscriber
- Operator

## Subscriber
- 해당 채널의 업데이트된 소식을 알 수 있는 구독자

## Publisher
- 사전적 의미: 출판하다
- 퍼블리셔는 **이벤트를 퍼블리싱**한다.
- 해당 이벤트는 구독자가 원하든 그렇지 않든 구독자에게 간다.
- 데이터가 왔을 때 그려진다.

## Operator
- 만약 더 글로리를 넷플릭스에서 배포한다면, 일본 구독자에게는 중간에 일본인들이 원하는 형태로 바꾸어줄 작업이 필요하다. 
- Combine을 배운다는 것은 어떤 Operator가 있는지 공부한다는 말
- 기초적인 `Map, filter, reduce`를 학습해보세요.

**실습 코드**

```swift
import SwiftUI

struct ContentView: View {
	  private let publisher = [1,2,3,4].publisher
	  @State private var myData = [Int]()
	  var body: some View {
		  VStack {
				List {
					ForEach(myData, id: \.self) { data in 
						Text("\(data)")
					}
				}
				Button {
				  publisher.sink { result in 
					  myData.appned(result)
				  }
				} :label {
				  Text("Tap")
				}
		  }
		  
	  }
}
```

---
# 어느 Operator를 사용해야할까? 
[https://miro.com/app/board/uXjVNdss7sg=/?share_link_id=763588488147](https://miro.com/app/board/uXjVNdss7sg=/?share_link_id=763588488147 "https://miro.com/app/board/uxjvndss7sg=/?share_link_id=763588488147")

- sink, assign
- [WWDC practice](https://developer.apple.com/wwdc19/721)
- [Combine Introduce](https://developer.apple.com/wwdc19/722)
