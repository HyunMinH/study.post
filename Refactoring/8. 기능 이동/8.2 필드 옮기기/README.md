# 8.2 필드 옮기기

## 언제

1. 현재 데이터 구조가 적절치 않음을 깨달을 때

    - 어떤 레코드를 넘길 때마다 또 다른 레코드의 필드도 함께 넘기고 있을 때

    - 한 레코드를 변경하려 할 때 다른 레코드의 필드까지 변경해야 한다면

## 절차

1. 소스 필드 캡슐화 x -> 캡슐화

2. 테스트

3. 타깃 객체에 필드(와 접근자 메서드들)를 생성

    ```javascript
    // in CustomerContract
    construct(..., discountRate){
        ...
        this._discountRate = discountRate;
    }

    ...
    get discountRate() {return this._discountRate;}
    set discountRate(arg) {this._discountRate = arg;}
    ```

4. 정적 검사

5. 소스 객체에서 타깃 객체 참조할 수 있는지 확인

    - 기존 필드나 메서드 중 타깃 객체 넘겨주는 게 없음

    - -> 이른 기능의 메서드 쉽게 만들 수 있는지 봄

    - -> 그것도 쉽지 않다면 타깃 객체를 필드로 저장!

6. 소스의 접근자들이 타깃 필드 사용하도록 수정

    ```javascript
    // in Customer Class
    construct(name, discountRate){
        ...
        this._contract = new CustomerContract(dateToday());
        this._setDiscountRate(discountRate);
    }

    get discountRate() {return this._contract.discountRate;}
    set discountRate(arg) {this._contract.discountRate = arg;}
    ```

7. 테스트

8. 소스 필드 제거

9.  테스트

## 날 레코드 변경하기

> 이 리팩터링은 대체로 객체를 활용할 때 가 더 수월(<- 캡슈로하 덕에 데이터 접근을 메서드로 자연스레 감싸주기 때문) 

> 날 레코드를 여러 함수가 직접 옮겨주는 상황에 이 리팩터링 적용하려 할 때 -> 캡슐화부터 먼저 하고 적용하는게 수월하다.