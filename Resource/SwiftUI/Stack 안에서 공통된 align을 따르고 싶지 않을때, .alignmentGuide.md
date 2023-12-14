 
```
VStack(alignment: .leading) { 
	Text("Hello, world!")
		.alignmentGuide(.leading) { d in 
			d[.leading] 
		} 
	Text("This is a longer line of text") 
}
```

https://www.hackingwithswift.com/books/ios-swiftui/how-to-create-a-custom-alignment-guide

https://www.hackingwithswift.com/books/ios-swiftui/alignment-and-alignment-guides

추가로 해볼 것, HStack에서 모두 탑에 맞추고 하나만 바텀에 맞추기?  이런거 해보면서 이해해보자.