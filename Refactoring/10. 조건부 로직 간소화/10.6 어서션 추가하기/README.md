# 10.6 어서션 추가하기

# 언제

1. 특정 조건이 참일 때만 제대로 동작하는 코드 영역이 있을 때

2. 또한 오류 찾기, 어떤 상태임을 가정하고 실행되는지 좋은 소통 도구가 됨.

## 절차

1. 참이라고 가정하는 조건이 보이면 그 조건을 명시하는 어서션을 추가

    ```javascript
    applyDiscount(aNumber){
        if(!this.discountRate) return aNumber;
        assert(this.discountRate >= 0);
        ...
    }

    set discountRate(aNumber){
        assert(null === aNumber || aNumber >= 0)
        ...
    }
    ```