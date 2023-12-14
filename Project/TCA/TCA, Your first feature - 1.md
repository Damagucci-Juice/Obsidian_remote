# [링크](https://pointfreeco.github.io/swift-composable-architecture/main/tutorials/composablearchitecture/01-01-yourfirstfeature)

# 학습 목표
기능의 행동과 논리를 수행하기 위해 `reducer`를 생성하는 법을 배우고, 그 기능을 SwiftUI의 View와 연결하는 법을 배운다. 

# Section 1, Reducer 생성
Composable Architecture에서 기능이 지어지는 기본 단위는 `Reducer()` 매크로와 `Reducer` 프로토콜이다. 이 프로토콜을 준수하는 것은 앱의 기능의 로직과 행동을 나타낸다. 이는 액션이 시스템에 전달되었을 때 현재 상태를 다음 상태로 진화하는 방법 바깥 세상과 시스템 내부로 데이터를 다시 피드백하는 소통 방식을 포함한다. 

그리고 가장 중요하게는 **기능의 핵심 로직과 행동이 SwiftUI의 View와 완벽하게 분리되어 독립적으로 지어진다는 것이다.** 이는 독립적으로 개발하고 재사용과 테스트를 용이하게 한다. 

Counter의 로직을 캡슐화한 간단한 리듀서를 만들어 보자.
기능에 앞으로 더 흥미로운 행동을 추가할 것이지만 지금 당장은 간단하게 시작해보자. 

## Step1. 
```task
- [ ] `CounterFeature.swift`라는 파일을 생성하기
- [ ] `ComposableArchitecture`를 import하기
```
>[!note]
> 라이브SPM package나 Xcode project setting에 추가해야한다.