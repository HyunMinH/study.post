# 6.8 매개변수 객체 만들기

## 언제

1. 데이터 항목 여러 개가 이 함수에서 저 함수로 함께 몰려다닐 때

## 절차

1. 적당한 데이터 구조 없을 시 만들기(ex 클래스)

    ```javascript
    class NumberRange{
        constructor(min,max){
            this._data = {min:min, max:max};
        }
        get min() {return this._data.min;}
        get max() {return this._data.max;}
    }
    ```

2. 테스트
3. 함수 선언 바꾸기 -> 새 데이터 구조를 매개변수로 추가

    ```javascript
    function readingsOutsideRange(station, min, max){
        return station.readins.filter(r => r.temp < min || r.temp > max);
    }
    ```

    ```javascript
    function readingsOutsideRange(station, min, max, range){
        return station.readins.filter(r => r.temp < min || r.temp > max);
    }

    //호출문
    alerts =readingsOutsideRange(station, operatingPlan.temperatureFloor,
        operatingPlan.temperatureCeiling, null);
    ```

4. 테스트
5. 함수 호출 시 새로운 데이터 구조 인스턴스 반환(호출문 하나 수정마다 테스트)

    ```javascript
    const range = new NumberRange(operatingPlan.temperatureFloor,
                        OperatingPlan.temperatureCeiling);

    // 호출문
    alerts =readingsOutsideRange(station, operatingPlan.temperatureFloor,
        operatingPlan.temperatureCeiling, range);
    ```

6. 기존 매개변수 사용 코드 -> 새 데이터 구조의 원소를 사용하도록 바꾸기

    - 하나씩 제거하고 테스트(모두 바꿀 때까지)

    ```javascript
    function readingsOutsideRange(station, min, range){
        return station.readins.filter(r => r.temp < min || r.temp > range.max);
    }
    ```

## 진정한 값 객체로 거듭나기

### 이 리팩토링을 적용하면 관련 동작들을 이 클래스로 옮길 수 있음.

```javascript
function readingsOutsideRange(station, range){
    return station.readinsgs.filter(r => !range.contains(r.temp));
}

// in NumberRange class
contains(arg) { return (arg >= this.min && arg <= this.max); }
```
