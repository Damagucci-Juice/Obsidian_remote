## 원인
- Custom Font 추가 왜 안되는지 아는분
	- iPhone에서는 인식이 되는 것을 확인 했는데
	- WatchOS에서는 그것이 확인이 안된다.
## 해결책
- 확인 결과 WatchOS의 Info.plist에서도 폰트를 추가해줘야 한다.
- Copy Bundle Resource에서도 확인해야함 
- 실제와 파일 이름이 다를 수도 있음

## 파일 이름 확인하는 코드
```swift 
            .onAppear {

                for family in UIFont.familyNames.sorted() {
                    print("Family: \(family)")
                    
                    let names = UIFont.fontNames(forFamilyName: family)
                    for fontName in names {
                        print("- \(fontName)")
                    }
                }
            }
```


```swift
extension UIFont { static func pretendard(size fontSize: CGFloat, weight: UIFont.Weight) -> UIFont { let familyName = "Pretendard" var weightString: String switch weight { case .black: weightString = "Black" case .bold: weightString = "Blod" case .heavy: weightString = "ExtraBold" case .ultraLight: weightString = "ExtraLight" case .light: weightString = "Light" case .medium: weightString = "Medium" case .regular: weightString = "Regular" case .semibold: weightString = "SemiBold" case .thin: weightString = "Thin" default: weightString = "Regular" } return UIFont(name: "\(familyName)-\(weightString)", size: fontSize) ?? .systemFont(ofSize: fontSize, weight: weight) } }
```
