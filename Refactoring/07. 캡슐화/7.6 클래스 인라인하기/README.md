# 7.6 클래스 인라인하기

## 언제

1. 특정 클래스에 남은 역할이 거의 없을 때

2. 두 클래스의 기능을 지금과 다르게 배분하고 싶을 때

    - 하나로 합친 다음 다시 클래스 추출하기(7.5)가 쉬울수도 있으므로

## 절차

1. 소스 클래스의 각 public 메서드 -> 타깃 클래스(더 많이 사용하는 클래스)에 생성.

    - 이 메서드들은 단순히 작업을 소스 클래스로 위임해야 함.

    ```javascript
    // in Shipment Class
    // TrackingInformation의 코드를 위임
    set shippingCompany(arg) {this._trackingInformation.shippingCompany = arg;}
    ```

2. 소스 클래스 메서드 사용 코드 -> 타킷 클래스 위임 메서드 사용하도록 (하나 바꿀 때마다 테스트)

    ```javascript
    // client
    aShipment.trackingInformation.shippingCompany = request.vendor;
    ```

    ```javascript
    // client
    aShipment.shippingCompany = request.vendor;
    ```

3. 소스 클래스의 메서드 필드 -> 타깃 클래스로 (하나 바꿀 때마다 테스트)

    ```javascript
    // in Shipment Class
    get trackingInfo(){
        return '${this.shippingCompany}: ${this.trackingNumber}'};
    }
    ```

4. 소스 클래스 삭제

    ```javascript
    // TrackingInformation Class 삭제
    ```
