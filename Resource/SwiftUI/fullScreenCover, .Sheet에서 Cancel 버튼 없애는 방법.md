

https://github.com/DeveloperAcademy-POSTECH/MacC-Team10-Guryongpo/pull/55

```swift
AlertView()
    .navigationBarBackButtonHidden() // 적용 안됨
    .toolbar { // 적용됨 
        ToolbarItem(placement: .cancellationAction) {
            Button("") { }
                .hidden()
        }
    }
```
