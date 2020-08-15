# 11.5 매개변수를 질의 함수로 바꾸기

## 언제

1. 피호출 함수가 스스로 쉽게 결정할 수 있는 값을 매개변수로 건넬 때

    - 물론 그러면 피호출 함수가 값 결정을 책임져야 하므로 -> 피호출 함수가 그 역할을 수행하기에 적절할 때만

2. 매개변수 제거로 인해 -> 피호출 함수에 원치 않는 의존성이 생길 때는 리팩터링 X

## 절차

1. 필요하다면 대상 매개변수 값 계산하는 코드를 별도 함수로 추출(6.1)

    ```javascript
    get finalPrice(){
        const basePrice = this.quantity * this.itemPrice;
        return this.discountedPrice(basePrice, this.discountLevel);
    }

    // 이 경우 임시 변수를 질의 함수로 바꾸기(7.4)
    get discountLevel(){
        return (this.quantity > 100) ? 2 : 1;
    }


    ```

2. 함수 본문에서 대상 매개변수로의 참조 모두 찾아 -> 그 매개변수의 값을 만들어주는 표현식을 함조하도록 바꾼다. (하나 수정마다 테스트)

    ```javascript
    discountedPrice(basePrice, discountLevel){
        switch(this.discountedLevel){ // <- discountLevel에서 바꿈
            case 1 : return basePrice * 0.95;
            case 2 : return basePrice * 0.9;
        }
    }
    ```

3. 함수 선언 바꾸기(6.5)로 대상 매개변수를 없앤다

    ```javascript
    get finalPrice(){
        const basePrice = this.quantity * this.itemPrice;
        return this.discountedPrice(basePrice);
    }

    discountedPrice(basePrice, /* discountLevel */){
        switch(this.discountedLevel){
            case 1 : return basePrice * 0.95;
            case 2 : return basePrice * 0.9;
        }
    }
    ```
