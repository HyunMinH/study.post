# 인터페이스와 설계 품질

## 좋은 인터페이스

### 달성해야 하는 2가지

- 최소한의 인터페이스

- 추상적인 인터페이스

### 책임 주도 설계

- 위 2가지를 둘 다 충족하는 가장 좋은 방법임

### 퍼블릭 인터페이스의 품질에 영향을 미치는 원칙

- 책임 주도 설계 방법이 훌룡한 인터페이스르 얻을 수 있는 지침을 제공

- 하지만 훌룡한 인터페이스가 가지는 공통적인 특징을 아는 것은 안목을 넓히고 올바른 설계에 도달 할 수 있는 지름길을 제공할 것

- 아래에서 4가지 원칙을 배울 것

## 1. 디미터 법칙

- 협력하는 객체의 내부 구조에 강하게 결합되지 않도록 경로를 제한하자

    - 오직 하나의 도트만 사용하라

- 아래 조건을 만족하는 인스턴스에만 메시지를 전송하자

    - this 객체

    - 메서드의 매개변수

    - this의 속성

    - this의 속성인 컬렉션의 요소

    - 메서드 내에서 생성된 지역 객체

- 코드 개선

    - ReservationAgency가 Screening의 내부 Movie 객체와도 결합돼 있고

    - ReservationAgency가 Movie의 내부 DiscountConditon과도 결합돼 있고

```java
// in ReservationAgency Class
screening.getMovie().getDiscountConditions();
```

```java
// in ReservationAgency Class
screening.calculateFee(audienceCount);
```

## 2. 묻지 말고 시켜라

- 메시지 전송자는 메시지 수신자의 상태를 기반으로 결정을 내린 후 메시지 수신자의 상태를 바꿔서는 안된다. 이는 캡슐화 위반이다.

    - 상태를 묻는 오퍼레이션을 행동을 요청하는 오퍼레이션으로 대체하자

  - 앞에서 요금을 계산하는 데 필요한 정보를 잘 알고 있는 Screening에게 요금을 계산할 책임을 할당함.

- 이 원칙을 따르면 밀접하게 연관된 정보와 행동을 함께 가지는 객체를 만들 수 있다. -> 응집도 높아짐

## 3. 의도를 드러내는 인터페이스

- 메서드의 이름을 지을 때 **어떻게**가 아니라 **무엇**을 하는지를 드러내자

    - 그렇지 않으면 메서드 수준에서 캡슐화를 위반할 수 있음. (클라이언트로 하여금 협력하는 객체의 종류를 알도록 강요하기 때문)

- 하나의 구현을 가진 메시지의 이름을 일반화하도록 도와주는 훈련

    1. 매우 다른 두 번째 구현을 상상하라

    2. 해당 메서드에 동일한 이름을 붙인다고 상상하자

    3. 그러면 그 순간 할 수 있는 가장 추상적인 이름을 메서드에 붙일 것이다.

- 어떻게를 드러내는 코드

```java
public class PeriodCondition{
    public boolean isSatisfiedByPeriod(Screening screening) {...}
}

public class SequenceCondition{
    public boolean isSatisfiedBySequence(Screening screening) {...}
}
```

- 무엇을 하는지 드러내는 코드
```java
public class PeriodCondition{
    public boolean isSatisfiedBy(Screening screening) {...}
}

public class SequenceCondition{
    public boolean isSatisfiedBy(Screening screening) {...}
}
```

> 책의 예제를 통해 1~3 원칙들을 적용하는 것을 볼 수 있다.

## 4. 명령-쿼리 분리

- 이는 따로 중단원에서 다룸
