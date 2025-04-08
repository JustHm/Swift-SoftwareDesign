## 리스코프 치환 원칙 (Liskov Substitution Principle)

리스코프 치환 원칙(LSP)은 객체 지향 프로그래밍에서 매우 중요한 원칙으로,  
**상위 클래스의 객체는 하위 클래스의 객체로 대체되어도 프로그램의 정확성이 유지되어야 한다**고 말합니다.  
쉽게 말해, **기본 클래스가 필요한 곳에 자식 클래스가 들어와도 아무 문제 없이 동작해야 한다**는 의미입니다.

---

### LSP를 이해하기 위한 예시:

`Shape`라는 슈퍼클래스가 있고, 이 클래스에는 도형의 면적을 계산하는 `calculateArea()` 메서드가 있다고 가정해 봅시다.  
그리고 두 개의 서브클래스 `Rectangle`과 `Square`가 있습니다.  
`Rectangle`은 `length`와 `width`를 가지고 있고, `Square`는 하나의 `sideLength`만 가집니다.

다음은 LSP를 적용하지 않은 상태의 클래스 구조입니다:

```swift
class Shape {
    // 면적을 계산하는 메서드
    func calculateArea() -> Double {
        fatalError("반드시 오버라이드해야 합니다.")
    }
}

class Rectangle: Shape {
    var length: Double
    var width: Double

    init(length: Double, width: Double) {
        self.length = length
        self.width = width
    }

    override func calculateArea() -> Double {
        return length * width
    }
}

class Square: Shape {
    var sideLength: Double

    init(sideLength: Double) {
        self.sideLength = sideLength
    }

    override func calculateArea() -> Double {
        return sideLength * sideLength
    }
}
```

이제 `Shape` 타입을 받아 면적을 출력하는 함수가 있다고 가정해봅시다:

```swift
func printArea(shape: Shape) {
    let area = shape.calculateArea()
    print("Area: \(area)")
}
```

다음과 같이 `Rectangle`과 `Square` 객체를 넘겨서 사용할 수 있습니다:

```swift
let rectangle = Rectangle(length: 5, width: 3)
printArea(shape: rectangle) // 출력: Area: 15

let square = Square(sideLength: 4)
printArea(shape: square) // 출력: Area: 16
```

이 경우 결과는 맞게 출력되지만, 실제로 `Square` 객체는  
`Shape` 상위 클래스가 기대하는 **행동(behavior)** 을 완전히 따르지는 않습니다.  
LSP는 하위 클래스가 상위 클래스의 역할을 **정확하게 대체할 수 있어야** 한다고 말합니다.

---

### LSP를 준수하도록 코드 리팩토링하기:

이 문제를 해결하기 위해 **상속 대신 구성(composition)** 을 사용하는 방법이 있습니다.  
Swift에서는 프로토콜을 통해 이 원칙을 보다 명확히 따를 수 있습니다:

```swift
protocol Shape {
    // 면적을 계산하는 메서드
    func calculateArea() -> Double
}

struct Rectangle: Shape {
    var length: Double
    var width: Double

    func calculateArea() -> Double {
        return length * width
    }
}

struct Square: Shape {
    var sideLength: Double

    func calculateArea() -> Double {
        return sideLength * sideLength
    }
}
```

`Shape` 프로토콜이 면적 계산을 위한 **공통 인터페이스**를 정의하고,  
각 구조체는 자신의 로직에 맞게 이 메서드를 구현합니다.

`printArea` 함수도 그대로 사용할 수 있습니다:

```swift
func printArea(shape: Shape) {
    let area = shape.calculateArea()
    print("Area: \(area)")
}
```

이제 리팩토링된 코드에서는 다음과 같이 두 객체 모두 전달할 수 있으며,  
리스코프 치환 원칙을 **완벽히 만족**하게 됩니다:

```swift
let rectangle = Rectangle(length: 5, width: 3)
printArea(shape: rectangle) // 출력: Area: 15

let square = Square(sideLength: 4)
printArea(shape: square) // 출력: Area: 16
```

이제 이 코드는 LSP를 제대로 따릅니다.  
`Shape`라는 상위 타입을 요구하는 곳에 `Rectangle`이나 `Square` 객체를 넣어도  
**동작에 아무런 문제가 없이 잘 작동**하죠.
