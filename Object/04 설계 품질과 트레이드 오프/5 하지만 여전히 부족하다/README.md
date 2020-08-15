# 하지만 여전히 부족하다

## 캡슐화 위반

- 분명히 수정된 객체들은 자기 자신의 데이터를 스스로 처리한다.

- 하지만 아래 예들로 보자

### 파라미터로 노출

```java
public class DiscountCondition{
    private DiscountconditionType type;
    private int sequence;
    private DayOfWeek dayOfWeek;
    private LocalTime startTime;
    private LocalTime endTime;

    public DiscountConditionType getType() {...}

    public boolean isDiscountable(DayOfWeek dayOfWeek, LocalTime time) {...}

    public boolean isDiscountable(int sequence) {...}
}
```

- isDiscountalbe 메서드들이 파라미터가 있음을 인터페이스를 통해 외부에 노출하고 있음

    - 만약 DiscountCondition의 속성을 변경해야 한다면

        1. 두 isDiscountable 메서드 파라미터 수정

        2. 해당 메서드 사용하는 모든 클라이언트도 함께 수정해야함.

    - 이처럼 내부 구현의 변경이 외부로 퍼져나가는 파급 효과는 캡슐화가 부족하다는 증거

### 파라미터로 노출 x, 메서드 명으로 노출

```java
public class Movie{
    ...

    public Money calculateAmountDiscountedFee() {...}
    public Money calculatePercentDiscountedFee() {...}
    public Money calculateNoneDiscountedFee() {...}
}
```

- DiscountCondition의 isDiscountable 메서드처럼 파라미터를 노출하지는 않음.

- 하지만 할인 정책이 3가지 있다는 정보를 노출시키고 있음.

- 만약 새로운 할인 정책이 추가되거나 제거되면 -> 이 메서드들에 의존하는 모든 클라이언트가 영향을 받음.

## 높은 결합도

```java
public class Movie{
    public boolean isDiscountable(LocalDateTime whenScreened, int sequence){
        for(DiscountCondition condition : movie.getDiscountConditions()){
            if(condition.getType() == DiscountConditionType.PERIOD){
                if(condition.isDiscountable(whenScreened.getDayOfWeek(), whenScreened.toLocalTime())){
                    return true;
                }
            }else{
                if(condition.isDiscountable(sequence)){
                    return true;
                }
            }
        }

        return;
    }
}
```

- discountCondition의 기간 할인 조건의 명칭이 PERIOD에서 다른 값으로 변경 -> Movie 수정해야 함

- DiscountConditon의 종류가 추가되거나 삭제 -> Movie의 if~else 구문 수정

- 각 DiscountCondition의 만족 여부 판단하는 필요한 정보 변경 -> isDiscountalbe 메서드로 변경된 파라미터를 변경해야 함. -> Movie의 isDiscountable 시그니처도 함께 변경 -> Screening도 변경해야함.

## 낮은 응집도

- 할인 조건의 종류를 변경하기 위해 DiscountCondition, Movie, movie를 사용하는 Screening 함께 변경해야 함 -> 여러 곳을 동시 수정해야 한다는 것은 -> 설계의 응집도가 낮다는 증거 