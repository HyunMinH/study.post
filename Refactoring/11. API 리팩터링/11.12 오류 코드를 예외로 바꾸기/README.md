# 11.12 오류 코드를 예외로 바꾸기

## 언제

1. 정상 동작 범주에 들지 않는 오류가 나타낼 때만

    - 판단 기준은 프로그램 종료 코드로 바꿔도 프로그램이 여전히 정상 작동하는지 따져보기

    - 만약 정상 작동하지 않을 것 같다면 -> 예외로 바꾸지 말기

## 절차

1. 콜스택 상위에 해당 예외를 처리할 예외 핸들러를 작성

    - 처음에는 모든 예외를 다시 던지게 해둠.

    - 적절한 처리를 해주는 핸들러 있다면 -> 지금의 콜스택도 처리할 수 있도록 확장

    ```javascript
    //최상위
    let status;
    try{
        status = calculateShippingCoists(orderData);
    }catch(e){
        throw e;
    }
    if(stats < 0) errorList.push({order: orderData, errorCode: status});
    ```

2. 테스트

3. 해당 오류 코드를 대체할 예외와 그 밖의 예외를 구분할 식별 방법 찾기

    - 대부분 언어에서는 서브클래스 사용

    ```javascript
    class OrderProcessingError extends Error{
        constructor(errorCode){
            super('주문 처리 오류 ${errorCode}');
            this.code = errorCode;
        }
        get name() { return "OrderProcessingError"; }
    }
    ```

4. 정적 검사 수행

5. catch절을 수정하여 직접 처리할 수 있는 예외는 적절히 대처하고 그렇지 않은 예외는 다시 던짐.

    ```javascript
    //최상위
    let status;
    try{
        status = calculateShippingCoists(orderData);
    }catch(e){
        if(e instanceof OrderProcessingError)
            errorList.push({order: orderData, errorCode: e.code});
        else
            throw e;
    }
    if(stats < 0) errorList.push({order: orderData, errorCode: status});

    ```

6. 테스트

7. 오류 코드를 반환하는 곳 모두에서 예외를 던지도록 수정 (하나 수정마다 테스트)

    ```javascript
    function localShippingRules(country){
        ...
        // return -23;
        throw new OrderProcessingError(-23);
    }
    ```

8.  모두 수정했다면 그 오류 코드를 콜스택 위로 전달하는 코드를 모두 제거 (하나 수정마다 테스트)

    - 오류를 콜스택 위로 전달하는 일은 예외 메커니즘이 대신 처리해줄 것이기 때문

    ```javascript
    function calculateShippingCosts(anOrder){
        ...
        const shippingRules = localShippingRules(anOrder.country);
        // if(shippingRules < 0) throw new Error("오류 코드가 다 사라지지 않았습니다")
        ...
    }

    // 최상위
    //let status;
    try{
        calculateShippingCoists(orderData);
    }catch(e){
        if(e instanceof OrderProcessingError)
            errorList.push({order: orderData, errorCode: e.code});
        else
            throw e;
    }
    //if(stats < 0) errorList.push({order: orderData, errorCode: status});
    ```