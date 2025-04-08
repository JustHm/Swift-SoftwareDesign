## 단일 책임 원칙 (Single Responsibility Principle)

단일 책임 원칙(SRP)은 클래스나 모듈은 **오직 하나의 변경 이유만 가져야 한다**는 소프트웨어 설계 원칙입니다.  
좀 더 간단히 말하자면, 각 클래스는 **단일한 책임 또는 작업만을 가져야** 하며,  
서로 관련 없는 여러 작업을 동시에 담당해서는 안 된다는 의미입니다.

SRP를 적용하기 전에, 먼저 하나의 예제를 살펴보겠습니다.  
급여 시스템에 `Employee`라는 클래스가 있다고 가정해봅시다.  
이 클래스는 직원 정보를 저장하고, 급여를 계산하며, 급여 명세서를 생성하는 역할을 모두 맡고 있습니다.  
해당 클래스는 다음과 같을 수 있습니다:

```swift
class Employee {
    var name: String
    var id: String
    var salary: Double

    init(name: String, id: String, salary: Double) {
        self.name = name
        self.id = id
        self.salary = salary
    }

    func calculateSalary() {
        // 급여 계산 로직
    }

    func generatePayslip() {
        // 급여 명세서 생성 로직
    }

    // 직원 관리와 관련된 기타 메서드들
}
```

이 예제에서 `Employee` 클래스는 SRP를 위반하고 있습니다.  
왜냐하면 이 클래스는 **여러 가지 책임을 동시에 가지고 있기 때문**입니다.  
직원 정보를 저장할 뿐만 아니라, 급여를 계산하고, 급여 명세서를 생성하는 역할도 수행합니다.  
이로 인해 해당 클래스는 나중에 유지보수하거나 수정하기가 어려워질 수 있습니다.

예를 들어, 급여 계산 로직이나 명세서 생성 방식이 변경된다면  
우리는 `Employee` 클래스를 수정해야 합니다.  
이는 **클래스의 책임이 너무 많을 때 발생하는 전형적인 문제**입니다.

---

SRP를 준수하기 위해, 우리는 책임을 **서로 다른 클래스**로 분리할 수 있습니다.  
SRP를 적용한 후의 코드는 다음과 같습니다:

```swift
class Employee {
    var name: String
    var id: String

    init(name: String, id: String) {
        self.name = name
        self.id = id
    }
}

class SalaryCalculator {
    func calculateSalary(employee: Employee) -> Double {
        // 급여 계산 로직
    }
}

class PayslipGenerator {
    func generatePayslip(employee: Employee) {
        // 급여 명세서 생성 로직
    }
}
```

이제 변경된 코드에서 `Employee` 클래스는 **직원 정보를 저장하는 책임만**을 가집니다.  
`SalaryCalculator` 클래스는 급여 계산을, `PayslipGenerator` 클래스는 명세서 생성을 담당합니다.  
각 클래스는 **하나의 책임만 가지며**, 서로 독립적으로 수정할 수 있어 다른 클래스에 영향을 주지 않습니다.

책임을 이렇게 분리함으로써 우리는 **더 나은 코드 구성, 유지보수성, 유연성**을 얻게 됩니다.  
만약 급여 계산이나 명세서 생성 로직에 변경이 생겨도  
해당 클래스만 수정하면 되므로 `Employee` 클래스는 건드릴 필요가 없습니다.

이러한 **모듈화된 설계 방식**은 코드를 더 이해하기 쉽게 만들고,  
테스트와 유지보수도 쉬워지게 하며,  
이것이 바로 단일 책임 원칙의 핵심입니다.
