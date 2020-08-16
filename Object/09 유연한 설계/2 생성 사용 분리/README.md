# 생성 사용 분리

## 뭔가

- 추상화에 의존하기 위해서는 구체 클래스의 인스턴스를 생성해서는 안 된다.

    - 결합도가 높아질수록 개방-패쇄 원칙을 따르는 구조를 설계하기가 어려워짐

- 생성과 사용을 분리하자 (한가지만 하자)

```java
public class Client{
    public Money getAvatarFee(){
        Movie avatar = new Movie("아바타", Duration.ofMinutes(120), Money.wons(10000), new AmountDiscountPolicy(...));
        
        return avatar.getFee();
    }
}
```

## Factory 추가하기

- 위 코드의 문제점은 Movie가 DiscountPolicy를 직접 생성하지 않아 Movie는 개방-폐쇄 원칙을 지킬 수 있지만 client가 Movie의 생성과 사용의 책임을 함께 지니고 있게 되었다.

- 객체 생성과 책임만 전담하는 별도의 객체를 추가하고 Client가 이 객체를 사용하도록 하자

    - 이를 통해 Client는 오직 사용과 관련된 책임만 지고 생성과 관련된 어떤 지식도 가지지 않을 수 있다.

```java
public class Factory{
    public Movie createAvatarMovie(){
        return new Movie("아바타", Duration.ofMinutes(120), Money.wons(10000), new AmountDiscountPolicy(...));
    }
}

public class Client{
    private Factory factory;

    public Client(Factory factory){
        this.factory = factory;
    }

    public Movie getAvatarFee(){
        Movie avatar = factory.createAvatarMovie();
        return avatar.getFee();
    }
}
```

## 순수한 가공물에게 책임 할당하기

- 위 Factory는 원래의 도메인 모델에 속하지 않는다. 이 Factory를 추가한 이유는 순수하게 기술적인 결정이다. 

- 이처럼 전체적으로 결합도를 낮추고 재사용성을 높이기 위해, 도메인 개념에게 할당돼 있던 객체 생성 책임을 -> 도메인 개념과 아무런 상관이 없는 가공의 객체로 이동시킨 것


> 시스템을 객체로 분해하는 큰 두 가지 방식
>    - 표현적 분해
>
>        - 도메인에 존재하는 사물 또는 개념을 표현하는 객체들을 이용해 시스템을 분해하는 것
>
>        - 이는 객체지향 설계를 위한 가장 기본적인 접근법
>
>    - 행위적 분해
> 
>       - 이를 통해 Pure Fabrication을 생성할 수 있음.

- 모든 책임을 도메인 객체에게 할당하면 낮은 응집도, 높은 결합도, 재사용성 저하와 같은 심각한 문제점에 봉착하게 될 가능성이 높아진다.

    - 도메인과 무관한 인공적인 객체(Pure Fabrication)에 책임을 할당하여 문제를 해결해야 한다.

### Pure Fabrication Pattern

- 어떤 객체가 책임을 수행하는 데 필요한 많은 정보를 가졌지만 해당 책임을 할당할 경우 응집도가 낮아지고 결합도가 높아진다면 -> 가공의 객체를 추가하여 책임을 옮기는 것을 고민하라.

- Information Expert Pattern에 따라 책임을 할당한 결과가 바람직하지 않을 때 대안으로 사용됨.
