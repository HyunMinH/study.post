# 데이터 중심의 영화 예매 시스템

## 개요

- 데이터가 아닌 책임에 초점을 맞춰야 하는 이유는?

    - 객체의 상태는 구현에 속함 -> 구현은 불안정하여 변하기 쉬움

    - 상태를 객체 분할의 중심축으로 삼으면 구현에 관한 세부사항이 객체의 인터페이스에 스며들게 되어 캡슐화 원칙 깨짐.

    - 자연스레 변경에 취약할 수 밖에

    - 책임에 초점 맞추면 구현 변경에 대한 파장이 외부로 퍼져나가는 것을 방지 -> 상대적으로 안정적인 설계 얻을 수 있음.

## 데이터를 준비하자

- 객체가 내부에 저장해야 하는 데이터가 무엇인가를 묻는 것으로 시작한다.

### 영화
```java
public class Movie{
    private String title;
    private Duration runningTime;
    private Money fee;
    private List<DiscountCondition> discountConditions;

    private MovieType movietype;
    private Money discountAmount;
    private double discountPercnet; 
    
    ... // 위 필드의 접근자와 수정자들
}

public enum MovieType{
    AMOUNT_DISCOUNT,
    PERCENT_DISCOUNT,
    NONE_DISCOUNT
}
```

### 할인 조건
```java
public enum DiscountConditionType{
    SEQUENCE, PERIOD
}

public class DiscountCondition{
    private DiscountConditionType type;

    private int sequence;
    private LocalTime startTime;
    private LocalTime endTime;

    ... // 위 필드에 따른 접근자, 수정자들
}
```

### 상영
```java
public class Screening{
    private Movie movie;
    private int sequence;
    private LocalDateTime whenScreened;

    ... // 접근자, 수정자들
}
```

### 예매
```java
public class Reservation{
    private Customer customer;
    private Screening screening;
    private Money fee;
    private int audienceCount;

    ... // 접근자, 수정자들
}
```

### 고객
```java
public class Customer{
    private String name;
    private String id;

    public Customer(String name, Stirng id){
        this.id = id;
        this.name = name;
    }
}
```

## 영화를 예매하자

### 영화 예매 절차 구현

```java
public class ReservationAgency{
    public Reservation reserve(Screening screening, Customer customer, int audienceCount){
        Movie movie = screening.getMove();

        boolean discountable = false;
        for(DiscountCondition condition : movie.getDiscountConditions()){
            if(condition.getType() == DiscountConditionType.PERIOD){
                discountable = screening.getWhenScreened().getDayOfWeek().equals(condition.getDayOfWeek()) 
                    && condition.getStartTime().compareTo(screening.getWhenScreened().toLocalTime()) <= 0
                    && condition.getEndTime().compareTo(screening.getWhenScreened().toLocalTime()) >= 0;
            }else{
                discountable = condition.getSequence() == screening.getSequence();
            }

            if(discountalbe) break;
        }

        Money fee;
        if(discountalbe){
            Money discountAmount = Money.ZERO;
            switch(movie.getMovieType()){
                case AMOUNT_DISCOUNT: discountAmount = movie.getDiscountAmount();
                    break;
                case PERCENT_DISCOUNT: discountAmount = movie.getFee().times(movie.getDiscountPercent());
                    break;
                case NONE_DISCOUNT: discountAmount = Money.ZERO;
                    break;
            }

            fee = movie.getFee().minus(discountAmount);
        }else{
            fee = movie.getFee();
        }

        return new Reservation(customer, screening, fee, audienceCount);
    }
}
```