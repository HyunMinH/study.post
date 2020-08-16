# 클래스

## 클래스는 추상 데이터 타입인가?

- 명확한 의미에서 서로 다르다.

- 핵심적인 차이는 상속과 다형성을 지원하냐 안하냐이다.

- 프로그래밍 언어적인 관점에서는 클래스는 절차를 추상화 한것, 추상 데이터 타입은 타입을 추상화 한 것이다.

    - 추상 데이터 타입은 오퍼레이션을 기준으로 타입을 묶는다.

    - 객체지향은 타입을 기준으로 오퍼레이션을 묶는다.

## 추상 데이터 타입에서 클래스로 변경하기

### 추상 데이터 타입

```ruby
Employee = Struct.new(:name, :basePay, :hourly, :timeCard) do
    def calculatePay(taxRate)
        if (hourly) then
            return calculateHourlyPay(taxRate)
        end
        return calculateSalariedPay(taxRate)
    end

    private
        def calculateHourlyPay(taxRate)
            return (basePay * timeCard) - (basePay * timeCard) * taxRate
        end
        def calculateSalariedPay(taxRate)
            return basePay - (basePay * taxRate)
        end
    end
end
```
### 클래스
```ruby
class Employee
    attr_reader:name, :basePay

    def initialize(name, basePay)
        @name = name;
        @basePay = basePay
    end

    def calculatePay(taxRate)
        raise NotImplementedError
    end

    def monthlyBasePay()
        raise NotImplementedError
    end
end

class SalariedEmployee < Employee
    def initialzie(name, basePay)
        super(name, basePay)
    end

    def calculatePay(taxRate)
        return basePay - (basePay * taxRate)
    end

    def monthlyBasePay()
        return basePay
    end
end

class HourlyEmployee < Employee
    ...
end
```

## 변경을 기준으로 선택하라

- 단순히 클래스를 구현 단위로 사용한다는 것이 객체지향 프로그래밍을 한다는 것은 아니다!

    - 타입을 기준으로 절차를 추상화해야 객체지향 분해이다.

    - 클래스 내부에 인스턴스의 타입을 표현하는 변수는 객체지향을 위반하는 것으로 간주

        - 이는 다형성으로 대체할 수 있다.

### 개방-패쇄 원칙

- 기존 코드에 아무런 영향도 미치지 않고 새로운 객체 유형과 행위를 추가할 수 있는 객체지향의 특성

### 객체지향 vs 추상 데이터 타입

- 타입 추가라는 변경의 압력이 더 강한 경우 -> 객체지향 승리

    - 추상 데이터 타입의 경우 -> 타입을 체크하는 클라이언트 코드를 모두 찾아 수정해야 함.

    - 객체지향의 경우 -> 그냥 새로운 클래스를 상속 계층에 추가하기만 하면 됨.

- 변경의 주된 압력이 오퍼레이션 추가 -> 추상 데이터 타입의 승리

    - 객체지향의 경우 새로운 오퍼레이션을 추가하기 위해 -> 상속 계층에 속하는 모든 클래스를 한 번에 수정해야 함.

    - 추상 데이터 타입은 전체 타입에 대한 구현 코드가 하나의 구현체 내에 포함돼 있음 -> 간단

- 변경의 축을 찾아야 한다. 객체지향적인 접근법이 모든 경우에 해결 방법이 아님

> 데이터-주도 설계 : 추상 데이터 타입의 접근법을 객체지향 설계에 구현한 것
