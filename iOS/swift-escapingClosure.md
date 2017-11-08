# 탈출 클로저
- Escaping Closure:: 함수 실행이 끝난 후 호출 
- Nonescaping Closure:: 함수 내에서 바로 실행

### 예시 savedClosure
```swift
// 클로저 보관 변수
var savedClosure: () -> Void = {}

// 탈출 클로저 @escaping
func function1(someClosure: @escaping () -> Void) {
    savedClosure = someClosure // savedClosure에 보관
}

// nonescaping closure
func function2(closure: () -> Void) {
    closure() // 함수 내에서 바로 실행
}

class someClass {
    var x = 10
    func doSomething() {
        function2{ x = 200 } // Nonescaping Closure
        function1{ self.x = 100 } // Escaping Closure
    }
}
``` 
- Escaping Closure:: self 생략 불가
- Nonescaping Closure:: self를 생략 하는 것을 권장 (self를 자동 인식)


### 호출
```swift
let instance = someClass()
instance.doSomething()
print(instance.x) // 값: 200, Nonescaping Closure 실행. 바로 값이 바뀜

savedClosure() // savedClosure 실행. 100이라는 값을 저장
print(instance.x) // 값: 100, 함수 호출이 끝난 후 값이 바뀜
```
