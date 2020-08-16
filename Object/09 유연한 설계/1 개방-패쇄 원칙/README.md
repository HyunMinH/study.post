# 개방-폐쇄 원칙

## 뜻

- 소프트웨어 개체(클래스, 모듈, 함수 등등)은 확장에 대해 열려 있어야하고, 수정에 대해서는 닫혀 있어야 한다.

- 즉 코드를 수정하지 않고도 새로운 동작을 추가할 수 있어야 한다.

## 컴파일타임 의존성을 고정시키고 런타임 의존성을 변경하라

- 이를 통하면 새로운 클래스를 추가하는 것만으로 확장할 수 있다.

## 추상화가 핵심이다.

- 이 원칙의 핵심은 **추상화에 의존**하는 것이다.

- 하지만 단순히 어떤 개념을 추상화했다고 해서 수정에 대해 닫혀 있는 설계를 만들 수 있는 것은 아니다.

- 바로 의존성의 방향이다. 수정에 대한 영향을 최소화하기 위해서는 모든 요소가 추상화에 의존해야 한다.

    - 아래 코드에서는 DiscountPolicy에만 의존한다.

    ```java
    public class Movie{
        ...
        private DiscountPolicy discountPolicy;

        public Movie(String title, Duration runningTime, Money fee, DiscountPolicy discountPolicy){
            ...
            this.discount = discountPolicy
        }

        public Money calculateMovieFee(Screening screening){
            return fee.minus(discountPolicy.calculateDiscountAmount(screening));
        }
    }
    ```
