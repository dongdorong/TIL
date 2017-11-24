# 배열 (Array)
- 대괄호 사이에 쉽표로 분리해서 사용

```swift
var newArray = [1, 2, 3, 4, 5, 6, 7]
```
- 자동으로 [Int] 정수형 배열로 인식

### append
```swift
newArray.append(8)
newArray.append(9)
newArray.append(10)

print(newArray) // 1, 2, 3, 4, 5, 6, 7, 8, 9, 10
print(newArray[0]) // 1
print(newArray[10]) // error 없는 값 
```

### 다른 메서드

- removeLast() :: 마지막 값을 제거
- removeFirst() :: 첫번째 값을 제거 
- remove(at: 인덱스) :: 특정 인덱스 값을 제거

### 빈 배열
- 빈배열은 타입을 유추 할 수 없기 때문에 타입을 직접 설정해줘야한다.

```swift
var newArray = [] // error

var newArray: [String] = []
```

### Tip!
```swift
var newArray = [1, 2, 3, 4, 5, 6, 7]

newArray.count // 6

newArray += [8, 9, 10]

newArray.contains(1) // true
newArray.contains(11) // false
```
- count:: Array의 총 개수
- += :: 같은 타입의 배열들은 덧셈 연산자 사용 가능
- contains :: 어떤 값이 있는지 불린으로 확인