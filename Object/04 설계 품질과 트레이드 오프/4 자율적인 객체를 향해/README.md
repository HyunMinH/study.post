# 자율적인 객체를 향해

## 캡슐화를 지켜라

- 캡슐화는 설계의 제 1원리

- 아례 얘는 책임을 이동하여 캡슐화를 강화하였음

```java
class AnyClass{
    void anyMethod(Rectangle rectangle, int multiple){
        rectangle.setRight(rectangle.getRight() * multiple);
        rectangle.setBottom(rectangle.getBottom() * multiple);
        ...
    }
}
```

```java
class Rectangle{
    ...
    public void enlarge(int multiple){
        right *= multiple;
        bottom *= multiple;
    }
}
```

## 스스로 자신의 데이터를 책임지는 객체

- 객체를 설계할 때 "이 객체를 어떤 데이터를 포함해야 하는가" 라는 질문 쪼개기

    1. 이 객체가 어떤 데이터를 포함해야 하는가

    2. 이 객체가 데이터에 대해 수행해야 하는 오퍼레이션은 무엇인가

### 할인 조건에 적용

```java
public class DiscountCondition{
    public DiscountConditiontype getType(){
        return type;
    }

    public boolean isDiscountable(DayOfWeek dayOfWeek, LocalTime time){
        ...
    }

    public boolean isDiscountalbe(int sequence){
        ...
    }
}
```

### 영화에 적용
```java
public class Movie{
    public MovieType getMovieType(){
        return movieType;
    }

    public Money calculateAmountDiscountedFee(){
        ...
    }

    public Money calculatePercentDiscountedFee(){
        ...
    }

    public Money calculateNoneDiscountedFee(){
        ...
    }

    public boolean isDiscountable(LocalDateTime whenScreened, int sequence){
        ...
    }
}
```

### Screening에 적용

```java
public class Screening{
    public Money calculateFee(int audienceCount){
        ...
    }
}
```

### ReservationAgency 변화

```java
public class ReservationAgency{
    public Reservation reserve(Screening screening, Customer customer, int audienceCount){
        Money fee = screening.calculateFee(audiencecount);
        return new Reservation(customer, screening, fee, audienceCount);
    }
}
```