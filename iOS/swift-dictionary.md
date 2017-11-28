# 사전 (Dictionary)
- key: value 로 이루어진 사전

```swift
var newArray = [
    dona: 27,
    anggo: 21
]
```
- key: value값을 쉼표로 구분

### value값 받기
```swift
var newValue = newArray["dona"]
```

### 유의할 점!
- Dictionary에서 value로 받아오는 값은 옵셔널 값
- 옵셔널인 이유: 사전에 없는 값을 가져오면 nil이 나오기 때문

### 옵셔널을 풀어줄 안전한 방법
```swift
var name = "dongdorong"

// key가 Dictionary안에 있는지 없는지 모를때
if let test = newArray[name] {
    print(test)
}else {
    print("newArray dose not have key \(name)")
}
```

### key가 있다고 확실할 때: ! 사용
```swift
print(newArray[name]!)
``` 

### 변경 
```swift
newArray["dona"] = 28
``` 
- 원래 존재하던 key값이기에 value값이 새롭게 추가됨

### 추가
```swift
newArray["dongdorong"] = 100
``` 
- 사전에 없는 값이기에 새로운 key-value값이 추가된다. 

### 삭제
```swift
newArray.removeValue(forKey: "dongdorong")
``` 
- key-value값을 쌍으로 삭제 할 때

### 빈 사전
```swift
// key: 문자열 , value: 정수형
var newDict: [String: Int] = [:]

//추가 
newDict["anna"] = 30

//삭제
newDict.removeKeyForValue("anna")
``` 
- 빈 사전을 정의 할땐 항상 타입을 정해줘야 한다. (빈 배열과 같다.)

### for 문
- 사전의 값을 볼 때 사용한다.
- 값을 사용할 때에도 사용한다. 

```swift
let who = [
    "dona": 27,
    "dev": 31,
    "anggo": 21
]

for (name, age) in who {
    print("\(name)is \(age).")
}
```

### count
```swift
newDict.count
```
- 사전의 개수를 리턴 받는다. 

### 정보 정리하기 

```swift
let dataString = "dona: 27, dev: 31, anggo: 21"

let dataSeparatedByComma = dataString.components(separatedBy: ",") // 콤마를 기준으로 정리 "dona: 27", "dev: 31", "anggo: 21"

var dataDict: [String: Int] = [:]

for element in dataSeparatedByComma{
    let separatedByColon = element.trimmingCharacters(in: CharacterSet.whitespacesAndNewlines).components(separatedBy: ":") // 공배 제거 후 콜론을 기준으로 정리
    
    dataDict[separatedByColon[0]] = separatedByColon[1].trimmingCharacters(in: CharacterSet.whitespacesAndNewlines)
    
    print(dataDict) // ["dona": 27, "dev": 31, "anggo": 21]
}

```


 
