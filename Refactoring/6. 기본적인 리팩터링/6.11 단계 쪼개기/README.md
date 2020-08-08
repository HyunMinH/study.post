# 6.11 단계 쪼개기

## 언제

1. 서로 다른 두 대상을 한꺼번에 다루는 코드

## 절차

1. 두 번째 단계에 해당하는 코드 -> 독립 함수로 추출

    ```javascript
    function priceOrder(product, quantity, shippingMethod){
        const basePrice = product.basePrice * quantity;
        const discount = Math.max(quantity - product.discountThreshold, 0) * product.basePrice * product.discountRate;

        const price = applyShipping(basePrice, shippingMethod, quantity, discount);
        return price;
    }

    // 두 번째 단계 처리 함수
    function applyShipping(basePrice, shippingMethod, quantity , discount){
        const shippingPerCase = (basePrice > shippingMethod.discountThreshold) ? shippingMethod.discountedFee : shippingMethod.feePerCase;
        const shippingCost = quantity * shippinPerCase;
        const price = basePrice - discount + shippingCost;
        return price;
    }
    ```

2. 테스트

3. 중간 데이터 구조 만들어 앞에서 추출한 함수의 인수로 추가

    ```javascript
    function priceOrder(product, quantity, shippingMethod){
        ...

        const priceData = {};

        const price = applyShipping(priceData, basePrice, shippingMethod, quantity, discount);
        ...
    }

    // 두 번째 단계 처리 함수
    function applyShipping(priceData, basePrice, shippingMethod, quantity , discount){
       ...
    }
    ```

4. 테스트

5. 추출한 두 번째 단계 함수의 매개변수 하나씩 검토. 그 중 첫 번째 단계에서 사용되는 것은 중간 데이터 구조로 옮김. (옮길 때마다 테스트)

    ```javascript
    function priceOrder(product, quantity, shippingMethod){
        ...

        const priceData = {basePrice : basePrice};

        const price = applyShipping(priceData, shippingMethod, quantity, discount);
        ...
    }

    // 두 번째 단계 처리 함수
    function applyShipping(priceData, shippingMethod, quantity , discount){
       // 원래 basePrice 사용 코드 -> priceData.basePrice로 대치
    }
    ```

    - 간혹 두 번째 단계에서 사용하면 안 되는 매개변수 있음 -> 각 매개변수를 사용한 결과를 중간 데이터 구조의 필드로 추출, 이 필드 값을 설정하는 문장을 호출한 곳으로 옮기기

    ```java
    private static long countOrders(CommandLine commandLine, String[] args, String fileName){
        ...
    }
    ```

    ```java
    // 이 경우에는 args를 처리하는 부분은 모두 1번째 단계로 분리하는 게 목적이기 때문

    static long run(String[] args) throws IOException{
        ...
        CommandLine commandLine = new CommandLine();
        String filename = args[args.length -1];
        commandLine.onlyCountReady = Stream.of(args).anyMatch(arg -> "-r".equals(arg));
    }

    private static long countOrders(CommandLine commandLine, String fileNmae){
        ...
    }
    ```


6. 첫 번째 단계 코드를 함수로 추출하면서 중간 데이터 구조 반환하도록

    ```javascript
    function priceOrder(product, quantity, shippingMethod){
        const priceData = calculatePricingData(product, quantity);
        return applyShipping(priceData, shippingMethod); // + 상수 정리
    }

    // 첫번째 단계 처리 코드
    function calculatePricingData(product, quantity){
        const basePrice = product.basePrice * quantity;
        const discount = Math.max(quantity - product.discountThreshold, 0) * product.basePrice * product.discountRate;
        return {basePrice:basePrice, quantity: quantity, discount:discount};
    }

    // 두 번째 단계 처리 함수
    function applyShipping(priceData, shippingMethod){
       ...
    }
    ```

   - 이 때 첫 번째 단계를 변환기 객체로 추출해도 좋음
  
    ```java
    static long run(String[] args) throws IOException{
        CommandLine commandLine = new CommandLine(args);
        return countOrders;
    }
    ```
