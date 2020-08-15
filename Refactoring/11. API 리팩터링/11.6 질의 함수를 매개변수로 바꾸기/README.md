# 11.6 질의 함수를 매개변수로 바꾸기

## 언제

1. 함수 안에서 전역 변수를 참조한다거나, 제거하길 원하는 참조하는 경우

    - 보통 의존관계를 바꾸려할 때 자주 일어남

- 단점은 호출자가 어떤 값을 함수에게 건네줘야할지 알아야한다는 점! (자율성 떨어짐)

## 절차

1. 변수 추출하기(6.3)로 질의 코드를 함수 본문의 나머지 코드와 분리

    ```javascript
    get targetTemperature(){
        const selectedTemperature = thermostat.selectedTemperature;

        if(seletedTemperature > this._max) return this._max;
        else if(selectedTemperature < this._min) return this._min;
        else return seletedTemperature;
    }
    ```

2. 함수 본문 중 해당 질의를 호출하지 않는 코드들을 별도 함수로 추출(6.1)

    ```javascript
    get targetTemperature(){
        const selectedTemperature = thermostat.selectedTemperature;
        return this.xxNEWtargetTemparature(selectedTemperature);
    }

    xxNEWtargetTemperature(selectedTemperature){
        if(seletedTemperature > this._max) return this._max;
        else if(selectedTemperature < this._min) return this._min;
        else return seletedTemperature;
    }
    ```

3. 방금 만든 변수를 인라인(6.4)하여 제거

    ```javascript
    get targetTemperature(){
        return  this.xxNEWtargetTemperature(thermostat.selectedTemperature);
    }
    ```

4. 원래 함수도 인라인(6.2)한다.

    ```javascript
    // 호출자
    if(thePlan.xxNEWtargetTemperature(thermostat.selectedTemperature)
        > thermostat.currentTemperature)
        setToHeat();

    else if(thePlan.xxNEWtargetTemperature(thermostat.selectedTemperature)
        < thermostat.currentTemperature)
        setToCool();
        
    else
        setOff();
    ```

5. 새 함수의 이름을 원래 함수의 이름으로 고쳐준다.

    ```javascript
    // 호출자
    if(thePlan.targetTemperature(thermostat.selectedTemperature)
        > thermostat.currentTemperature)
        setToHeat();
    else if(thePlan.targetTemperature(thermostat.selectedTemperature)
        < thermostat.currentTemperature)
        setToCool();
    else
        setOff();


    targetTemperature(selectedTemperature){
        if(seletedTemperature > this._max) return this._max;
        else if(selectedTemperature < this._min) return this._min;
        else return seletedTemperature;
    }
    ```