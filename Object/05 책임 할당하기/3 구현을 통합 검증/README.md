# 구현을 통한 검증

## 협력의 관점에서 시작

### 상영

- 책임 먼저 결정

```java
public class Screening{
    public Reservation reserve(Customer customer, int audienceCont){

    }
}
```

- 책임 수행하는 데 필요한 인스턴스 변수 결정

```java
public class Screening{
    private Movie movie;
    private int sequence;
    private LocalDateTime whenScreened;

    public Reservation reserve(Customer customer, int audienceCont){

    }
}
```

- 내부 구현하기

    -  구현 도중 Movie에 메시지(협력)을 전송하게 됨

    - 이는 메시지가 객체를 선택하도록 하는 책임 주도 셀계의 방식임!

```java
public class Screening{
    private Movie movie;
    private int sequence;
    private LocalDateTime whenScreened;

    public Reservation reserve(Customer customer, int audienceCont){
        return new Reservation(customer, this, calculate(audienceCount), audienceCount);
    }

    private Money calculateFee(int audienceCount){
        return movie.calculateMovieFee(this).times(audienceCount);
    }
}
```

### 영화

- 책임 먼저 (메시지 응답)

```java
public class Movie{
    public Money calculateMovieFee(Screening screening){

    }
}
```

- 책임 수행하기 위한 인스턴스 변수 설정

```java
public class Movie{
    private String title;
    private Duration runningTime;
    private Money fee;
    private List<DiscountCondition> discountConditions;

    private MovieType movieType;
    private Money discountAmount;
    private double discountPercent;

    public Money calculateMovieFee(Screening screening){

    }
}

public enum MovieType{
    AMOUNT_DISCOUNT,
    PERCENT_DISCOUNT,
    MONE_DISCOUNT
}
```

- 내부 구현

```java
public class Movie{
    ...

    public Money calculateMovieFee(Screening screening){
        if(isCiscountable(screening)){
            return fee.minus(calculateDiscountAmount());
        }

        return fee;
    }

    private boolean isDiscountable(Screening screening){
        return discountConditions.stream().anyMatch(condition -> condition.isSatisfiedBy(screening));
    }

    private Money calculateDiscountAmount(){
        switch(movieType){
            case AMOUNT_DISCOUNT: return calculateDiscountAmount();
            case PERCENT_DISCOUNT : return calculatePercentDiscountAmount();
            case NONE_DISCOUNT: return calculateNoneDiscountAmount();
        }
        throw new IllegalStateException();
    }

    private Money calculateDiscountAmount(){
        return discountAmount;
    }

    private Money calculatePercentDiscountAmount(){
        return fee.times(discountPercent);
    }

    private Money calculateNoneDiscountAmount(){
        return Money.ZERO;
    }
}
```

### 할인 조건

- 책임(메시지 수신) 부터

```java
public class DiscountCondition{
    public boolean isSatisfiedBy(Screening screening){

    }
}
```

- 책임 수행 위한 필요 인스턴스 변수

```java
public class DiscountCondition{
    private DiscountConditionType type;
    private int sequence;
    private DayOfWeek dayOfWeek;
    private LocalTime startTime;
    private LocalTime endTime;

    public boolean isSatisfiedBy(Screening screening){

    }
}
```

- 내부 구현
```java
public class DiscountCondition{
    ...

    public boolean isSatisfiedBy(Screening screening){
        if(type == DiscountConditionType.PERIOD){
            return isSatisfiedByPeriod(screening);
        }

        return isSatisfiedBySequence(screening);
    }

    private boolean isSatisfiedByPeriod(Screening screening){
        ...
    }

    private boolean isSatisfiedBySequence(Screening screening){
        ...
    }
}
```

- DiscountConditon은 할인 조건 판단 위해 Screening의 상영 시간과 상영 순번 알아야 함

  - Screening에 아래 메서드들을 추가

```java
public class Screening{
    public LocalDateTime getWhenScreened(){
        return whenScreened;
    }

    public int getSequence(){
        return sequence;
    }
}
```

## DiscountCondition 개선하기

### 살펴보기

- 변경에 취약하다 = 변경의 이유가 다양하다 (응집도 낮음)

    - 새로운 할인 조건 추가 

      - isSatisfiedBy 메서드 안의 if ~ else 구문 수정해야 함.

      - 새로운 할인 조건이 새로운 데이터 요구하면 DiscountConfition에 속성도 추가해야 함

    - 순번 조건을 판단하는 로직 변경

        - isSatisfiedBySequence 메서드 내부 구현을 수정해야 한다.

        - 순번 조건을 판단하는 데 필요한 데이터가 변경되면 DiscountCondition의 sequence 속성 역시 변경돼야 한다.

    - 기간 조건 판단하는 로직 변경되는 경우

        - isSatisfiedByPeriod 메서드 내부 구현을 수정해야 하며

        - 기간 조건을 판단하는 데 필요한 데이터가 변경되면 DiscountCondition의 dayOfWeek, startTime, endTime 속성 역시 변경해야 한다.

### 클래스 응집도 판단

- 클래스가 하나 이상의 이유로 변경되야 한다면 응집도가 낮은 것 -> 변경의 이유를 기준으로 클래스를 분리하라.

- 클래스의 인스턴스를 초기화하는 시점에 경유에 따라 서로 다른 속성들을 초기화하고 있다면 응집도가 낮다는 것 -> 초기화되는 속성의 그룹을 기준으로 클래스를 분리

- 메서드 그룹이 속성 그룹을 사용하는지 여부로 나뉜다면 응집도가 낮다는 것 -> 이들 그룹을 기준으로 클래스를 분리

## 타입 분리하기

- 순번 조건과 기간 조건이라는 두 개의 독립적인 타입이 하나의 클래스 안에 공존 -> 두 개의 클래스로 분리

```java
public class PeriodCondition{
    private DayOfWeek dayOfWeek;
    private LocalTime startTime;
    private LocalTime endTime;

    public PeriodCondition(DayOfWeek dayOfWeek, LocalTime startTime, LocalTime endTime){
        ...
    }

    public boolean isSatisfiedBy(Screening screening){
        ...
    }
}

public class SequenceCondition{
    private int sequence;

    public SequenceCondition(int sequence){
        ...
    }

    public boolean isSatisfiedBy(Screening screening){
        ...
    }
}
```

- 하지만 위처럼 분리하면 Movie는 협력하는 클래스가 DiscountCondition 하나에서

- SequenceCondition, PeriodCondition이라는 두 개의 서로 다른 클래스의 인스턴스 모두와 협력할 수 있어야 한다.

- 응집도는 높아졌지만 변경과 캡슐화 관점에서 보면 전체적으로 설계 품질이 나빠진다.

## 다형성을 통해 분리하기

### Polymorphism Pattern

- 객체의 타입에 따라 변하는 로직이 있을 때 변하는 로직을 담당할 책임을 어떻게 할당할 까?

    - 타입을 명시적으로 정의하고 각 타입에 다형적으로 행동하는 책임을 할당하기

- if~else문, switch등의 조건 논리르 사용해 설계한다면 새로운 변화가 일어난 경우 조건 논리를 수정해야 함 -> 이 패턴 사용하자

- 역할을 사용하여 객체의 구체적인 타입을 추상화할 수 있다.

    - 구현 공유하고 싶다 -> 추상 클래스 사용

    - 역할을 대체하는 객체들의 책임만 정의하고 싶다 -> 인터페이스를 사용

### 개선된 코드

```java
public interface DiscountCondition{
    boolean isSatisfiedBy(Screening screening);
}

public PeriodCondition implements DiscountCondition{
    ...
}

public SequenceCondition implements DiscoutnCondition{
    ...
}
```



## 변경으로부터 보호하기

### Protected Variations Pattern

- 설계에서 변하는 것이 무엇인지 고려하고 변하는 개념을 캡슐화하라

    - PeriodCondition과 SequenceCondition은 서로 다른 이유로 변경된다.

    - 서로 다른 변경이 두 개의 서로 다른 클래스 안으로 캡슐화된다.

### Protected Variations Pattern과 Polymorphism Pattern

- 하나의 클래스가 여러 타입의 행동을 구현하는 것처럼 보인다면

    - 클래스를 분해하고 Polymorphism 패턴에 따라 책임을 분산시켜라

- 예측 가능한 변경으로 인해 여러 클래스들이 불안정해진다면

    - Protected Variation Pattern에 따라 안정적인 인터페이스 뒤로 변경을 캡슐화 하라

## Movie 개선하기

### 어떻게

- 금액 할인 정책 영화와 비율 할인 정책 영화라는 두 가지 타입 -> 하나 이상의 변경 이유 -> 응집도 낮음

- Polymorphism Pattern 사용해 서로 다른 행동을 타입별로 분리

- Protected Variation Patter을 이용해 타입의 종류를 안정적인 인터페이스 뒤로 캡슐화 -> Screening에 영향 안 미치게

### 개선된 코드

```java
public abstract class Movie{
    private String title;
    private Duration runningTime;
    private Money fee;
    private List<DiscountCondition> discountConditions;

    public Movie(String title, Duration runningTime, Money fee, Discountcondition... discountConditons){
        ...
    }

    public Money calculateMovieFee(Screening screening){
        if(isDiscountable(screening)){
            return fee.minus(calculateDiscountAmount());
        }
        return fee;
    }

    private boolean isDiscountable(Screening screening){
        ...
    }

    // PercentDiscoutnMovie에서 쓰기 위해 추가
    public abstract class Movie{
        return fee;
    }

    abstract protected Money calculateDiscountAmount();
}

public class AmountDiscountMovie extends Movie{
    private Money discountAmount;

    public AmountDiscountMovie(String title, Duration runningTime, Money fee, Money discountAmount, DiscountConditon ... discountConditions){
        ...
    }

    @Override
    protected Money calculateDiscountAmount(){
        return discountAmount;
    }
}

public class PercentDiscountMovie extends Movie{
    private double percent;

    public PercentDiscountMovie(String title, Duration runningTime, Money fee, double percent, DiscountCondition ... discountconditions){
        ...
    }

    @Override
    protected Money calculateDiscountAmount(){
        return getFee().times(percent);
    }
}

public class NoneDiscountMovie extends Movie{
    public NoneDiscountMovie(String title, Duration runnningTime, Money fee){
        super(title,runningTime, fee);
    }

    @Override
    protected Money calculateDiscountAmount(){
        return Money.ZERO;
    }
}
```

## 변경과 유연성

- 새로운 할인 정책이 일어날 때 Movie는 더 복잡해짐. 새로운 서브 클래스 작성해야 함. 의존성도 높고

    - 상속 대신 합성을 사용하라

- 도메인 모델은 코드에 대한 가이드를 제공할 수 있어야 하며 코드의 변화에 발맞춰 함께 변화하는 것임.