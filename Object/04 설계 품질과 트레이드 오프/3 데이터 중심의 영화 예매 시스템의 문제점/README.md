# 데이터 중심의 영화 예매 시스템의 문제점

## 캡슐화 위반

```java
public class Movie{
    private Money fee;

    public Money getFee(){
        return fee;
    }

    public void setFee(Money fee){
        this.fee = fee;
    }
}
```

- 얼핏 보면 직접 객체의 내부에 접근할 수 없기 때문에 캡슐화의 원칙을 지키고 있는 것처럼 보이지만 아니다.

    - getFee 메서드와 setFee 메서드는 Movie 내부에 Money 타입의 fee라는 이름의 인스턴스 변수가 존재한다는 사실을 퍼블릭 인터페이스에 노골적으로 드러낸다.

- 데이터 중심 설계의 경우 -> 객체가 사용될 문맥을 추축할 수 밖에 없는 경우 -> 개발자는 어떤 상황에서도 해당 객체가 사용할 수 있게 최대한 많은 접근자 메서드를 추가하게 됨.

    - 이처럼 접근자와 과도하게 의존하는 설계 방식을 -> 추축에 의한 설계 전략이라고 함.

## 높은 결합도

```java
public class ReservationAgency{
    public Reservation reserve(Screening screening, Customer customer, int audienceCount){
        ...
        Money fee;
        if(discountable){
            ...
            fee = movie.getFee().minus(discountedAmount).times(audienceCount);
        }else{
            fee = movie.getFee();
        }
    }
}
```

- 위 코드를 보면 fee의 타입을 변경한다고 가정했을 때

    - getFee 메서드의 반환 타입도 수정해야 하며

    - getFee를 호출하는 ReservationAgency의 구현도 변경된 타입에 맞게 수정해야 함.

- 원래 코드를 보면 ReservationAgency가 모든 데이터 객체에 의존한다는 것을 알 수 있다.

- 또 예를 들면 DiscountCondition의 데이터가 변경되면 DiscountCondition 뿐만 아니라 ReservationAgency도 함께 수정해야 한다.

## 낮은 응집도

- ReservationAgency의 코드를 수정해야 하는 경우
    
    - 할인 정책이 추가될 경우

    - 할인 정책별로 할인 요금을 계산하는 방법이 변경될 경우

    - 할인 조건이 추가되는 경우

    - 할인 조건별로 할인 여부를 판단하는 방법이 변경될 경우

    - 예매 요금을 계산하는 방법이 변경될 경우

- 이처럼 서로 다른 이유로 변경되는 코드가 하나의 모듈 안에 공존함 -> 낮은 응집도

- 낮은 응집도의 문제

    1. 변경의 이유가 서로 다른 코드들을 하나의 모듈 안에 뭉쳐놓았기 때문에 변경과 아무 상관이 없는 코드들이 영향을 받게 됨.

    2. 하나의 요구사항 변경을 반영하기 위해 동시에 여러 모듈을 수정해야함. 
        - 응집도가 낮을 경우 다른 모듈에 위치해야 할 책임의 일부가 엉뚱한 곳에 위치하게 되기 때문

### 단일 책임 원칙

- 클래스는 단 한가지의 변경 이유만 가져야 한다.

- 이 원칙을 통해 클래스의 응집도를 높일 수 있다.
