## 의존성 역전 원칙 (Dependency Inversion Principle)

**의존성 역전 원칙(DIP)**은 소프트웨어 설계 원칙 중 하나로,  
**상위 모듈(클래스)이 하위 모듈에 직접 의존하지 말고, 추상화(인터페이스나 프로토콜)에 의존해야 한다**는 원칙입니다.  
그리고 하위 모듈이 이 추상화를 구현하게 하면 됩니다.

---

### DIP 적용 전:

DIP를 적용하기 전에는 상위 모듈이 하위 모듈의 구체적인 구현에 직접 의존하고 있습니다.  
이 경우 하위 모듈에 변화가 생기면 상위 모듈도 영향을 받아 코드가 단단히 결합되고 유연성이 떨어집니다.  

다음은 결제 시스템 예제입니다:

```swift
class PaymentProcessor {
    private let paymentGateway: PaymentGateway

    init() {
        paymentGateway = PaymentGateway()
    }

    func processPayment(amount: Double) {
        // 결제 게이트웨이를 사용해 결제 처리
        paymentGateway.processPayment(amount)
    }
}

class PaymentGateway {
    func processPayment(amount: Double) {
        // 실제 결제 처리 로직
    }
}
```

위 코드에서는 `PaymentProcessor`가 `PaymentGateway`라는 구체 클래스에 직접 의존하고 있습니다.  
만약 다른 결제 게이트웨이를 추가하거나 교체하려면, `PaymentProcessor`를 수정해야 합니다.  
이는 **캡슐화 원칙을 위반**하고, 유지보수나 테스트가 어려워지는 원인이 됩니다.

---

### DIP 적용 후:

DIP를 적용하면, 상위 모듈은 **프로토콜 같은 추상화**에 의존하고,  
하위 모듈이 그 추상화를 **구현(채택)** 하게 됩니다.

```swift
protocol PaymentGateway {
    func processPayment(amount: Double)
}

class PaymentProcessor {
    private let paymentGateway: PaymentGateway

    init(paymentGateway: PaymentGateway) {
        self.paymentGateway = paymentGateway
    }

    func processPayment(amount: Double) {
        // 주입받은 결제 게이트웨이를 통해 결제 처리
        paymentGateway.processPayment(amount)
    }
}

class PayPalGateway: PaymentGateway {
    func processPayment(amount: Double) {
        // PayPal을 사용한 결제 처리 구현
    }
}

class StripeGateway: PaymentGateway {
    func processPayment(amount: Double) {
        // Stripe를 사용한 결제 처리 구현
    }
}
```

이제 `PaymentGateway`라는 **프로토콜**이 결제 처리 규약을 정의하고 있습니다.  
`PaymentProcessor`는 더 이상 구체 클래스에 의존하지 않고,  
**프로토콜을 따르는 객체**만 전달되면 어떤 결제 게이트웨이든 사용 가능합니다.

런타임에서 `PayPalGateway`, `StripeGateway`와 같은 다양한 구현체를 주입할 수 있고,  
`PaymentProcessor`는 전혀 변경하지 않아도 됩니다.

---

### 이점

이러한 설계를 하면:

- 결제 게이트웨이 변경 또는 확장이 쉬워지고,
- **결합도가 낮아져** 유지보수성과 확장성이 좋아지며,
- 테스트 시에도 **모킹(mocking)** 이 용이해져 유닛 테스트 작성이 쉬워집니다.

즉, DIP는 유연하고 확장 가능한 구조를 만드는 데 핵심적인 원칙입니다.
