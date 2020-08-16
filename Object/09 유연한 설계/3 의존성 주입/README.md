# 의존성 주입

## 뭔가

- 사용하는 객체가 아닌 외부의 독립적인 객체가 인스턴스를 생성한 후 이를 전달하여 의존성을 해결하는 방법

- 세 가지 방법

    - 생성자 주입

    - setter 주입

    - aptjem wndlq

## 숨겨진 의존성은 나쁘다.

- 의존성 주입외에서 의존성을 해결할 수 있는 다양한 방법이 존재한다.

### 1. Service Locator Pattern

- 의존성을 해결할 객체들을 보관하는 일종의 저장소

- 이를 통해 서비스를 사용하는 코드로부터 서비스가 누구인지(서비스를 구현한 구체 클래스의 타입이 무엇인지), 어디에 있는지(클래스 인스턴스를 어떻게 얻을지)를 몰라도 되게 해준다.

- 하지만 이 패턴은 큰 단점이 의존성을 숨긴다는 것이다. (캡슐화 위반)

    - 만약 client에서 provide를 호출하는 것을 까먹었다고 해보자. Movie가 discountPolicy에게 메시지를 전송했을 때 NullPointException Error가 날 수 있다.

- 하지만 의존성 주입 대신 이 패턴을 써야할 때도 있다.

    - 의존성 주입을 지원하는 프레임워크를 사용하지 못하는 경우

    - 깊은 호출 계층에 걸쳐 동일한 객체를 계속 전달해야 할 때

```java
public class ServiceLocator{
    private static ServiceLocator soleInstance = new ServiceLocator();
    private DiscountPolicy discountPolicy;

    public static DiscountPolicy discountPolicy(){
        return soleInstance.discountPolicy;
    }

    public static void provide(DiscountPolicy discountPolicy){
        soleInstance.discountPolicy = discountPolicy;
    }

    private ServiceLocator(){

    }
}

public class Move{
    ...
    private DiscountPolicy discountPolicy;

    public Movie(String title, Duration runningTime, Money fee){
        ...
        this.discountPolicy = ServiceLocator.discountPolicy();
    }
}

// client
ServiceLocator.provide(new AmountDiscountPolicy(...));
Movie avatar = new Movie("아바타", Duration.ofMinutes(20), Money.wons(10000));
```