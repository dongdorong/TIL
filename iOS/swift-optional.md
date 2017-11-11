# 옵셔널(Optional)
 - 값이 nil을 갖을 경우 사용
 - (?)를 붙여서 옵셔널 타입 표시
 - (!)를 붙이면 옵셔널 타입을 풀어 원래 타입으로 변환 
 
 ### 예시
 ```swift
 var testString = "hello dona"
 
 testString = nil // error!!
 ```
- 문자열은 nil일 수 없기에 에러 발생!
- nil 값을 갖게 하려면 옵셔널 타입으로 만들어 줘야한다.
 
 ### ? 옵셔널 타입으로 변환
 ```swift
 var testString: String? = "hello dona" \\ ? 를 붙임
 ```

### ! 원래 타입으로 변환
```swift
 print(testString!) // ! 붙임
```
- 이 방법은 옵셔널 값이 nil이 아닐때만 가능하다.

 ### if let 안전한 방법 
 ```swift
//옵셔널 변수의 값이 nil인지 확인할 때
var optionalTest: String? = "Hello Dona"
 
 if let test = optionalTest {
    print(test)
 } else {
    print("변수 test의 값은 nil입니다.")
 }
```
- if let을 사용하면 optionalTest 변수가 nil일 경우 if 조건문이 false가 되기 때문에 else 문이 실행
- nil이 아닐 경우 옵셔널 타입을 풀어주는 optionalTest! 가 test에 지정되어 출력 
 
