# 7.4 임시 변수를 질의 함수로 바꾸기

## 언제

1. 여러 곳에서 똑같은 방식으로 계산되는 변수

2. 보통 클래스 안이 좋다. 클래스 바깥의 최상위 함수로 추출 시 -> 매개변수가 너무 많아져 함수 사용 장점이 줄어들기 때문

## 절차

1. 변수가 사용되기 전 값이 확실히 결정되는지, 변수 사용때마다 계산 로직이 매번 다른 결과 내지 않는지 확인

    - 

2. 읽기 전용으로 만들 수 있는 변수는 읽기 전용으로

3. 테스트

4. 변수 대입문을 함수로 추출

    ```javascript
    // in Order Class
    get price(){
        const basePrice = this.basePrice;
        ...
    }

    // 추출한 함수
    get basePrice(){
        return this._quantity * this._item.price;
    }
    ```

    - 추출한 함수가 부수 효과 있다면 -> 질의 함수와 변경 함수 분리하기로 대처

5. 테스트

6. 변수 인라인하기로 임시 변수 제거

    ```javascript
    get price(){
        // const basePrice = this.basePrice; <- 삭제
        var discountFactor = 0.98;
        if(this.basePrice > 1000) discountFactor -= 0.03;
        return this.basePrice * discountFactor;
    }
    ```
