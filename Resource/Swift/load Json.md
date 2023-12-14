https://stackoverflow.com/questions/24410881/reading-in-a-json-file-using-swift

[yarlg](https://stackoverflow.com/users/1424206/yarlg) 답변이 좋음 207 추천

- Generics를 이용해서 바꾸는 방법
https://www.hackingwithswift.com/books/ios-swiftui/using-generics-to-load-any-kind-of-codable-data
https://www.hackingwithswift.com/books/ios-swiftui/loading-a-specific-kind-of-codable-data

```swift
extension Bundle {
    func decode<T: Decodable>(by fileName: String) -> T {
        guard let url = self.url(forResource: fileName, withExtension: nil) else {
            fatalError("Failed to locate \(fileName) in bundle.")
        }
        
        guard let data = try? Data(contentsOf: url) else {
                    fatalError("Failed to load \(fileName) from bundle.")
                }
        
        let decoder = JSONDecoder()
        
        guard let loaded = try? decoder.decode(T.self, from: data) else {
            fatalError("Failed to decode \(fileName) from bundle.")
        }
        
        return loaded
    }
}

```

