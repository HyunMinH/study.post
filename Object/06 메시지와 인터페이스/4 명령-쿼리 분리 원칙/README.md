# 명령-쿼리 분리 원칙

## 왜 필요한가

- 가끔씩은 필요에 따라 물어야 한다.

## 정의

### 명령 (프로시저)

- 부수효과를 발생시킬 수 있지만 값을 반환할 수 없음

### 쿼리 (함수)

- 값을 반환할 수 있지만 부수효과를 발생시킬 수 없다.

### 이를 지킨다는 것은

- 질문이 답변을 수정해서는 안된다.

- 명령이 상태를 변경할 수 있지만 상태를 반환해서는 안된다.

## 명령-쿼리 분리와 참조 투명성

- 명령과 쿼리를 분리함으로써 -> 명령형 언어의 틀 안에서 참조 투명성의 장점을 제한적이나마 누릴 수 있게 된다.

    - 객체의 부수효과를 제어하기가 수월해진다.

        - 쿼리는 객체의 상태를 변경하지 않기 때문이고 몇 번이고 반복 호출해도 상관 x

        - 쿼리의 결과를 예측하기 쉬워진다.

        - 명령이 개입하지 않는 한 쿼리들의 순서를 자유롭게 변경할 수 있다.

## 책임에 초점을 맞춰라

- 1~3 원칙을 따르며 설계하는 아주 쉬운 방법은 

  - 메시지를 먼저 선택하고 그 후에 메시지를 처리할 객체를 선택하는 것

- 4 원칙을 따르고 계약에 의한 설계 개념을 통해 객체의 협력 방식을 명시적으로 드러내는 방법은

    - 객체의 구현 이전에 객체 사이의 협력에 초점을 맞추고 협력 방식을 단순하고 유연하게 만드는 것이다.

### 즉 모든 방식의 중심(훌룡한 메시지를 얻기 위해서)에는 객체가 수행할 책임이 위치한다.

> 이번 장에서 배웠던 원칙은 좋은 지침을 제공하지만 실제로 실행 시점에 필요한 구체적인 제약이나 조건을 명확하게 표현하지는 못한다.
> - 계약에 의한 설계 개념을 참고(부록 A)한다면 코드 상에 명시적으로 표현하고 강제하는 팁들을 알 수 있다..