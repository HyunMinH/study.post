# 6.9 여러 함수를 클래스로 묶기

## 언제

1. 공통 데이터를 중심으로, 긴밀하게 엮여 작동하는 함수 무리

## 절차

1. 함수들이 공유하는 공통 데이터 레코드를 캡슐화

   - 공통 테이터가 레코드 구조 x -> 6.8로 데이터를 하나로 묶는 레코드 만들기

    ```javascript
    class Reading{
        constructor(data){
            this._customer = data.customer;
            this._quantity = data.quantity;
            this._month = data.month;
            this._year = data.year;
        }

        get customer() {return this._customer;}
        get quantity() {return this._quantity;}
        get month() {return this._month;}
        get year() {return this._year;}
    }
    ```

2. 공통 레코드를 사용하는 함수 각각을 새 클래스로 옮긴다.

   - 공통 레코드의 멤버는 함수 호출문의 인수목록에서 제거

    ```javascript
    // client
    const aReading = acquireReading();
    const basicchargeAmount = baseCharge(aReading);

    function baseCharge(aRdading){
        return baseRate(aReading.month, aReading.year) * aReading.quantity;
    }
    ```

    ```javascript
    // in Reading Class
    get baseCharge(){
        return baseRate(this.month, this.year) * this.quantity;
    }

    // client
    const rawReading = aquireReading();
    const aReading = new Reading(rawReading);
    const basicChargeAmount = aReading.baseCharge;
    ```

3. 데이터를 조작하는 로직들 함수 추출 -> 새 클래스로 옮김

    ```javascript
    // client 2
    const rawReading = aquireReading();
    const aReading = new Reading(rawReading);
    const taxableCharge = Math.max(0, aReading.baseCharge - taxThreshold(aReading.year));
    ```

    ```javascript
    // in Reading class
    get taxableCharge(){
        return Math.max(0, this.baseCharge - taxThreshold(this.year));
    }

    const rawReading = aquireReading();
    const aReading = new Reading(rawReading);
    const taxableCharge = aReading.taxableCharge;
    ```
