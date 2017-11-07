# Closure 
- 이름 없는 함수
- lambda와 유사
- 변수나 상수가 선언된 위치에서 참조를 획득하고 저장 가능

### 클로저 문법
```swift
// 함수 정의 문법
func funcName(parameters) -> return type {
    statements
}

// 위 함수에 해당하는 클로저 문법
{(pafameters) -> return type in 
    statements
}
```

### 함수 예시
```swift
func largerFirst(First: Int, Second: Int) -> Bool {
    return First > Second
}

var numArray = [1, 2, 3, 4, 5, 6, 7]
var sortedArray = numArray.sorted(by: largerFirst)
```
- 함수를 써서 넘기는 방식은 권장하지 않는다. (함수를 찾아서 봐야하기 때문)

### 함수를 클로저로 변경 1
```swift
var sortedArray = numArray.sorted(by: {(First: Int, Second: Int) -> Bool in
    return First > Second
})
```

### 함수를 클로저로 변경 2
```swift
var sortedArray = numArray.sorted(by: {
    First: Int, Second in
    return First > Second
})
```

### 함수를 클로저로 변경 3 - Trailing Closure (후행 클로저)
- 함수의 맨 마지막 파라미터가 클로저 일 때 사용 가능
- 파라미터 by는 sortred 매소드의 마지막 파라미터, 생략해서 사용 가능 
```swift
var sortedArray = numArray.sorted() {
    First: Int, Second in
    return First > Second
}
```

```swift
var sortedArray = numArray.sorted {
    return $0 > $1 // 단축인자로 표현
}
```

```swift
var sortedArray = numArray.sorted { $0 > $1 } // return 값 제거 가능
```