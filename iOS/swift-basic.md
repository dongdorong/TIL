# swift Basic

### 변수와 상수

```
//변수
var str = "Hello Dona"

//이미 정한 변수를 바꿀때
str = "Hello Everyone"

//상수
let myConstant = "Hi Swift"

//타입 설정
var str: String = "Hello Dona"

//타입을 설정해줘야 오류가 안난다
var myString: String

//숫자연산
var myInt = 10       //정수형 
var myDouble = 23.5  //소수형

myInt + myDouble     //error

//형변환
Double(myInt) + myDouble
```

 - Swift는 Static Type: 정의된 변수의 타입 변경 불가
 - 변수나 상수를 만들때 타입을 정의해준다
 - 숫자연산은 같은 타입끼리 가능
 
### 불린
```
//AND 연산자 
true && true    //true
true && false   //false
false && false  //false

//OR 연산자
true || true    //true
true || false   //false
false || false  //false

!false          //true
var myBloolean = true
!myBoolean      //flase
``` 

### 함수와 함수호출 1
```
func welcome(name: String, className: String, message: String) {
    print("Hello :) \(name). welcome to \(className)/ \(message)")   
}

welcome(name: "dona", className: "Swift", message: "반가워요!")

```

### 함수와 함수호출 2
```
func welcome(Student name: String, className: String, _ message: String) {
    print("Hello :) \(name). welcome to \(className)/ \(message)")   
}

welcome(Student: "dona", className: "Swift", "반가워요!")
```
- 파라미터를 2개 써주면 [Student]는 호출할때, [name]은 함수 수행부분에 사용
- 파라미터에 (_)를 추가해주면 호출할때 이름을 사용하지 않는다. 

### Return
```
func add(x: Int, y: Int) -> Int {
    return x + y
}
```
- 리턴값을 주고 싶으면 (->) 화살표와 리턴하는 값의 자료형 설정
 
 ### 옵셔널(Optional)
 - 값이 nil을 갖을 경우 사용
 - (?)를 붙여서 옵셔널 표시
 - (!)를 붙이면 원래 타입으로 사용 - 값이 nil이 아닐때만 가
 
 ```
//옵셔널 변수의 값이 nil인지 확인할 때
var optionalTest: String? = "Hello Dona"
 
 if let test = optionalTest {
    print(test)
 } else {
    print("변수 test의 값은 nil입니다.")
 }
```
- if let을 사용하면 optionalTest 변수가 nil일 경우 else 문이 실행, nil이 아닐 경우 optionalTest! 가 test에 지정되어 출력
 
