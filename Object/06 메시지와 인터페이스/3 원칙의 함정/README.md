# 원칙의 함정

## 잊지말아야 할 사실

- 원칙이다. 법칙이 아님 -> 예외가 많다는 것

- 설계가 트레이드오프의 산물이라는 것

- 원칙이 현재 상황에 부적합하다고 판단되면 과감하게 원칙을 무시하라

## 디미터 법칙은 하나의 도트를 강제하는 규칙이 아니다.

- 스트림 연산은 동일한 클래스의 인스턴스를 반환함 -> 디미터 법칙 위반이 아님.

```java
IntStream.of(1,15,20,3,9).filter(x -> x > 10).distinct().count();
```

- 하나 이상의 토트를 사용하는 코드라도 객체의 내부 구현에 대한 어떤 정보도 외부로 노출하지 않는다면 -> 디미터 법칙을 준수한다는 것

## 결합도와 응집도의 충돌

- 디미터 법칙과 묻지 말고 시켜라를 준수하는 것이 항상 긍정적인 결과로 귀결되는 것이 아니다.

    - 맹목적으로 위임 메서드를 추가하다보면 같은 퍼블릭 인터페이스 안에 어울리지 않는 오퍼레이션들이 공존하게 된다.

    - 서로 상관 없는 책임들을 떠안게 되고

    - 여러 개의 변경 이유를 가져 응집도가 낮아진다.

- 가끔식은 묻는 것 외에는 다른 방법이 존재하지 않는 경우도 존재한다.

    - 컬렉션에 포함된 객체들을 처리할 때 객체에게 물어볼 수 밖에 없음.

## 경우에 따라 다르다!
