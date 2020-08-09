# 10.3 중첩 조건문을 보호 구문으로 바꾸기

## 언제

1. if-else 구문에서 둘 중 하나가 정상 동작으로 이어지지 않을 때

    - 핵심 로직을 더 부각할 수 있으며, 더 가독성이 좋아짐

## 절차

1. 교체해야 할 조건 중 가장 바깥 것을 선택하여 보호 구문으로 바꾼다.

    ```javascript
    function payAmount(employee){
        let result;
        if(employee.isSeparated){
            result = {amount: 0, reasonCode: "SEP"};
        }else{
            ...
        }
        return result;
    }
    ```

    ```javascript
    function payAmount(employee){
        if(employee.isSeparated) return {amount: 0, reasonCode: "SEP"};
        ...
    }
    ```

2. 테스트한다.

3. 1~2 과정을 필요한 만큼 반복한다.

4. 모든 보호 구문이 같은 결과를 반환한다면 -> 보호 구문들의 조건식을 통합(10.2)한다.

## 조건 반대로 만들기

- 이 리팩터링을 수행할 때는 조건식을 반대로 만들어 적용하는 경우도 많다.

    ```javascript
    function adjustedCapital(anInstrument){
        let result = 0;
        if(anInstrument.capital > 0){
            if(anInstrument.interestRate > 0 && anInstrument.duration > 0){
                result = ...;
            }
        }
        return result;
    }
    ```

    ```javascript
    function adjustedCapital(anInstrument){
        if(anInstrument.capital < 0) return 0;
        if(anInstrument.interestRate <= 0 || anInstrument.duration <=0) return 0;

        return ...;
    }
    // 이경우에 두 보호조건이 모두 0을 반환함 -> 10.2 적용 가능
    ```