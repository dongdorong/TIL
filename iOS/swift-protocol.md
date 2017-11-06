# Protocol
- swift의 프로토콜은 자바의 인터페이스와 같은 개념
- 프로토콜을 따르는 클래스들도 다형성 적용 가능
- iOS 개발을 할 때 이미 만들어진 프로토콜을 사용하는 경우가 많으므로 어떻게 상속되고 사용되는지 잘 알아두기

```swift
protocol Shape {
    var height: Int { get set }
    var description: String { get set }
    
    func getArea() -> Double
}
```
### property
- get:: property를 받아서 사용
- set:: property에 새로운 값 지정 (상수 사용 X)
- 프로토콜 규칙에 따라 클래스 property 지정

### 메소드
- getArea:: 파라미터를 받지 않고 Double로 리턴해주는 규칙을 따른다.

### 클래스
```swift
class Triangle: Shape {
    var height: Int
    var width: Int
    var description: String
    
    init(height: Int, width: Int, description: String) {
        self.height = height
        self.width = width
        self.description = description
    }
    
    func getArea() -> Double {
        return 1/2 * Double(width) * Double(height)
    }
}
```

```swift
class Square: Shape{
    var height: Int
    var description: String
    
    init(height: Int, description: String) {
        self.height = height
        self.description = description
    }
    
    func getArea() -> Double {
        return Double(height * height)
    }
}
```

### 다형성
```swift
\\Shape을 상속 받는 빈 배열 생성
var protocolList: [Shape] = []

protocolList.append(Triangle(height: 10, width: 7, description: "First Traiagle"))
protocolList.append(Square(height: 12, description: "Fisrt Square"))

for shape in protocolList{
    print(shape.getArea())
}
```