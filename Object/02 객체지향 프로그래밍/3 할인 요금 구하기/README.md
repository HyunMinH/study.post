# 할인 요금 구하기

## 할인 요금 계산을 위한 협력 시작

### 영화
```java
public class Movie{
    private String title;
    private Duration runningTime;
    private Money fee;
    private DiscountPolicy discountPolicy;

    ...

    public Money calculateMovieFee(Screening screening){
        return fee.minus(discountPolicy.calculateDiscountAmount(screening));
    }
}
```

- 위 코드에는 상속과 다형성이 숨겨져 있다. <- 이 둘의 기반에는 추상화라는 원리가 숨겨져 있음. 차례대로 살펴보자

## 할인 정책과 할인 조건

### 할인 정책

- 기본적인 알고리즘의 흐름을 구현하고 중간에 필요한 처리를 자식 클래스에게 위임하는 Template Method Pattern이 쓰였다.

```java
public abstract class DiscountPolicy{
    private List<DiscountCondition> conditions = new ArrayList<>();

    public DiscountPolicy(DiscountCondition ... conditions){
        this.conditions = Arrays.asList(conditions);
    }

    public Money calculateDiscountAmount(Screening screening){
        for(DiscountCondition each : conditions){
            if(each.isSatisfiedBy(screening)){
                return getDiscountAmount(screening);
            }
        }

        return Money.ZERO;
    }

    abstract protected Money getDiscountAmount(Screening screening);
}
```

- 일정한 금액을 할인해주는 할인 정책

```java
public class AmountDiscountPolicy extends DiscountPolicy{
    private Money discountAmount;

    public AmountDiscountPolicy(Money discountAmount, DiscountCondition ... conditions){
        super(conditions);
        this.discountAmount = discountAmount;
    }

    @Override
    protected Money getDiscountAmount(Screening screening){
        return discountAmount;
    }
}
```

- 일정 비율을 차감해주는 할인 정책

```java
public class PercentDiscountPolicy extends DiscountPolicy{
    private double percent;

    public PercentDiscountPolicy(double percent, DiscountCondition ... conditions){
        super(conditions);
        this.percent = percent;
    }

    @Override
    protected Money getDiscountAmount(Screening screening){
        return screening.getMovieFee().times(percent);
    }
}
```

### 할인 정책
```java
public interface DiscountCondition{
    boolean isSatisfiedBy(Screening screening);
}
```

- 순번 할인 조건
```java 
public class SequenceCondition implements DiscountCondition{
    private int sequence;

    public SequenceCondition(int sequence){
        this.sequence = sequence;
    }

    public boolean isSatisfiedBy(Screening screening){
        return screening.isSequence(sequence);
    }
}
```

- 기간 할인 조건
```java
public class PeriodCondition implements DiscountCondition{
    private DayOfWeek dayOfWeek;
    private LocalTime startTime;
    private LocalTime endTime;

    public PeriodCondition(DayOfWeek dayOfWeek, LocalTime startTime, LocalTime endTime){
        this.dayOfWeek = dayOfWeek;
        this.startTime = startTime;
        this.endTime = endTime;
    }

    public boolean isSatisfiedBy(Screening screening){
        ...
    }
}
```