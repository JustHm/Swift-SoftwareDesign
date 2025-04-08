## 개방-폐쇄 원칙 (Open/Closed Principle)

개방-폐쇄 원칙은 소프트웨어의 엔티티(클래스, 모듈, 함수 등)는 **확장에는 열려 있어야 하고, 수정에는 닫혀 있어야 한다**는 설계 원칙입니다.  
즉, 기존 코드를 수정하지 않고도 새로운 기능이나 동작을 **추가할 수 있어야 한다**는 의미입니다.

---

### 개방-폐쇄 원칙 적용 전:

예시를 하나 살펴보겠습니다.  
`Shape`라는 클래스가 있고, 이 클래스는 `calculateArea()`라는 메서드를 가지고 있습니다.  
초기에는 단지 **사각형의 면적**만 계산하도록 구성되어 있습니다:

```swift
class Shape {
    func calculateArea(length: Double, width: Double) -> Double {
        return length * width
    }
}
```

이제 **원의 면적 계산도 추가**하고 싶다고 가정해 봅시다.  
개방-폐쇄 원칙을 따르지 않는다면, 기존의 `Shape` 클래스에 새로운 메서드  
`calculateArea(radius: Double)`를 직접 추가할 수도 있습니다:

```swift
class Shape {
    func calculateArea(length: Double, width: Double) -> Double {
        return length * width
    }
    
    func calculateArea(radius: Double) -> Double {
        return 3.14 * radius * radius
    }
}
```

이 접근 방식의 문제는, **기존 클래스를 수정했다는 것**입니다.  
이로 인해 기존 코드에 **버그가 발생할 가능성**이 생기고,  
`Shape` 클래스를 사용하는 다른 코드들에도 **의도하지 않은 영향**을 줄 수 있습니다.

---

### 개방-폐쇄 원칙 적용 후:

개방-폐쇄 원칙을 준수하려면, 우리는 `Shape`라는 추상 클래스 또는 프로토콜을 만들고,  
각 구체적인 도형(사각형, 원 등)을 **별도의 클래스**로 정의할 수 있습니다:

```swift
protocol Shape {
    func calculateArea() -> Double
}

class Rectangle: Shape {
    let length: Double
    let width: Double

    init(length: Double, width: Double) {
        self.length = length
        self.width = width
    }

    func calculateArea() -> Double {
        return length * width
    }
}

class Circle: Shape {
    let radius: Double

    init(radius: Double) {
        self.radius = radius
    }

    func calculateArea() -> Double {
        return 3.14 * radius * radius
    }
}
```

이 방식에서는 `Shape` 프로토콜이 **모든 도형이 가져야 할 공통 인터페이스**를 정의합니다.  
그리고 `Rectangle`, `Circle`과 같은 구체적인 클래스들이 **자신만의 면적 계산 로직**을 구현합니다.  
새로운 도형이 추가되더라도, 기존 코드를 **전혀 수정하지 않고**  
새로운 클래스를 만들어 `Shape` 프로토콜만 채택하면 됩니다.

---

개방-폐쇄 원칙을 따르게 되면,  
**기존 코드를 건드리지 않고도** 소프트웨어에 새로운 기능을 손쉽게 추가할 수 있으며,  
**버그 발생 위험도 줄어들고**,  
**기존 기능에 영향 없이 확장이 가능**해집니다.
