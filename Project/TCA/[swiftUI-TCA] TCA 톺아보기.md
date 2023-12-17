
![[TCA 구조의 정수.png]]
### 값 타입과 참조 타입

- 구조체 vs 클래스
- 왜 TCA는 State 관리에 구조체를 쓸까?
    - 참조 타입의 속성에 대한 변형은 쉽지만 어디서 어떻게 변형이 일어나는지 추적이 어렵다.
    - 공유 상태를 일으킬 수 있어서 아예 구조체를 쓴다.

### TCA 개요

- 다양한 목적과 복잡성을 가진 애플리케이션을 설계할 때 도움이 된다.
- 단순한 ‘값 타입’을 활용해서 애플리케이션의 상태를 관리할 수 있다.
- 거대한 기능을 독립된 컴포넌트로 추출하거나 다시 합쳐서 기능을 구성할 수 있다.
    - 모듈화

### TCA의 명과암

- 장단이 명확함
    - 장: Scalable & Composable
        - 독립적인 유닛테스트
    - 단: 러닝 커브
        - TCA-UI를 추가로 공부

## 상태와 액션, Reducer

### State

- 우리가 아는 그 상태, TCA에서 사용할 수 있도록 약간의 Porting이 필요하다.

### Action

- reducer의 상태가 어떻게 변형되는지에 대한 모든 명세를 갖는 열거형
- 변형에 대한 명세는 열거형의 ‘case’로 정의 된다.
- case의 이름은

### Reducer

- 상태와 액션을 관리하는 어떤 녀석
- 느낌상 SwiftUI적으로 데이터를 관리하는 모습은 RxSwift를 닮았고, 뷰와 데이터처리를 분리하는 모습과 그로인해 테스트를 유용하게 할 수 있는 것은 MVVM을 닮았네.
    - 이 과정이 함수형 프로그래밍처럼 유기적으로 연결되어 있는 느낌
- `reducer._printChanges()` : 디버그할 때 찍음

### Store, View는 다 여기 있다.
- Store의 역할은 무엇일까?
- 모든 리듀서를 총괄해서 다루는 애

## Q&A
- @BindingState를 일일히 넣어줘야하나요?
    - 묶어서 쓸 수는 있는데 … 권장하지는 않음 → Reducer가 크다는 이야기
- 기능 단위별로 Reducer로 할 것인지? 뷰 별로 Reducer를 할 것인지?
    - 기능 별로, → 하나의 뷰에 여러 리듀서를 만들어도 됨

# 참조 링크

https://pointfreeco.github.io/swift-composable-architecture/main/documentation/composablearchitecture/

https://gist.github.com/pilgwon/ea05e2207ab68bdd1f49dff97b293b17

https://medium.com/riiid-teamblog-kr/riiid%EC%9D%98-swift-composable-architecture-231a665e5f47

https://medium.com/@Jager-yoo/swiftui-tca-store-vs-viewstore-%EB%AC%B4%EC%8A%A8-%EC%B0%A8%EC%9D%B4%EA%B0%80-%EC%9E%88%EC%9D%84%EA%B9%8C-40fce12e02f6

https://ridibooks.com/books/2773000087

https://axiomatic-fuschia-666.notion.site/Notion-PDF-QR-7509eb22e69049f48bc8d4e79161d635

2023.12.17 작성중..