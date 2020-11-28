# 6.4 변수 인라인하기

## 언제

1) 변수의 이름이 원래 표현식과 다를 바 없을 때

2) 변수가 주변 코드를 리팩터링하는데 방해가 될 때

## 절차

1) 표현식에서 부작용 생기는지 확인

2) 대입값 한번만 적용되는지 확인하기 위해, 불변으로 만든 후 테스트

    ```javascript
    const basePrice = anOrder.basePrice;
    ...
    ```

3) 이 변수 가장 처음 사용하는 곳 -> 대입문 우변의 코드로 바꾸기

    ```javascript
    const basePrice = anOrder.basePrice;
    return (basePrice > 1000);
    ```

    ```javascript
    const basePrice = anOrder.basePrice;
    return anOrder.basePrice > 1000;
    ```

4) 테스트

5) 변수 사용하는 곳 모두 3,4 반복

6) 변수 선언문 및 대입문 삭제

    ```javascript
    return anOrder.basePrice > 1000;
    ```

7) 테스트
