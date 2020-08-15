# 11.4 객체 통째로 넘기기

## 언제

1. 하나의 레코드에서 값 두어 개를 가져와 인수로 넘기는 코드가 있을 때

2. 하지만 함수가 레코드 자체에 의존하기 원치 않을 때는 수행 X

3. 어떤 객체로부터 값 몇 개를 얻은 후 그 값들만으로 무언가를 하는 로직이 있다면 -> 그 로직을 객체 안으로 집어넣어야 함을 알려주는 악취로 봐야 함.

4. 한 객체가 제공하는 기능 중 항상 똑같은 일부만을 사용하는 코드가 많다면 -> 그 기능만 따로 묶어서 클래스로 추출하라는 신호일 수 있음

## 절차

1. 매개변수들을 원하는 형태로 받는 빈 함수를 만든다.

    ```javascript
    // in HeatingPlan Class
    xxNEWwithinRange(aNumberRange){

    }
    ```

2. 새 함수의 본문에서는 원래 함수를 호출하도록 하며, 새 매개변수와 원래 함수의 매개변수를 매핑

    ```javascript
    // in HeatingPlan Class
    xxNEWwithinRange(aNumberRange){
        return this.withinRange(aNumberRange.low, aNumberRange.high);
    }
    ```

3. 정적 검사

4. 모든 호출자가 새 함수를 사용하게 수정 (하나 수정마다 테스트)

    ```javascript
    const low aRoom.daysTempRange.low;
    const high aRoom.daysTempRange.high;
    if(!aPlan.xxNEWwithinRange(aRoom.daysTempRange)) 
    ...
    ```

    * 죽은 코드 만들어 지면 죽은 코드 제거(8.9);

    ```javascript
    //const low aRoom.daysTempRange.low;
    //const high aRoom.daysTempRange.high;
    if(!aPlan.xxNEWwithinRange(aRoom.daysTempRange)) 
    ...
    ```

5. 호출자 모두 수정했다면 -> 원래 함수를 인라인

    ```javascript
    xxNEWwithinRange(aNumberRange){
        return (aNumberRange.low >= this._temperatureRange.low)
            && (aNumberRange.high <= this.+temperatureRange.high);
    }
    ```

6. 새 함수의 이름을 적절히 수정 후 모든 호출자에 반영

> 새 함수를 다른 방식으로 만들기