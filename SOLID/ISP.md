## 인터페이스 분리 원칙 (Interface Segregation Principle)

인터페이스 분리 원칙(ISP)은 **큰 단일 인터페이스를 작고 특화된 인터페이스로 나눌 것**을 제안하는 설계 원칙입니다.  
이 원칙은 **클라이언트가 사용하지 않는 인터페이스에 의존하지 않도록 해야 한다**는 아이디어를 중심으로 합니다.  
즉, **각 클라이언트에 맞춘 명확하고 구체적인 인터페이스를 생성하자**는 의미입니다.

---

### ISP 적용 전:

아래 예시에서는 `Printer`라는 프로토콜이 있고, 이 안에 여러 기능이 포함되어 있습니다:

```swift
protocol Printer {
    func printDocument()
    func scanDocument()
    func faxDocument()
    func copyDocument()
}

class OfficePrinter: Printer {
    func printDocument() {
        // 구현
    }

    func scanDocument() {
        // 구현
    }

    func faxDocument() {
        // 구현
    }

    func copyDocument() {
        // 구현
    }
}

class HomePrinter: Printer {
    func printDocument() {
        // 구현
    }

    func scanDocument() {
        // 구현
    }

    func faxDocument() {
        // 구현
    }

    func copyDocument() {
        // 구현
    }
}
```

이 예제에서 `OfficePrinter`와 `HomePrinter` 모두 `Printer` 프로토콜을 구현하고 있습니다.  
하지만 실제로는 `HomePrinter`는 **팩스나 복사 기능이 없는 경우가 많습니다.**  
그럼에도 불구하고 `Printer` 프로토콜 전체를 구현해야 하므로,  
`HomePrinter`는 불필요하거나 의미 없는 구현을 강요받게 됩니다.

---

### ISP 적용 후:

ISP를 적용하면, 기능별로 인터페이스(프로토콜)를 세분화하게 됩니다:

```swift
protocol Printable {
    func printDocument()
}

protocol Scanable {
    func scanDocument()
}

protocol Faxable {
    func faxDocument()
}

protocol Copyable {
    func copyDocument()
}

class OfficePrinter: Printable, Scanable, Faxable, Copyable {
    func printDocument() {
        // 구현
    }

    func scanDocument() {
        // 구현
    }

    func faxDocument() {
        // 구현
    }

    func copyDocument() {
        // 구현
    }
}

class HomePrinter: Printable, Scanable {
    func printDocument() {
        // 구현
    }

    func scanDocument() {
        // 구현
    }
}
```

이제 각 기능은 별도의 프로토콜(`Printable`, `Scanable`, `Faxable`, `Copyable`)로 나뉘었습니다.  
`OfficePrinter`는 모든 기능을 지원하므로 네 가지 모두 구현하고,  
`HomePrinter`는 인쇄와 스캔만 구현하면 됩니다.  

---

이처럼 인터페이스를 나누면 각 클래스가 **자신의 책임에만 집중**할 수 있게 됩니다.  
결과적으로 **코드는 더 깔끔하고 유지보수가 쉬워지며**,  
**사용하지 않는 메서드에 의존하는 일도 방지**할 수 있어  
불필요한 결합도와 오류 가능성을 줄일 수 있습니다.
