# 10.2 조건식 통합하기

## 언제

1. 비교하는 조건은 다르지만 그 결과로 수행하는 동작은 똑같을 때

## 절차

1. 해당 조건식들 모두에 부수효과 없는지 확인

    - 있을 시 -> 질의 함수와 변경 함수 분리하기(11.1) 먼저 적용

2. 조건문 두 개를 선택하여 -> 두 조건문의 조건식들을 논리 연산자로 결합

    ```javascript
    function disabilityAmount(anEmplyee){
        if((anEmployee.seniority < 2) 
            || (anEmployee.monthsDisabled > 12)) return 0;
        if(anEmployee.isPartTime) return 0;
    }
    ```

3. 테스트

4. 조건이 하나 남을 때까지 2~3 반복

    ```javascript
    function disabilityAmount(anEmployee){
        if((anEmployee.seniority < 2) 
            || (anEmployee.monthsDisabled > 12)
            || (anEmployee.isPartTime)) return 0;
    }
    ```

5. 함수로 추출(6.1)할지 고려

    ```javascript
    function disabilityAmount(anEmployee){
        if(isNotEligibleForDisability()) return 0;
    }

    function isNotEligibleForDisability(){
        return ((anEmployee.seniority < 2) 
            || (anEmployee.monthsDisabled > 12)
            || (anEmployee.isPartTime));
    }
    ```