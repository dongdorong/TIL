# Date

```swift
let date = Date()
        
let formatter = DateFormatter() // 어떤 포멧으로 내보낼지 정의
        
formatter.dateFormat = "MM.dd.yyyy" // 날짜
        
let dateString = formatter.string(from: date)
        
print(dateString)
        
formatter.dateFormat = "hh:mm:ss" // 시간
        
print(formatter.string(from: date))

```