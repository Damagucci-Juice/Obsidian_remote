# [링크](https://pointfreeco.github.io/swift-composable-architecture/main/tutorials/composablearchitecture/01-01-yourfirstfeature)

# 학습 목표
기능의 행동과 논리를 수행하기 위해 `reducer`를 생성하는 법을 배우고, 그 기능을 SwiftUI의 View와 연결하는 법을 배운다. 

# Section 1, Reducer 생성
- Composable Architecture에서 기능이 지어지는 기본 단위는 `Reducer()` 매크로와 `Reducer` 프로토콜 
- 프로토콜을 준수하는 것은 앱의 기능의 로직과 행동을 나타냄 
- 액션이 시스템에 전달되었을 때 현재 상태를 다음 상태로 진화하는 방법과 바깥 세상과 시스템 내부로 데이터를 다시 피드백하는 소통 방식을 포함

그리고 가장 중요하게는 **기능의 핵심 로직과 행동이 SwiftUI의 View와 완벽하게 분리되어 독립적으로 지어진다는 것이다.** 이는 독립적으로 개발하고 재사용과 테스트를 용이하게 한다. 

Counter의 로직을 캡슐화한 간단한 리듀서를 만들어 보자.
기능에 앞으로 더 흥미로운 행동을 추가할 것이지만 지금 당장은 간단하게 시작해보자. 

## Step1. 
```Swift
import ComposableArchitecture

```
- `CounterFeature.swift`라는 파일을 생성하기
- `ComposableArchitecture`를 import하기
>[!note]
> 라이브러리를 import하기 위해 SPM package나 Xcode project setting에 추가해야한다.

## Step2. 
```Swift
import ComposableArchitecture

@Reducer
struct CounterFeature {
	
}
```
- 새로운 구조체 `CounterFeature`를 추가하고 `Reducer()` 매크로 주석을 추가 
>[!note]
>`Reducer()` 매크로는 몇가지 일을 대신해주지만 지금 시점에서는 `CounterFeature`라는 타입에 Reducer 프로토콜을 채택해준다는 것만 알고 넘어가자. 


## Step3. 
```swift
@Reducer
struct CounterFeature {
	struct State {
		
	}

	enum Action {
		
	}
}
```
- Reducer를 채택하므로써 도메인 모델링 활동을 해야함
- `State` 타입과 `Action` 타입을 생성
- State: 기능이 작업을 하기 위해 필요한 상태를 지님, 구조체
- Action: 사용자가 수행하는 액션을 선언, 열거형

## Step4. 
```swift
@Reducer
struct CounterFeature {
	struct State {
		var count = 0
	}

	enum Action {
		case decrementButtonTapped
		case incrementButtonTapped
	}
}
```
- 간단한 카운터 기능을 목표로, 상태(State)는 간단한 정수로 구성
- 액션(Action)은 카운트를 증감하는 버튼을 누르는 것으로 구성
>[!tip]
>액션의 케이스 네이밍은 UI에서 사용자가 정확히 무슨 행위를 하는지로 지어주는게 최선
>`incrementButtonTapped` (O)
>`incrementCount` (X)

## Step5.
```swift
@Reducer
struct CounterFeature {
	struct State {
		var count = 0
	}

	enum Action {
		case decrementButtonTapped
		case incrementButtonTapped
	}

	var body: some ReducerOf<Self> {
		Reduce { state, action in 
			switch action {
			case .decrementButtonTapped:
			
			case .incrementButtonTapped:
			
			}
		}
	}
}
```
- 마지막으로 리듀서의 body를 상태와 액션을 포함하는 `Reduce`로 선언
- 이 리듀서는 사용자 액션이 주어졌을 때 State를 현재 값에서 다음 값으로 발전시킴
- 그 기능이 바깥 세상에서 실행하기 원하는 어떤 Effects를 반환
- 들어오는 액션에따라 스위칭되므로써 시작함
	- 어떤 로직을 수행해야하는지, 들어가고 나가는 어떤 상태가 제공되어야하는지를 결정
- mutation(변이)를 직접 수행할 수 있음
>[!note]
>리듀서는 body 프로퍼티로 실행되는데 후에 조합하고 싶은 리듀서를 리스팅한다. 
>지금은 간단한 리듀서로도 충분하고 그래서 하나의 리듀서만 존재 
>그러나 여러 리듀서를 함께 조합하는 것이 일반적이고 나중 튜토리얼에서 보여줄 것

# Step6. 
```swift
@Reducer
struct CounterFeature {
	struct State {
		var count = 0
	}

	enum Action {
		case decrementButtonTapped
		case incrementButtonTapped
	}

	var body: some ReducerOf<Self> {
		Reduce { state, action in 
			switch action {
			case .decrementButtonTapped:
				state.count -= 1
				return .none
			case .incrementButtonTapped:
				state.count += 1
				return .none
			}
		}
	}
}
```
- 더하거나 빼는 로직이 전부
- 반드시 `Effect`라는 값을 리턴해야 함
- 바깥 세상에서 수행되어야 하는 영향을 나타냄
- 이 케이스에선 아무것도 할 필요가 없음
- 아무것도 수행할 필요가 없다는 의미로  `.none`이라는 특별한 값을 반환함 

## 결론 
더 해볼 것이 많지만, View를 연결하는 것으로 넘어가려함 
- effect를 실행해 데이터를 시스템에 피드백하기
- 리듀서에 의존성을 사용하기
- 여러 리듀서를 조합하기 등등