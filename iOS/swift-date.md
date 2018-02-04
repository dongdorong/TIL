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


### + 추가 팁

- PM AM 추가하고 싶을때 `a` 추가

```
formatter.dateFormat = "a hh:mm:ss" // 시간
```
