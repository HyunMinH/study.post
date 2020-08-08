# 6.10 여러 함수를 변환 함수로 묶기

## 언제

1. 도출된 정보가 사용되는 곳마다 같은 도출 로직이 반복되기도 함 -> 도출 작업들을 모아놓기 위해 -> 검색 갱신 일관된 장소에서 처리 및 로직 중복 방지

2. 원본 데이터가 코드 안에서 갱신될 때 -> 클래스로 묶는게 나음.

## 절차

1. 변환할 레코드를 입력받아서 값을 그대로 반환하는 변환 함수 만들기

    - 대체로 깊은 복사로 처리해야 함 -> 불변성 유지하기 위해(데이터 일관성)

    ```javascript
    function enrichReading(original){
        const result = _.cloneDepp(original);
        return result;
    }
    ```

2. 묶을 함수 중 함수 하나를 골라서 본문 코드를 변환 함수로 옮기고, 처리 결과를 레코드에 새 필드로 기록 -> 클라이언트가 이 필드 사용하도록 수정

    - 로직 복잡할 시 -> 함수 추출부터

    ```javascript
    function enrichReading(original){
        const result = _.cloneDeep(original);
        result.baseCharge = calculateBaseCharge(result);
        return result;
    }

    // client
    const rawReading = acquireReading();
    const aReading = enrichReading(rawReading);
    const basicChargeAmount = aReading.baseCharge;
    ```

3. 테스트

4. 나머지 관련 함수도 위 과정에 따라 처리

    ```javascript
    function enrichReading(original){
        const result = _.cloneDeep(original);
        result.baseCharge = calculateBaseCharge(result);
        result.taxableCharge = Math.max(0, result.baseCharge - taxTreshold(result.year));
        return result;
    }

    // client2
    const rawReading = acquireReading();
    const aReading = enrichReading(rawReading);
    const taxableCharge = aReading.taxableCharge;
    ```
