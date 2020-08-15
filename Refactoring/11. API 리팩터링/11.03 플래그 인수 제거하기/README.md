# 11.3 플래그 인수 제거하기

## 언제

1. 함수에서 플래그 인수에 따라 실행하는 로직이 나눠질 때

    * 특히 boolean 사용시 호출자 코드를 알아보기 힘듬

2. 단 플래그 인수가 2개 이상일 때는 만들어야 하는 함수 조합이 많아짐.

    - 하지만 이는 한 함수가 너무 많은 일을 처리하고 있다는 신호임

    - 같은 로직을 조합해내는 더 간단한 함수를 만들 방법 고민해야 함

## 절차

1. 매개변수로 주어질 수 있는 값 각각에 대응하는 명시적 함수들을 생성

    - 주가 되는 함수에 깔끔한 분배 조건문 포함되어 있다면 

    - -> 조건문 분해하기(10.1)로 명시적 함수들을 생성

    ```javascript
    function deliveryDate(anOrder, isRush){
        if(isRush) return runDeliveryDate(anOrder);
        else return regularDeliveryDate(anOrder);
    }

    function rushDeliveryDate(anOrder){
        let deliveryTime;
        ...
    }

    function regularDeliveryDate(anOrder){
        let deliveryTime;
        ...
    }
    ```

    - 아니면 래핑 함수 형태로 만들기

2. 원래 함수를 호출하는 코드 -> 각 리터럴 값에 대응하는 명시적 함수를 호출하도록

    ```javascript
    // 호출자
    aShipment.deliverydate = rushDeliveryDate(anOrder)
    ```

    * 모든 호출 대체했다면 원래 함수 제거