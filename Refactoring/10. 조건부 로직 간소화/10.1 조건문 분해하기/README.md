# 10.1 조건문 분해하기

## 언제

1. 다양한 조건, 그에 따른 복잡한 코드 있을 시

## 절차

1. 조건식과 그 조건식에 딸린 조건절 각각을 함수로 추출(6.1)

    ```javascript
    if(!aDate.isBefore(plan.summerStart) && !aDate.isAfter(plan.summerEnd)){
        charge = quantity * plan.summerRate;
    }else{
        charge = quantity * plan.regularRate + plan.regularServiceCharge; 
    }
    ```

    ```javascript
    // 하나 추출할 때마다 테스트

    if(summer()){
        charge = summerCharge();
    }else{
        charge = regularCharge();
    }

    function summer(){
        return !aDate.isBefore(plan.summerStart) && !aDate.isAfter(plan.summerEnd);
    }

    function summerCharge(){
        return quantity * plan.summerRate;
    }
    
    function regularCharge(){
        return quantity * plan.regularRate + plan.regularServiceCharge;
    }
    ```

> 취향에 따라 아래처럼 바꿀 수 있다.
> ```javascript
> charge = summer() ? summerCharge() : regularCharge();
> ...
> ```