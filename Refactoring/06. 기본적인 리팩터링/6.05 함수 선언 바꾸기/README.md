# 6.5 함수 선언 바꾸기

## 언제

1) 적절한 메서드명으로 바꾸고 싶을 때

2) 매개변수 추가/삭제 하고 싶을 때

## 절차

### 간단한 절차

1) 매개변수 제거시 -> 함수 본문에서 제거 대상 매개변수 참조하는 곳 확인

2) 메서드 선언 원하는 형태로 바꾸기

    ``` javascript
    function circum(radius){
        return 2 * Math.PI * radius
    }
    ```

    ```javascript
    function circumference(radius){
        return 2 * Math.PI * radius;
    }

    ```

3) 기존 메서드 호출하는 부분 모두 찾아 바뀐 형태로 호출

4) 테스트

### 마이그레이션 절차 (복잡한 경우)

- 이름 변경, 매개변수 추가 둘 다 하고 싶을 때

- 상속 구조에 있는 클래스 메서드 변경하고 싶을 때 
    
    - 단일 상속 구조라면, 원하는 형태로 전달 메서드를 슈퍼클래스에 정의하고 시작하면 된다.

1) 이후 과정을 쉽게 하기 위해, 함수의 본문 적절히 리팩터링

2) 함수 본문을 임시 이름을 갖는 새로운 **함수로 추출**

    ```javascript
    function addReservation(customer){
        zz_addReservation(customer);
    }

    function zz_addReservation(customer){
        this._reservations.push(customer);
    }
    ```

3) 추출한 함수에 매개변수 추가해야하면 **간단한 절차** 적용

    ```javascript
    function addReservation(customer){
        zz_addReservation(customer, false);
    }

    function zz_addReservation(customer, isPriority){
        assert(isPriority === true || isPriority === false);
        this._reservations.push(customer);
    }
    ```

4) 테스트

5) 기존 **함수 인라인**하여 호출 코드들이 새 함수 이용하도록 한다.

    ```javascript
    // 클라이언트
    addReservation(aCustomer);
    ```

    ```javascript
    // 클라이언트
    zz_addReservation(customer, false);
    ```

6) 임시 이름을 **함수 선언 바꾸기**를 한번 더 적용하여 원래 이름으로
    
    ```javascript
    function addReservation(customer, isPriority){
        assert(isPriority === true || isPriority === false);
        this._reservations.push(customer);
    }
    ```
    
7) 테스트


## 매개변수 속성으로 바꾸기(마이그레이션)

```javascript
function inNewEngland(aCustomer){
    return ["MA", "CT", "ME", "VT", "NH", "RI"].includes(aCustomer.address.state);
}
```

```javascript
function inNewEngland(aCustomer){
    const State = aCustomer.address.state; // 1. 리팩터링, 변수 추출
    return xxNewinNewEngland(stateCode); 
}

// 2. 함수 추출
function xxNewinNewEngland(stateCode){
    return ["MA", "CT", "ME", "VT", "NH", "RI"].includes(stateCode);
}
```

```javascript
// 함수 인라인 후 함수 선언 바꾸기 적용
function inNewEngland(stateCode){
    return ["MA", "CT", "ME", "VT", "NH", "RI"].includes(stateCode);
}
```
