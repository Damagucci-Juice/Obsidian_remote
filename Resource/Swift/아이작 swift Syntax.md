

---
아이작 Swift스러운 Syntax 모음 

[11. 6. 오전 10:40] 강희종(애플 아카데미) 아이작

[Swift스러운 Syntax] Day 1 - Enum(1)

 ---

## Enumeration - Q1) 왜 존재하는가?

Swift에서 Enumeration이 중요하다는 것은 이미 많이 들어서 익숙하실거에요. 이미 잘 사용하고 계신 분이 있다면 이 부분에 공감도 하실 것이라고 생각합니다. Swift의 Enum은 다른 언어보다 훨씬 풍부한 표현방식을 가지고 있고, 용법도 굉장히 다양합니다.

우선 깊이 있게 Enumeration에 대해서 공부해보기 이전에, 이 문법이 왜 존재하는지 알아보면 좋을 것 같아요.

각자가 생각하는 Enumeration의 존재 이유에 대해서 가볍게 댓글로 달아봅시다 🙂

간단한 질문이기 때문에 워밍업 차원에서 각자 생각하는 것들을 달아봐주세요 🔥 

비슷한 방식으로 **바로 내일 다음 질문**이 올라올 예정이에요~! 앞으로도 만관부 ![🙂](https://statics.teams.cdn.office.net/evergreen-assets/personal-expressions/v2/assets/emoticons/smile/default/20_f.png "Smile")

**참고 리소스**

[[Swift Docs] Enumerations](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/enumerations/#Matching-Enumeration-Values-with-a-Switch-Statement "https://docs.swift.org/swift-book/documentation/the-swift-programming-language/enumerations/#matching-enumeration-values-with-a-switch-statement") (문서 최상단 도입부만 먼저 읽어보세요)

like 4개 star 1개

Documentation

[11. 6. 오전 10:48] 강희종(애플 아카데미) 아이작

좀 더 쉽게는 "Swift는 어떤 문제를 해결하기 위해서 Enumeration이라는 Syntax를 제공하고 있나?" 라는 질문으로도 치환할 수 있을 것 같네요.

저는 Enum은 [경우의 수]를 표현하기 위한 문법이라고 생각해요~! 문자열을 표현하는 String, 정수를 표현하는 Int 처럼 어떤 개념이 특정한 경우의 수 안에서 표현을 해야할 때 Enum 타입을 사용한다고 이해하고 있습니다 🤠

[11. 6. 오후 8:51] Celan (이승준/오후)

저도 마찬가지로 열거형은 아이작 말씀하신 것처럼 '경우의 수'를 표현하는 문법이라 생각해요

제 경험을 더해보자면 연관값을 활용할 때, 열거형의 단순성이 빛을 발했습니다.

파라미터 타입이 열거형이라면, 호출하는 부분에서 원하는 case에 대한 값을 함께 전달할 수 있고, 각 상황에 대한 분기처리도 case 별로 분기할 수 있어서 코드의 단순함을 챙기는 데에 도움이 되었어요.

그래서 '코드의 단순함을 편리하게 향상'하기 위해 Swift의 열거형이 존재하는 거라고 생각했습니다~

like 3개 heart 1개 surprised 1개

[11. 7. 오전 7:05] 강희종(애플 아카데미) 아이작

말씀해주신 것처럼 [연관값](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/enumerations/#Associated-Values "https://docs.swift.org/swift-book/documentation/the-swift-programming-language/enumerations/#associated-values") 이라는 존재가 Swift의 Enum의 활용도를 엄청 높여주는 것 같아요.

실패할 수도 있는 결과를 다루는데 사용하는 [Result 타입](https://developer.apple.com/documentation/swift/result "https://developer.apple.com/documentation/swift/result")이나 [Optional](https://developer.apple.com/documentation/swift/optional/ "https://developer.apple.com/documentation/swift/optional/")역시 Enum으로 구현이 되어있죠. 

어제 적지는 못했는데, 공식문서에도 있는 내용으로, 무엇보다 Enum의 중요한 목적 중 하나는 Type Safe인 것 같아요. 

물론 경우의 수를 표현하는데 String, Int등 다른 타입들도 충분히 사용은 할 수는 있습니다.

하지만, 그 값을 사용하는 단계에서 if - else 문의 반복 혹은 switch 문을 사용한다고 했을때 그 경우의 수를 전부 다뤘다고 보장할 수가 없죠.

반면에, enum을 사용하면 switch문을 통해 모든 경우의 수에 대해서 분기가 가능하고, 이후 경우의 수가 추가되거나 삭제 되었을 때에도 컴파일러의 도움을 받고 코드를 수정해 나갈 수 있습니다. 또한 정의한 Enum이 [CaseIterable](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/enumerations/#Iterating-over-Enumeration-Cases "https://docs.swift.org/swift-book/documentation/the-swift-programming-language/enumerations/#iterating-over-enumeration-cases") 프로토콜을 채택하면 [.allCases](https://developer.apple.com/documentation/swift/caseiterable/allcases-swift.type.property "https://developer.apple.com/documentation/swift/caseiterable/allcases-swift.type.property")라는 static variable을 통해서 모든 경우의 수를 배열로 받아서 활용할 수도 있습니다. 

이만 정리하고, 다시 한번 멋진 답변 달아주신 Celan (이승준/오후) (게스트) 께 감사드립니다 ![🙂](https://statics.teams.cdn.office.net/evergreen-assets/personal-expressions/v2/assets/emoticons/smile/default/20_f.png "Smile") 

like 5개

Optional | Apple Developer Documentation

A type that represents either a wrapped value or the absence of a value.

[11. 7. 오후 9:43] Moro (송재훈/오후)

아이작이 말씀해주신 CaseIterable을 보고 생각난 건데, 본문과는 다른 주제기는 하지만 프로젝트에 커스텀 폰트, 컬러 같은 에셋 들을 추가해서 사용할 때에도 사용할 수 있어요.

저 같은 경우에는 아래처럼 프로젝트에서 사용하는 다양한 컬러들을 한 번에 확인하기 위해 사용하는데 더 개선할 수 있는지 다른 분들 의견이 궁금하네요. [11. 7. 오후 9:43] Moro (송재훈/오후)

  ![이미지](data:image/jpeg;base64,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Kf8AIpz/APXc10G56XRRRQAVKOlRVKOlADH+9+A/lTac/wB78B/Km0AFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAV4Z8cf+Q3p/wD1yavc68M+OP8AyHNP/wCuTUAeAyf61vqa6KP/AFSf7o/lXOyf6xvqa6KP/VJ/uj+Vc9foY1ug6iiiucwCiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAqlqn/HoP98f1q7VLVP+PQf74/rV0/iRUPiRR03/AI/4/wAf5V9O/BT/AJFOf/rua+YtN/4/4/x/lX078FP+RTn/AOu5rtOs9LooooAKlHSoqlHSgBj/AHvwH8qbTn+9+A/lTaACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAK8M+OP/Ic0/wD65NXudeGfHH/kN6f/ANcmoA8Bk/1rfU10Uf8Aqk/3R/Kudk/1jfU10Uf+qT/dH8q56/QxrdB1FFFc5gFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABVLVP+PQf74/rV2qWqf8AHoP98f1q6fxIqHxIo6b/AMf8f4/yr6d+Cn/Ipz/9dzXzFpv/AB/x/j/Kvp34Kf8AIpz/APXc12nWel0UUUAFSjpUVSjpQAx/vfgP5U2nP978B/Km0AFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAV4Z8cf+Q5p//XJq9zrwz44/8hvT/wDrk1AHgMn+tb6muij/ANUn+6P5Vzsn+tb6muij/wBUn+6P5Vz1+hjW6DqKKK5zAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACqWqf8eg/3x/WrtUtU/wCPQf74/rV0/iRUPiRR03/j/j/H+VfTvwU/5FOf/rua+YtN/wCP+P8AH+VfTvwU/wCRTn/67mu06z0uiiigAqUdKiqUdKAGP978B/Km05/vfgP5U2gAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACvDPjj/wAhzT/+uTV7nXhnxx/5Dmn/APXJqAPAZP8AWt9TXRR/6pP90fyrnZP9Y31NdFH/AKpP90fyrnr9DGt0HUUUVzmAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFUtU/49B/vj+tXapap/x6D/AHx/Wrp/EiofEijpv/H/AB/j/Kvp34Kf8inP/wBdzXzFpv8Ax/x/j/Kvp34Kf8inP/13Ndp1npdFFFABUo6VFUo6UAMf734D+VNpz/e/AfyptABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFeGfHH/AJDmn/8AXJq9zrwz44/8hvT/APrk1AHgMn+tb6muij/1Sf7o/lXOyf6xvqa6KP8A1Sf7o/lXPX6GNboOooornMAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKpap/wAeg/3x/WrtUtU/49B/vj+tXT+JFQ+JFHTf+P8Aj/H+VfTvwU/5FOf/AK7mvmLTf+P+P8f5V9O/BT/kU5/+u5rtOs9LooooAKlHSoqlHSgBj/e/AfyptOf734D+VNoAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigArwz44/wDIc0//AK5NXudeGfHH/kOaf/1yagDwGT/Wt9TXRR/6pP8AdH8q52T/AFjfU10Uf+qT/dH8q56/QxrdB1FFFc5gFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABVLVP8Aj0H++P61dqlqn/HoP98f1q6fxIqHxIo6b/x/x/j/ACr6d+Cn/Ipz/wDXc18xab/x/wAf4/yr6d+Cn/Ipz/8AXc12nWel0UUUAFSjpUVSjpQAx/vfgP5U2nP978B/Km0AFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAV4Z8cf+Q5p/8A1yavc68M+OP/ACHNP/65NQB4DJ/rG+proo/9Un+6P5Vzsn+tb6muij/1Sf7o/lXPX6GNboOooornMAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKpap/x6D/AHx/WrtUtU/49B/vj+tXT+JFQ+JFHTf+P+P8f5V9O/BT/kU5/wDrua+YtN/4/wCP8f5V9O/BT/kU5/8Arua7TrPS6KKKACpR0qKpR0oAY/3vwH8qbTn+9+A/lTaACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAK8M+OP/Ib0/8A65NXudeGfHH/AJDmn/8AXJqAPAZP9Y31NdFH/qk/3R/Kudk/1rfU10Uf+qT/AHR/Kuev0Ma3QdRRRXOYBRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAVS1T/j0H++P61dqlqn/HoP98f1q6fxIqHxIo6b/wAf8f4/yr6d+Cn/ACKc/wD13NfMWm/8f8f4/wAq+nfgp/yKc/8A13Ndp1npdFFFABUo6VFUo6UAMf734D+VNpz/AHvwH8qbQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABXhnxx/5Dmn/APXJq9zrwz44/wDIc0//AK5NQB4DJ/rG+proo/8AVJ/uj+Vc7J/rW+proo/9Un+6P5Vz1+hjW6DqKKK5zAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACqWqf8eg/3x/WrtUtU/49B/vj+tXT+JFQ+JFHTf8Aj/j/AB/lX078FP8AkU5/+u5r5i03/j/j/H+VfTvwU/5FOf8A67mu06z0uiiigAqUdKiqUdKAGP8Ae/AfyptOf734D+VNoAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigArwz44/8hvT/APrk1e514Z8cf+Q5p/8A1yagDwGT/Wt9TXRR/wCqT/dH8q52T/Wt9TXRR/6pP90fyrnr9DGt0HUUUVzmAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFF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FFFABRRRQAUUUUAFFFFABRRRQAUUUUAFeGfHH/kN6f8A9cmr3OvDPjj/AMhzT/8Ark1AHgMn+sb6muij/wBUn+6P5Vzsn+tb6muij/1Sf7o/lXPX6GNboOooornMAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKpap/x6D/fH9au1S1T/j0H++P61dP4kVD4kUdN/wCP+P8AH+VfTvwU/wCRTn/67mvmLTf+P+P8f5V9O/BT/kU5/wDrua7TrPS6KKKACpR0qKpR0oAY/wB78B/Km05/vfgP5U2gAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACvCPjs5TXNNx3ib+le714Z8cR/wATvT/+uRoA8Bf/AFjfU10Uf+qT/dH8q52T/Wv/ALxroo/9Un+6P5Vz1+hjW6DqKKK5zAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACqWqf8eg/3x/WrtUtU/49B/vj+tXT+JFQ+JGfYEreIRX058EGLeEJyf8Anu1fMum/8f0f4/yr6d+Cn/IpT/8AXc12nWel0UUUAFSjpUVSjpQAx/vfgP5U2nP978B/Km0AFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAV4f8AHJf+JtpresbV7hXjXxzhO7S58cfMtAHzpL/rn/3jXQQ8wRn/AGR/KsG5G26lHoxrbtTutIj/ALIrCvsjGtsiaiiiuYwCiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAqjqv/Hqv+/V6s/Vj+5jHq2aun8SKh8SKumf8f8AH+P8q+n/AILLjwfIfWdq+YtKGb0H0Umvqj4QwmLwNCxH+skZq7TrO9ooooAKlHSoqlHSgBj/AHvwH8qbTn+9+A/lTaACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAK8z+NVkZvC8FyBkwzDP0NemVz3jfS/7X8I6hagZbyyy/UUAfHGpJsvpP8A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)  

[11. 8. 오전 10:01] 강희종(애플 아카데미) 아이작

Moro (송재훈/오후) (게스트) 좋은 활용법이네요~! 저도 어떤 플젝에서는 이런 식으로 사용해본 경험도 있는 것 같고요~! 코드에 대해서 살짝 첨가 하자면, 만약 lightGreen, darkGreen 이 녹색에 대한 라이트모드 / 다크모드의 표현이라면, [Xcode의 Color Asset](https://developer.apple.com/documentation/uikit/appearance_customization/supporting_dark_mode_in_your_interface "https://developer.apple.com/documentation/uikit/appearance_customization/supporting_dark_mode_in_your_interface") 기능을 활용해서 표현해보는 것도 좋지 않을까 싶네용.

Supporting Dark Mode in Your Interface | Apple Developer Documentation

Update colors, images, and behaviors so that your app adapts automatically when Dark Mode is active.

[11. 8. 오전 11:01] Moro (송재훈/오후)

강희종(애플 아카데미) 아이작 말씀 감사합니다.  
코드는 사용자 정의에 대한 예시였는데 컬러를 확인하는 선택지 중 하나로 말씀하신 컬러 에셋이나 literal도 유용하다고 생각합니당

like 1개

[11. 8. 오전 11:02] Moro (송재훈/오후)

컬러 리터럴 - [https://mini-min-dev.tistory.com/m/80](https://mini-min-dev.tistory.com/m/80 "https://mini-min-dev.tistory.com/m/80")

heart 1개

---
[11. 7. 오전 9:56] 강희종(애플 아카데미) 아이작

## [Swift 스러운 Syntax] Day 2 콘텐츠 알림

[어제 올렸던 Day 1 콘텐츠](https://teams.microsoft.com/l/message/19:e086b8212ac24898baabe43610515175@thread.tacv2/1699234845722?tenantId=bff3e98c-5cca-455c-adc8-5fd24fc9908d&groupId=98602378-0e10-4295-bd1e-e63921e3b18c&parentMessageId=1699234845722&teamName=Cohort%202023%20-%20Apple%20Developer%20Academy%20%40%20POSTECH&channelName=All%20Code&createdTime=1699234845722 "https://teams.microsoft.com/l/message/19:e086b8212ac24898baabe43610515175@thread.tacv2/1699234845722?tenantid=bff3e98c-5cca-455c-adc8-5fd24fc9908d&groupid=98602378-0e10-4295-bd1e-e63921e3b18c&parentmessageid=1699234845722&teamname=cohort%202023%20-%20apple%20developer%20academy%20%40%20postech&channelname=all%20code&createdtime=1699234845722")에 이어서 오늘 Day 2콘텐츠도 올라왔습니다.

오늘은 내용을 알고 있는 분이라면 5분 안쪽으로 시도해볼 수 있는 일일 퀘스트도 있습니다. 

아래 내용 확인 해보시고, 관심 있는 분들은 참여 부탁드려요~!

[[Swift스러운 Syntax] Day 2 - Enum(2) *도전과제 있음*](https://teams.microsoft.com/l/message/19:e086b8212ac24898baabe43610515175@thread.tacv2/1699310209103?tenantId=bff3e98c-5cca-455c-adc8-5fd24fc9908d&groupId=98602378-0e10-4295-bd1e-e63921e3b18c&parentMessageId=1699310209103&teamName=Cohort%202023%20-%20Apple%20Developer%20Academy%20%40%20POSTECH&channelName=All%20Code&createdTime=1699310209103 "https://teams.microsoft.com/l/message/19:e086b8212ac24898baabe43610515175@thread.tacv2/1699310209103?tenantid=bff3e98c-5cca-455c-adc8-5fd24fc9908d&groupid=98602378-0e10-4295-bd1e-e63921e3b18c&parentmessageid=1699310209103&teamname=cohort%202023%20-%20apple%20developer%20academy%20%40%20postech&channelname=all%20code&createdtime=1699310209103")

star 1개

Join conversation

---
[11. 8. 오전 10:43] 강희종(애플 아카데미) 아이작

[Swift스러운 Syntax] Day 3 - Closure

(*) Enumeration 할 때랑 같은 플로우입니다.

## Closure - Q1) 왜 존재하는가?

Closure는 간단하게 말하자면 "이름 없는 함수" 입니다. 

함수처럼 일련의 코드를 묶어서 실행할 수 있게 해주지만, 따로 이름을 붙히지는 않는 문법이죠.

어떻게 쓰는지는 대부분 아실테고, 직접 작성 해보신 분들도 많을테지만, 왜? 이게 존재하는지는 많이 생각 안해보셨을거에요

[공식 문서](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/closures "https://docs.swift.org/swift-book/documentation/the-swift-programming-language/closures")를 한번 함께 천천히 보면서, 

- Closure가 풀고자 하는 문제

에 대해서 함께 알아보는 시간을 가지려고 합니다 🎓

아래 양식과 제가 작성한 첫 댓글을 참고해서 함께 클로져에 대해서 알아봅시다 🔥🔥🔥🔥

## 댓글 양식

---

1. 공식 문서를 보면서 "아! Closure는 이런 문제를 해결하기 위해 존재하는구나!"를 알게된 부분에 대한 인용
2. 인용구를 Closure의 존재 이유로 생각한 이

--- 

like 1개 star 1개

Documentation

[11. 8. 오전 10:43] 강희종(애플 아카데미) 아이작

1) 인용구

> _Closures_ are self-contained blocks of functionality that can be passed around and used in your code

2) 이유

함수를 작성하다 보면, 함수의 특정한 어떤 부분은 함수가 호출되는 시점의 상태에 따라서 함수의 사용자가 직접 작성하게 하고 싶은 경우가 있습니다. 이 인용구에 있는 것처럼, 특정 기능을 수행하는 코드 블록이 내 코드 안에서 돌아다니는 것이 가능하다면, 함수의 사용자가 함수 코드의 일부분을 직접 작성하는 것도 가능해지지 않을까 싶어서 선정 했습니다

[11. 8. 오전 11:07] Celan (이승준/오후)

1) 인용구

> functions and closures are _reference types._

2) 이유

클로저는 그 자체로 참조 타입이기 때문에 그 이름을 숨길 수 있는 형태로, 서로 다른 변수와 상수에 타입 명시할 수 있다는 장점이 있습니다. 그래서 `var fetch: () async throws -> Data` 의 형태로 코드를 작성하면, 추후에 `fetch` 변수에 새로운 클로저를 할당할 수 있습니다. 특히 테스트 코드를 작성할 때 이렇게 변수에 저장된 클로저를 새롭게 할당할 수 있게 되면서, 서버에 의존적이지 않은 방식으로 테스트를 진행할 수도 있습니다. 테스트용 로직을 릴리즈용 로직에 덮어씌워서 테스트할 수 있기 때문입니다. 중괄호 내부의 로직을 각 변수에 할당할 수 있고, 때에 따라 변경할 수 있어서 유연한 프로그래밍 설계에 큰 도움이 되어서 Closure의 존재 이유가 될 수 있다고 생각했습니다. 

heart 1개 surprised 1개

[11. 8. 오후 1:40] Eren (문희찬/오후)

1) 인용구  
> A closure is said to escape a function when the closure is passed as an argument to the function...  
2) 이유  
클로저는 비동기 작업을 수행하는 함수에서 유용하다고 생각합니다. 대표적인 예시로, 서버에서 이미지를 불러오는 상황이 있습니다. 서버에서 이미지를 불러오는건 얼마나 오랜 시간이 걸릴지 예측할 수 없습니다. 네트워크 상황, 서버 트래픽 상황, 이미지의 크기 등등 데이터를 불러오는 시간을 결정짓는 요인들은 매우 많기 때문입니다. 그럼 클라이언트는 언제 올지 모르는 이미지를 어떻게 생성해줄 수 있을까요?   
escaping closure는 함수의 파라미터로 함수를 전달하는 문법입니다. 이미지를 가진 객체(이하 A객체)는 네트워크 작업을 하는 객체(이하 B객체)에게 함수를 제공할 수 있습니다. B객체는 네트워크 작업이 끝나는 시점을 알고 있습니다. 그럼 A객체로부터 받은 함수(escaping closure)를 실행시켜서 이미지를 생성하는 함수를 실행시킬 수 있습니다  
클로저는 비동기 작업에서 강력한 힘을 발휘한다고 생각합니다.   
혹시 틀린 내용이 있다면 댓글부탁드려요 감사합니다

heart 1개

[11. 8. 오후 4:25] Cliff (윤범태/오후)

[11. 8. 오후 4:29] Cliff (윤범태/오후)

인용구  
  
`_Closures_ are self-contained blocks of functionality that can be passed around and used in your code. Closures in Swift are similar (...) to lambdas in other programming languages.`

`Closures can capture and store references to any constants and variables from the context in which they’re defined.` 

이유

- 함수를 파라미터 형태로 전달할 수 있음 (고차함수)
- 클로저는 현재 컨텍스트의 상수 및 변수를 캡처할 수 있음

heart 1개

[11. 8. 오후 6:32] Sunday (이선호/오후)

인용구

Closures can capture and store references to any constants and variables from the context in which they’re defined. This is known as _closing over_ those constants and variables. Swift handles all of the memory management of capturing for you.

이유 - 클로저안에서 변수나 상수를 캡쳐해서 사용할수 있게 하고 메모리 관리에서도 자동으로 swift가 해주어 클로저안에서 다양한 처리를 할수 있다.

heart 1개

[11. 9. 오전 3:14] Lets (고석환/오후)

> The closure can then refer to and modify the values of those constants and variables from within its body, even if the original scope that defined the constants and variables no longer exists.

이유: closure의 장점은 주위의 상수, 변수등의 컨텍스트를 전부 다 캡쳐해서 특정 시점을 기록함에 있다고 생각합니다. 보통의 경우 하나의 스코프가 끝나면 메모리에서 자동으로 삭제되기 때문에 특정 시점을 사용하기가 어렵지만, 클로져의 캡쳐링하는 특성을 이용해서 가능하게 할 수 있습니다.

heart 1개

---
[11. 9. 오전 10:08] 강희종(애플 아카데미) 아이작

[Swift스러운 Syntax] Day 4 - Closure *일일퀘스트*

오늘 콘텐츠는 **매우 간단**

Array의 [reduce(_:_:)](https://developer.apple.com/documentation/swift/array/reduce(_:_:) "https://developer.apple.com/documentation/swift/array/reduce(_:_:)")를 **직접 구현**해보세요

Generic, Extension 키워드를 목적으로 하는 것은 아니고, Closure의 이해와 구현을 확인하기 위해서 하는 활동이어서

정수 타입의 배열에만 reduce를 구현 해볼거고, 아래 코드의 주석 설명에 맞춰서 한번 시도해보시면 됩니다.

욕심이 더 난다면, Generic을 적용 한다던가, 다른 재미있는 기능을 만들어보셔도 좋습니다~! 

star 1개

reduce(_:_:) | Apple Developer Documentation

Returns the result of combining the elements of the sequence using the given closure.

[11. 9. 오전 11:38] Celan (이승준/오후)

처음엔 `MyElement` 제너릭 타입을 `Equatable`로 제한하려 했는데, 과제가 정수타입 배열에 국한되어 있어서 이렇게 구현해봤습니다~

like 2개 surprised 1개

[11. 9. 오후 1:07] Eren (문희찬/오후)

이론으로만 생각해봤는데 직접 구현해보니까 재미있네요

추가) 파일말고 코드 올리는법 아시는분.. 😂

[custom_reduce_example.swift](https://postechackr.sharepoint.com/sites/Cohort2023-AppleDeveloperAcademyPOSTECH/Shared Documents/All Code/custom_reduce_example.swift)

[11. 9. 오후 1:14] Eren (문희찬/오후)

   ```
 
```

이론으로만 생각해봤는데 직접 구현해보니까 재미있네요

~~추가) 파일말고 코드로 올리는법 아시는분 😂~~

heart 1개 like 1개

[11. 9. 오후 1:16] Jeckmu (이재원/오후)

[11. 9. 오후 3:28] Kozi (한지석/오후)

기존에 reduce가 구현되어 있는 방식처럼 구현해보려 시도해봤습니다...! 내부 로직은 다를 수 있으나 Array에서 사용할 때 기존 reduce랑 같은 방법으로 사용할 수 있도록 최대한 노력해봤습니다.🥲

like 2개

[11. 9. 오후 5:15] Cliff (윤범태/오후)

 

heart 1개 like 1개

[11. 10. 오전 3:51] Lets (고석환/오후)

혼자 힘으로는 풀지 못해서 이것저것 찾아봤는데 이런 문법공부가 정말 배워가는게 많네요! 어렵고 재밌습니다 ![🙂](https://statics.teams.cdn.office.net/evergreen-assets/personal-expressions/v2/assets/emoticons/smile/default/20_f.png "웃는 표정") 

like 3개

[11. 10. 오후 4:26] Sunday (이선호/오후)

새롭게 생각해볼 계기가 되었네요 늦었지만 올립니다. ㅎ

---

[11. 10. 오전 10:16] 강희종(애플 아카데미) 아이작

[Swift스러운 Syntax] Day 5 - Protocol

 ---

## Protocol - GQ) 왜 존재하는가?

살짝 주목도도 떨어지고, 다른 이유도 있는 듯 해서

지금은 내려가있기는 하지만 WWDC 2015에 Protocol Oriented Programming 이라는 제목으로

프로그래밍 패러다임을 소개할 만큼 Swift는 Protocol을 중요한 언어의 요소로 생각하는 것 같아요.

강한 타입을 가지는 Swift에서 Protocol과 Generic은 코드의 유연성과 재사용성을 높여주는 중요한 역할을 수행합니다.

저번과 마찬가지로 공식 문서를 읽으면서 댓글 양식에 맞춰서 댓글도 달고, 다른 사람 댓글에 코멘트도 남기고 해봅시다~!

**공식문서 링크**

[[Swift Docs] Protocol](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/protocols/ "https://docs.swift.org/swift-book/documentation/the-swift-programming-language/protocols/")

## 댓글 양식

---

1. 공식 문서를 보면서 "아! Closure는 이런 문제를 해결하기 위해 존재하는구나!"를 알게된 부분에 대한 인용
2. 인용구를 Closure의 존재 이유로 생각한 이유

--- 

star 1개

Documentation

[11. 10. 오전 10:27] Celan (이승준/오후)

> The protocol can then be _adopted_ by a class, structure, or enumeration to provide an actual implementation of those requirements.

 - 이유

OOP를 기반으로 하던 프로그래밍 패러다임에서 다형성은 클래스 객체에만 해당되었던 것으로 알고 있습니다. 그러나 다이아몬드 문제로 인해 하나의 자식 객체가 하나의 부모 객체만 상속하는 것이 다형성에 엄밀히 부합하는지 개인적으로는 의문이 들었습니다. 참조 타입의 사용을 강제하던 상속 패러다임에서 벗어나 프로토콜이 등장했고 값 타입도 이제 기본적인 요구 사항에 힘입어 '특정 프로토콜을 채택하는 타입이 어떠한 메소드나 속성을 가질 것을 보장' 받을 수 있게 되었습니다. 제너릭과의 궁합으로, 추상화된(불특정된) 타입이 어떠한 메소드, 속성을 가질 것으로 가정하여 코드의 설계 단계에서 재사용성은 높이고 타입 간 직접적인 간섭은 줄일 수 있게 되었다고 이해하고 있습니다.

그래서 인용구에서 말하듯 값 타입이 프로토콜을 채택할 수 있게 된 것이 굉장히 큰 이유라고 생각합니다.

like 5개

[11. 10. 오전 10:28] 강희종(애플 아카데미) 아이작

크으.. 멋집니다 👍

[11. 10. 오후 1:18] Eren (문희찬/오후)

> A _protocol_ defines a blueprint of methods, properties, and other requirements that suit a particular task or piece of functionality.

Protocol의 이점을 객체지향의 3가지 원칙에 근거해서 설명할 수 있습니다.  
1) OCP   
개발을 진행하다 보면 예상하지 못한 상황이 빈번히 발생합니다. 예를 들어, 우리의 앱이 mysql 데이터베이스를 사용했지만 스펙 상의 이유로 oracle 데이터베이스로 변경해야할 수 있습니다. 미리 데이터베이스에 대한 프로토콜을 만들었다면, 우리는 oracle에 대한 구현클래스(이하 레포지토리)를 만들어 주기만 하면 됩니다.  
2) ISP  
일반적으로 레포지토리는 CRUD 함수를 제공합니다. 하지만 레포지토리를 참조하는 모든 클래스가 CRUD 기능을 다 사용하지 않을 수 있습니다. 어떤 뷰는 create하는 함수만 필요하다고 생각해봅시다. 그럼 뷰는 레포지토리를 의존하지만 필요하지 않는 remove, update, delete 함수까지 사용할 수 있게됩니다. 우리는 협업을 하고 팀원들의 코드를 전부 알 수 없습니다. 혹여나 의도하지 않은 remove함수를 호출한다면 밤을 새서 디버깅을 해야하는 참사가 일어날 지도 모릅니다.   
이 때, 우리는 인터페이스의 분리를 통해 코드의 안정성을 높여줄 수 있습니다. create 함수만 제공하는 인터페이스를 만들어 레포지토리의 선택적 사용을 알려줍니다. 그럼 유지보수하는 팀원(혹은 나)의 잘못된 사용을 미연에 방지할 수 있습니다  
3) DIP  
앞서 OCP에서 설명한 상황에서 도움을 얻을 수 있습니다. 어떤 뷰가 mysql 구현클래스를 참조한다면, 그 뷰는 구현클래스에 대한 강한 의존도를 가집니다. 하지만 그 뷰는 구현클래스가 어떤 방식으로 동작하는지 궁금하지 않습니다. 뷰는 그저 데이터베이스에서 얻은 값을 화면에 보여주는 역할만 수행하면 됩니다. 그래서 나온 이론이 DIP입니다. 객체는 구현 클래스를 의존하는 것이 아닌 추상클래스를 의존하면 보다 낮은 의존도을 가지게 됩니다  
프로토콜은 객체지향의 꽃이라고 생각합니다. 프로토콜를 사용하여 유지보수가 가능하고 확장가능한 코드를 작성하면 좋겠습니다.   
혹시 틀린 내용이 있다면 커맨트 부탁드려요 감사합니다

like 4개

[11. 10. 오후 2:00] 강희종(애플 아카데미) 아이작

와... 오늘 댓글 퀄리티 미쳐따....

[11. 10. 오후 5:37] Cliff (윤범태/오후)

A _protocol_ defines a blueprint of methods, properties, and other requirements that suit a particular task or piece of functionality. The protocol can then be _adopted_ by a class, structure, or enumeration to provide an actual implementation of those requirements. Any type that satisfies the requirements of a protocol is said to _conform_ to that protocol.

(...)

_Delegation_ is a design pattern that enables a class or structure to hand off (or _delegate_) some of its responsibilities to an instance of another type. This design pattern is implemented by defining a protocol that encapsulates the delegated responsibilities, such that a conforming type (known as a delegate) is guaranteed to provide the functionality that has been delegated. Delegation can be used to respond to a particular action, or to retrieve data from an external source without needing to know the underlying type of that source.

- 청사진 - 구조체, 클래스 등이 어떻게 구성되어야 할 지 방향을 제시하고 그것에 따르도록 강제합니다.
- 위임 - 해당 오브젝트가 해야할 일을 그 오브젝트 내부에 정하는 것이 아니고 다른 오브젝트에서 정하도록 위임할 수 있습니다. 그 위임할 내용을 프로토콜을 통해 정의할 수 있습니다. 어떤 프로토콜을 준수하는 오브젝트는 위임자의 자격이 주어집니다. Swift로 이루어진 프레임워크(UIKit 등)의 많은 부분에서 위임 패턴을 사용하므로 프로토콜은 Swift에 있어 필수불가결합니다.

like 3개

[11. 13. 오전 9:58] 강희종(애플 아카데미) 아이작

필수 불가결!!

---
[11. 8. 오전 10:17] 강희종(애플 아카데미) 아이작

[Swift스러운 Syntax] 특별편 - Enum 어디까지 써봤나?? 경진대회

어제까지 2일에 걸쳐서 Enum에 대해서 빠르게 훑어보는 시간을 가졌는데요, 

서두에 Enum은 Swift에서 굉장히 활용도가 높다고 이야기를 했었어요. 

이미 다룬 키워드들에서도 Swift가 다른 언어들보다 Enum에 진심이라는 것을 알 수 있었지만, 

Enum 안에서 Property, Method, Initialization을 추가로 구현할 수도 있고, 

Extension을 사용한 기능 확장, Protocol 채택을 통한 여러 활용이 가능한 등 정말 많은 활용법을 가지고 있습니다.

**그래서**

이 포스팅에서는 각자 챌린지, 개인 플젝 등 공개할 수 있는 레포지토리에서 

## [Enum 이렇게 까지 써봤다!!]

를 공유 해보는 시간을 가져보면 어떨까 싶어요~!

---

## ** 즐기는 방법 **

1. 내가 생각해도 Enum의 의도를 잘 살려서 구현했다! 싶은 코드의 GitHub line link를 복붙합니다.
2. 구현에 대한 간단한 설명을 남깁니다
3. 게시물의 댓글 알람을 켜고 다른 사람들의 코드를 구경하면 토론해봅시다
4. 따로 종료시간을 생각하고 있지는 않지만, 오늘 / 내일 2일에 걸쳐서 한번 달려봅시다 ![🙂](https://statics.teams.cdn.office.net/evergreen-assets/personal-expressions/v2/assets/emoticons/smile/default/20_f.png "Smile")  

star 1개

---
[11. 13. 오전 10:17] 강희종(애플 아카데미) 아이작

[Swift스러운 Syntax] Day6 - Generic

프로토콜은 단독으로 퀘스트를 만들기 보다는 Generic까지 섞어서 넣는게 더 의미가 있을 것 같아서, 우선 오늘 토픽은 Generic으로 잡았습니다.

[P.S.] 내일은 Protocol + Generic으로 일일 퀘스트 나갑니다~!

## [ Generic ] 

## GQ-1) 어떤 문제를 풀기 위해서 존재하는가?

## GQ-2) 어떤 용례들이 있을까?

### 공식문서 링크

[[Swift Docs] Generic](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/generics#The-Problem-That-Generics-Solve "https://docs.swift.org/swift-book/documentation/the-swift-programming-language/generics#the-problem-that-generics-solve")

## 댓글 양식

---

1. [GA for GQ1] Generic은 어떤 문제를 풀기 위한것일까 나의 문장으로 정리
2. [GA for GQ2] 공식문서의 섹션 중에서, 내가 제일 익숙하지 않은, 혹은 직접 코드를 작성해보지 않은 용법 1개(이상)에 대해서 간단한 설명과 함께 코드로 직접 작성 해봅시다.

--- 

like 2개 star 1개

[11. 13. 오후 12:02] Celan (이승준/오후)

 1. `Generic`은 타입 추상화를 지원하기 위해 등장한 개념으로, 강타입 언어를 지향하는 스위프트에서 동일한 비즈니스 로직을 여러 차례 다른 타입으로 구현해야 했던 번거로움을 해결하는 데에 큰 도움을 주었습니다. 여러 타입들을 제너릭한 타입으로 받아옴으로써 반복되는 연산을 줄이고(DRY), 코드의 공간 복잡도를 줄이는 데에도 큰 역할을 했습니다.

 2. 그러나.. 이렇게 추상화되는 타입들은 프로토콜이나 타입으로 제약되지 않으면 '내용'을 갖지 않기 때문에 이러한 **타입 제한**의 도움 없이는 기능의 절반도 제대로 활용하지 못하곤 했습니다. 추상화하면 그 내용이 아무것도 남지 않기 때문이었죠. 제가 생각하는 제너릭의 활용도는 대체로 프로토콜이 `Generic`을 갖고, 내부에서 선언되는 `associatedtype`에 내부 로직과 메소드를 추가적으로 지원하는 방향이었습니다. 이번 프로젝트에서 사용한 코드의 일부를 첨해보겠습니당   

heart 2개

[11. 13. 오후 3:46] Cliff (윤범태/오후)

**Generic은 어떤 문제를 풀기 위한것일까 나의 문장으로 정리**

- 데이터 형식에 의존하지 않고, 하나의 값이 여러 다른 데이터 타입들을 가질 수 있는 기술에 중점을 두어 재사용성을 높일 수 있는 프로그래밍 방식입니다.

예) 배열에서 swap 기능은 단순히 두 변수의 값을 바꾸는 것 뿐으로 거의 대부분의 타입에서 사용 가능한 기능입니다. 예를 들어 Int, String, Double 및 기타 등등이 전부 swap 가능한 타입들입니다. 각각에 대한 swap 함수를 구현하면 다음과 같습니다.

지금은 타입이 3개이지만, 만약 다른 타입들에 대해서도 swap을 하고 싶다면 이와 같은 방식으로 함수들을 타입 개수만큼 무한히 작성해야 합니다. 그리고 함수의 내용을 보면 다른 것은 타입뿐이고, 교환 로직은 전부 동일하다는 것을 알 수 있습니다. 심지어 코드도 똑같습니다.

이런 문제를 해결하기 위해 등장한 것이 제네릭입니다. 제네릭 함수는 함수에 가상의 타입을 부여한 뒤, 이것을 파라미터로 받고 처리합니다.

**공식문서의 섹션 중에서, 내가 제일 익숙하지 않은, 혹은 직접 코드를 작성해보지 않은 용법 1개(이상)에 대해서 간단한 설명과 함께 코드로 직접 작성 해봅시다.**

아래 SuffixableContainer 프로토콜은 Container의 기능에 컨테이너 끝에서 주어진 수의 요소를 반환하여 Suffix 유형의 인스턴스에 저장하는 suffix(_:) 메서드를 추가한 프로토콜입니다.


- `SuffixableContainer`는 `Container` 프로토콜을 준수합니다.
- 연관 타입 `Suffix`
    - `Suffix`는 `SuffixableContainer`를 준수합니다.
    - `SuffixableContainer` `Suffix` 타입의 `Item`은 컨테이너의 연관 타입인 `Item`과 동일한 타입이어야 합니다.
        - 이러한 조건은 `where`절로 지정합니다.

### **where Suffix.Item == Item의 의미**   

heart 3개

---

- 강희종(애플 아카데미) 아이작11. 14. 오전 10:14편집됨## [Swift스러운 Syntax] Day7 - Protocol & Generic 일일퀘스트 🔫
    
    ## [ 클라이언트의 요구사항 ]
    
    - 앱 안에서 포스팅 기능이 있어요
    - 하나의 포스팅에는 [헤더] [메인] [푸터] 영역의 콘텐츠가 있어요
    
    - 각각의 영역에는 [텍스트], [이미지], [웹페이지 링크]가 블록의 형태로 들어갑니다.
    - 헤더에는 하나의 블록, 푸터에는 여러 개의 블록이 들어갈 수 있어요.
    
    ## [기술적으로 구현해야 하는 부분]
    
    - 위에 있는 UI를 참고해서 SwiftUI로 PostCard 를 구현해주세요
    
    ## [당부의 말]
    
    - 스펙이 상세하지 않아요, 자의로 해석하시고 편의에 맞춰 구현해주세요 ㅋㅋ
    - 정답은 없습니다. 최대한 Protocol과 Generic을 사용해서 💃 FANCY 하게 💃 코딩 해주세요 
    
    ---
    
    저번에 한 것처럼 댓글의 코드 블럭을 사용하셔서 올려주시면 됩니다~! 
    
    이번 일일퀘스트는 아마 시간을 좀 여유있게 가지고 구현해보면 더 재밌을 것 같아서
    
    **11월 14일 ~ 11월 15일 양일에 걸쳐서** 진행해보도록 할게요~!
    
    ~~(시간을 가지는게 더 좋을것 같아서에요. 절. 대. 귀찮아서가 아닙니다 !!!!!!)~~
    
    ## ( 첨 언 )
    
    만약 코드를 한번 올리고나서 뭔가 맘에 안들어서 다시 올리고자 한다면...?
    
    기존 댓글을 수정하지 마시고, 새로운 댓글로 작성하신 뒤에 수정된 부분을 코멘트로 다시 남겨주세요 ![🙂](https://statics.teams.cdn.office.net/evergreen-assets/personal-expressions/v2/assets/emoticons/smile/default/60_anim_f.png?v=v82) ![❤️](https://statics.teams.cdn.office.net/evergreen-assets/personal-expressions/v2/assets/emoticons/heart/default/50_f.png?v=v34)하트 반응 3개.3
- Cliff (윤범태/오후) (게스트)11. 14. 오후 6:03편집됨
    
    #### PostCard
    
     
    
    #### Swift
    
    ```
    import SwiftUI/// 생성자로 String 파라미터 한 개를 갖는 인터페이스입니다.protocol HasStringParameter {    init(_ stringParameter: String)}extension Text: HasStringParameter {}extension Image: HasStringParameter {    init(_ stringParameter: String) {        self.init(stringParameter, bundle: nil)    }}/// 링크 주소를 입력하면 하이퍼링크를 생성하는 뷰입니다.struct WebLinkView: View, HasStringParameter {    var url: URL    var descriptionValue: String?        init(_ stringParameter: String) {        url = .init(string: stringParameter)!    }        init(stringParameter: String, descriptionValue: String? = nil) {        url = .init(string: stringParameter)!        self.descriptionValue = descriptionValue    }        var body: some View {        Text("[\(descriptionValue ?? url.absoluteString)](\(url.absoluteString))")    }}/// 블록뷰struct BlockView<InnerView: View>: View where InnerView: HasStringParameter {    let innerView: InnerView        init(_ stringParameter: String) {        innerView = InnerView(stringParameter)    }        var body: some View {        innerView    }        @ViewBuilder func resizable() -> some View {        if let innerView = innerView as? Image {            innerView.resizable()        } else {            innerView        }            }}/// 포스트카드 모델struct PostCard: Codable {    var title: String    var imageName: String    var urlString: String}/// 포스트카드 표시 (헤더, 본문, 푸터)struct PostCardView: View {    enum Mode {        case header, body, footer    }        @State var mode: Mode = .body    @State var postCard: PostCard?        var body: some View {        switch mode {        case .header:            Text("Header")        case .body:            if let postCard  {                VStack {                    BlockView<Text>(postCard.title)                        .font(.title)                        .bold()                    BlockView<Image>(postCard.imageName)                        .resizable()                        .scaledToFit()                        .frame(width: 300)                    BlockView<WebLinkView>(postCard.urlString)                }            } else {                Text("Error")            }        case .footer:            Text("Footer")        }                Divider()    }}/// 메인 뷰struct ContentView: View {    @State var cards: [PostCard] = [        .init(title: "홍길동", imageName: "sample1", urlString: "https://google.com"),        .init(title: "임꺽정", imageName: "sample2", urlString: "https://apple.com"),        .init(title: "이생원", imageName: "sample3", urlString: "https://microsoft.com"),    ]        var body: some View {        ScrollView {            VStack {                PostCardView(mode: .header)                ForEach(cards, id: \.title) { card in                    PostCardView(postCard: card)                }                PostCardView(mode: .footer)            }        }    }}
    ```
    
    확장(119 줄)
    
    부족한 부분이 있으면 지속적으로 업그레이드 하겠습니다!
    
    ![❤️](https://statics.teams.cdn.office.net/evergreen-assets/personal-expressions/v2/assets/emoticons/heart/default/50_f.png?v=v34)하트 반응 2개.2

---

## [Swift스러운 Syntax] 업데이트 소식

새로운 포스팅이 등록 되었어요 ![🙂](https://statics.teams.cdn.office.net/evergreen-assets/personal-expressions/v2/assets/emoticons/smile/default/60_f.png?v=v82)

오늘은 **일일 퀘스트** 하는 날입니다 🎉

본문에도 적었지만, 이번 퀘스트는 2일동안 진행됩니다~!

[[Swift스러운 Syntax] Day7 - Protocol & Generic 일일퀘스트 🔫](https://teams.microsoft.com/l/message/19:e086b8212ac24898baabe43610515175@thread.tacv2/1699924454073?tenantId=bff3e98c-5cca-455c-adc8-5fd24fc9908d&groupId=98602378-0e10-4295-bd1e-e63921e3b18c&parentMessageId=1699924454073&teamName=Cohort%202023%20-%20Apple%20Developer%20Academy%20%40%20POSTECH&channelName=All%20Code&createdTime=1699924454073 "https://teams.microsoft.com/l/message/19:e086b8212ac24898baabe43610515175@thread.tacv2/1699924454073?tenantid=bff3e98c-5cca-455c-adc8-5fd24fc9908d&groupid=98602378-0e10-4295-bd1e-e63921e3b18c&parentmessageid=1699924454073&teamname=cohort%202023%20-%20apple%20developer%20academy%20%40%20postech&channelname=all%20code&createdtime=1699924454073")


[목요일 오후 10:33] Cliff (윤범태/오후)

1. Identify  
    a. Explicit identity: 뷰 인스턴스가 임의로 생성되거나 데이터에서 파생된 성질에 의해 구분됨.  
      - 메모리 포인터로 구분: UIKit  
      - 고유값(ID)로 구분: SwiftUI  
      - SwiftUI에서 사용되는 예) ForEach에서 데이터의 고유성을 파악할 때, 스크롤뷰의 scrollToTop에서 이동 대상이 되는 뷰의 위치를 찾을 때  
    b. Structural Identity: 상대적 위치에 의해 뷰 인스턴스의 고유성이 부여됨  
      - 예) if~else문, switch문을 통해 분기별로 뷰가 표시되는 경우  
      - 고유성이 부여되므로 똑같은 뷰 구조체에서 생성되었더라도 별도의 인스턴스로 취급됨  
    
2. Lifetime  
    - 뷰의 값(value)의 수명은 짦음: 변경될 때마다 해당 뷰 인스턴스가 파괴되고 재생성됨  
    - 뷰의 유효기간(lifetime)은 고유성(identity)의 수명과 같음: 고유성이 길어지면 뷰가 살아있는 시간도 길어지고, 반대도 마찬가지  
    - 상태의 영속성(state of persistence)은 뷰의 유효기간과 직결됨  
    - **따라서 뷰의 유효기간을 길게 유지하려면 오브젝트들이 안정된 고유성(stable identity)을 가져야 함**  
      - 매번 랜덤으로 생성되는 아이디는 부적합: 생성될때마다 다른 아이디를 지니므로 별개의 개체로 인식  
      - 배열의 인덱스(indices)를 아이디로 사용하는 것도 비권장: 삽입, 삭제시 문제 발생 소지 있음  
      - 데이터베이스에서 부여된 ID 등 고유성을 지닐 수 있는 아이디를 권장  
    
3. Dependencies  
    - Dependency Graph  
      - 뷰의 구조가 겉으로는 트리처럼 보여도, 각종 상태 프로퍼티(=>디펜던시)들의 존재로 인해 실질적으로 그래프의 형태를 띄고 있음  
      - 그래프는 잘 관리하면 효율적인 데이터 핸들링이 이루어질 수 있다는 장점이 있음. 그러나 잘못 관리된 경우 부작용이 증가  
    - 디펜던시의 종류: Binding, Environment, State, StateObject, ObservedObject, EnvironmentObject 등  
    - 디펜던시를 잘 통제하는 법: **안정된 Identity**를 부여하는 것 (고유성이 길게 유지되면 뷰의 유효기간이 늘어나며, 뷰를 재생성하는 자원을 아낄 수 있으므로 성능 향상에도 도움이 되고, 텀이 긴 영속성 관리로 공간 관리(메모리 등)도 효율적으로 이루어짐

rock 4개

[목요일 오후 10:52] 강희종(애플 아카데미) 아이작

드디어!!!

[금요일 오후 2:01] Kozi (한지석/오후)

# **Identity**

- SwiftUI가 앱의 여러 업데이트에서 요소를 동일하거나 별개로 인식하는 방법
- View Identity : 뷰의 정체성
    - 별개의 UI 요소를 나타내는 뷰는 항상 다른 ID를 가짐(조건문으로 나뉘는 경우)
    - 같은 ID일 경우 = 트랜지션, 다른 ID일 경우 뷰가 제거되고 삽입된다.
- SwiftUI에서 표현되는 ID
    - Explicit identity(명시적 ID) : 맞춤형 또는 데이터 기반 식별자를 사용
        - 강력하고 유연하지만, 어딘가에서 모든 이름을 추적해야 함
        - UIKit, AppKit에서의 ID가 대표적
        - SwiftUI 뷰는 일반적으로 클래스 대신 구조체로 표현되는 값 유형, SwiftUI는 포인터를 사용하지 않음. (`SwiftUI Essentials` 확인하기)
        - SwiftUI가 해당 뷰에 대한 지속적인 ID로 사용할 수 있는 정식 참조가 없음  
            → 다른 형태의 명시적 ID 사용
        - .id(_:) modifier는 사용자 정의 식별자를 사용하여 뷰를 명시적으로 식별하는 방법을 제공
        - 모든 뷰를 명시적으로 식별할 필요가 없고, 헤더 텍스트와 같이 코드의 다른 곳에서 참조해야 하는 뷰만 식별할 수 있음
        - 물론 명시적이지 않았던 뷰들도 모두 Identity를 가지고 있음  
            
    - Structural identity(구조적 ID)
        - 뷰 계층 구조에서의 유형과 위치에 따라 뷰를 구별. 뷰 계층 구조를 사용하여 뷰에 대한 암시적 ID를 생성  
            
        - 조건문의 구조는 각 뷰를 식별하는 명확한 방법을 제공함
        - SwiftUI는 뷰를 볼 때 Generic type으로 봄. _ConditionalContent 뷰로 변환됨
        - 이러한 translation은 ViewBuilder에 의해 제공됨(암시적)
        - 이 제네릭타입은 배후에서 암시적이고 안정적인 ID가 할당될 수 있도록 함
        - SwiftUI는 조건문이 고유한 ID를 가진 다른 뷰를 나타낸다는 것을 이해함
        - 일관된 ID로 단일 뷰를 수정할 경우 부드럽게 전환됨  
            
        - 일반적으로 SwiftUI는 2번째 방식을 권장함
        - **기본적으로 identity를 유지하고, 보다 유연한 전환을 제공하도록 노력하자**  
            → 뷰의 수명과 상태를 보존하는 데 도움이 됨

## AnyView

- 가능하면 AnyView를 피하자. 가독성이 떨어지고 이해하기 어려움
- AnyView는 컴파일러에서 정적 타입 정보를 숨기기에 유용한 에러 및 경고가 코드에 표시되지 않는 경우가 있다.
- 불필요할 때 AnyView를 사용하면 성능 저하 이슈 발생 가능
- ViewBuilder는 조건문을 단일 일반 뷰 구조로 변환하는 역할을 해줌
- 따라서 함수에 ViewBuilder를 명시하면 해결됨
- if/else → switch로  
    

# **Lifetime**

- ID를 사용하면 시간이 지남에 따라 다양한 값에 대한 안정적인 요소를 정의할 수 있다.
- 시간이 지남에 따라 연속성을 파악할 수 있음
- SwiftUI는 비교를 수행하고 뷰가 변경되었는지 알기 위해 값의 복사본을 유지함. 그 이후에는 value가 소멸됨
- 이해해야 되는 것 = 뷰 value와 뷰 identity는 다름
- 따라서, 뷰의 value는 일시적이므로 수명에 의존해서는 안된다. **Identity**를 통제해야 한다.

1. 뷰가 처음 생성되고 나타날 때, Identity 할당
2. 시간이 지남에 따라, 업데이트에 따라, 뷰에 대한 새로운 value가 생성됨  
    → SwiftUI의 관점에서는 동일한 View를 나타냄
3. View의 Identity가 변경되거나 뷰가 제거되면 수명이 종료됨

- **A view’s lifetime is the duration of the identity**
- State 및 StateObject는 뷰의 ID와 관련된 영구 저장소
- 뷰의 Identity가 처음 생성될 때 SwiftUI는 초기값을 사용하여 State or StateObject에 대한 메모리에 저장소를 할당함
- 뷰의 Lifetime 동안 SwiftUI는 이 저장소가 변경되고 본문이 re-evaluated 될 때 이 저장소를 유지함
- Identity의 변화가 state에 어떤 영향을 미치는지 예
    - 같은 뷰를 다른 분기에서 사용 시, State는 초기화 되버림. 실제 뷰에 대한 저장소는 바로할당 취소
- Identity가 변경될 때 마다 State가 대체된다.
- State lifetime = view lifetime
    - 뷰의 본질, 즉 상태가 무엇인지 명확하게 분리하고 이를 Identity와 연결할 수 있기에 매우 강력한 개념
- 데이터의 identity를 기반으로 명시적 identity를 제공해야 하는 뷰들은 더더욱 중요함
    - **ForEach, alert, List, Table, OutlineGroup**
- ForEach
    - 초기화 프로그램을 동적 범위와 함께 사용하는 것은 오류가 있음
    - ForEach의 id는 hashable 해야함
    - Identifiable 적용 시 키 경로(\.id) 생략 가능
    - Identity를 통해서 뷰와 데이터의 생명 주기가 싱크가 맞춰지기 때문에, 이 Identity를 잘 정해야 한다.
- 식별 가능한 프로토콜의 목적은 SwiftUI가 수명 동안 데이터를 추적할 수 있도록 유형이 안정적인 ID 개념을 제공하도록 허용한다.  
    → identity and lifetime의 개념과 유사
- Choosing a good identifier is your opportunity to control the lifetime of your views and data

1. A view’s value is short-lived. lifetime에 의존하지 말자
2. A view’s lifetime is the duration of its identity. identity는 시간이 지남에 따라 연속성을 제공한다.
3. Persistence of stats is tied to lifetime.  
    우리가 뷰의 Identity를 제어할 수 있으며, Identity를 사용하여 State의 Lifetime을 명확하게 지정할 수 있음
4. Provide a stable identity for your data  
    → 식별 가능한 프로토콜을 최대한 활용. 안정적인 식별자를 선택하자

# **Dependencies**

- Dependency는 뷰에 대한 input
- Dependency가 변경되면 View는 새로운 Body를 생성해야 함(업데이트)
- Body는 뷰의 계층 구조를 만드는 곳
- Action은 뷰의 dependency에 대한 변경을 트리거하는 것
- View는 트리구조, Dependency를 포함하면 그래프 형태가 됨  
    → Dependency 그래프
- Dependency 그래프
    - 새로운 body가 필요한 View만 효율적으로 업데이트할 수 있도록 하기 때문에 중요함
    - SwiftUI의 기저구조
    - 그래프에서 각 노드는 Identity를 가짐  
        → Identity는 그래프의 중추
    - 효율적으로 UI를 업데이트 해줌
- Identifier stability
    - Lifetime과 직접적인 연관이 있음
    - 성능 향상에 도움을 줌
    - Minimizes dependency churn
    - State 손실 방지
- Identifier uniqueness
    - 애니메이션 개선
    - 성능 향상에 도움
    - Dependencies를 올바르게 반영함
- Explicit identity
    - 우리가 SwiftUI를 도와줘야 할 부분
    - 랜덤 Identity를 사용하는 것을 주의하기
    - 안정적인 Identity를 사용하자
    - 고유성 보장, 1:1
        - 여러 뷰가 식별자를 공유할 수 없음
        - 매번 UUID를 초기화하거나 정수의 인덱스를 그대로 사용하는 경우
- Structural identity
    - 불필요한 분기 방지
        - 의도치 않은 결과가 발생할 수 있음
    - Dependent code의 범위를 빡세게 제한하기
    - inert modifieirs 사용을 하는게 낫다 

like 2개

---
[Swift스러운 Syntax] 

오~랫만에 ![🙁](https://statics.teams.cdn.office.net/evergreen-assets/personal-expressions/v2/assets/emoticons/sad/default/60_f.png?v=v30)

새로운 포스팅이 등록되었어요
[오전 9:51] 강희종(애플 아카데미) 아이작

[Swift스러운 Syntax] Day9 - @State, @ObservableObject, @StateObject, @EnvironmentObject

[이전 토픽](https://teams.microsoft.com/l/message/19:e086b8212ac24898baabe43610515175@thread.tacv2/1700547923467?tenantId=bff3e98c-5cca-455c-adc8-5fd24fc9908d&groupId=98602378-0e10-4295-bd1e-e63921e3b18c&parentMessageId=1700547923467&teamName=Cohort%202023%20-%20Apple%20Developer%20Academy%20%40%20POSTECH&channelName=All%20Code&createdTime=1700547923467 "https://teams.microsoft.com/l/message/19:e086b8212ac24898baabe43610515175@thread.tacv2/1700547923467?tenantid=bff3e98c-5cca-455c-adc8-5fd24fc9908d&groupid=98602378-0e10-4295-bd1e-e63921e3b18c&parentmessageid=1700547923467&teamname=cohort%202023%20-%20apple%20developer%20academy%20%40%20postech&channelname=all%20code&createdtime=1700547923467")의 Dependencies 파트를 통해서 어느 정도 익숙해지셨을 거라고 생각하지만, 한번더 굳히고 가보려고 합니다 ![🙂](https://statics.teams.cdn.office.net/evergreen-assets/personal-expressions/v2/assets/emoticons/smile/default/20_f.png "Smile")

(FYI, 이전 토픽 댓글 쩌니까 꼭 보세요!! 두번 보세요!!)

이번 토픽은 SwiftUI에서 사용하는 

**1. @State**

**2. @ObservableObject** 

**3. @StateObject**

**4. @EnvironmentObject** 

입니다!!

이 [링크의 영상(Data Essentials)](https://developer.apple.com/wwdc20/10040 "https://developer.apple.com/wwdc20/10040")을 보면서 상기한 4가지 방법이 

SwiftUI의 Lifetime, Dependencies 측면에서 어떻게 차이가 있는지 한번 정리해보시면 됩니다~!

이전과 마찬가지로, 

댓글이 작성하기 불편하시다면 Notion이나 블로그 등 본인이 작성하기 편하고 공유할 수 있는 플랫폼도 적극적으로 활용해주셔도 좋습니다 ![🙂](https://statics.teams.cdn.office.net/evergreen-assets/personal-expressions/v2/assets/emoticons/smile/default/20_f.png "Smile")

참고 리소스

- [(WWDC 20)Data Essentials](https://developer.apple.com/wwdc20/10040 "https://developer.apple.com/wwdc20/10040")
- [(Documentation) State](https://developer.apple.com/documentation/swiftui/state "https://developer.apple.com/documentation/swiftui/state")
- [(Documentation) StateObject](https://developer.apple.com/documentation/swiftui/stateobject "https://developer.apple.com/documentation/swiftui/stateobject")
- [(Documentation) ObservableObject](https://developer.apple.com/documentation/combine/observableobject "https://developer.apple.com/documentation/combine/observableobject")  

Data Essentials in SwiftUI - WWDC20 - Videos - Apple Developer

Data is a complex part of any app, but SwiftUI makes it easy to ensure a smooth, data-driven experience from prototyping to production...