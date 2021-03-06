# 유연성에 대한 조언

## 유연한 설계는 유연성이 필요할 때만 옳다.

- 유연한 설계의 이면에는 복잡한 설계라는 의미가 숨어 있다.

    - 불필요한 유연성은 불필요한 복잡성을 낳는다.

## 협력과 책임이 중요하다.

- 결국 객체의 협력과 책임이 중요하다.

- 설계를 유연하게 만들기 위해서 먼저 역할, 책임, 협력에 초점을 맞춰야 한다.

    - 다양한 컨텍스트에서 협력을 재사용할 필요가 없다면 -> 설계를 유연하게 만들 당위성도 사라진다.

- 즉, 핵심은 객체를 생성하는 방법에 대한 결정은 모든 책임이 자리를 잡은 후 가장 마지막 시점에 내리는 것이 적절하다는 것이다.
