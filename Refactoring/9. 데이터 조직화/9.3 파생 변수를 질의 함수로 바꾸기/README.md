# 9.3 파생 변수를 질의 함수로 바꾸기

## 언제

1. 값을 쉽게 계산해낼 수 있는 변수 있을 시

## 절차

1. 변수 값 갱신되는 지점 모두 찾기 -> 필요시 변수 쪼개기(9.1)로 각 갱신 지점에서 변수 분리

    ```javascript
    // in ProductionPlan Class
    constructor(production){
        this._production = production;
        this._adjustments = [];
    }

    get production() {return this._production;}

    applyAdjustment(anAdjustment){
        this._adjustments.push(anAdjustment);
        this._production += anAdjustment.amount;
    }
    ```

    ```javascript
    constructor(production){
        this._initialProduction = production;
        this._productionAccumlator = 0;
        this._adjustments = [];
    }

    get production(){
        return this._initialProduction + this._productionAccumulator;
    }

    applyAdjustment(anAdjustment){
        ...
    }
    ```

2. 해당 변수의 값을 계산해주는 함수 만들기

    ```javascript
    get calculatedProductionAccumulator(){
        return this._adjusments.reduce((sum, a) => sum + a.amount, 0);
    }
    ```

3. 해당 변수 사용되는 모든 곳 -> 어서션 추가하여 -> 함수 계산값 == 변수 값 인지 확인

    ```javascript
    get production(){
        assert(this._productionAccumulator == this.calculatedProductionAccumulator);
        return this._initialProduction + this._productionAccumulator;
    }
    ```

4. 테스트

5. 변수 읽는 코드를 -> 모두 함수 호출로

    ```javascript
    get production(){
        return this.initialProduction + this.calculatedProductionAccumulator;
    }
    ```

6. 테스트

7. 변수 선언하고 갱신하는 코드를 없애기(8.9)

    ```javascript
    ...
    applyAdjustment(anAdjustment){
        this._adjustments.push(anAdjustment);
        //this._production += anAdjustment.amount;
    }
    ```