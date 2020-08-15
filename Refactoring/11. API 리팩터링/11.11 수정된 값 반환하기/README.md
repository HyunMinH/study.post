# 11.11 수정된 값 반환하기

## 언제

1. 데이터를 갱신될 것임을 분명히 하고 싶을 때

## 절차

1. 함수가 수정된 값을 반환하게 하여 호출자가 그 값을 자신의 변수에 저장하게 함.

    ```javascript
    //호출자
    totalAscent = calculateAscent();

    //함수
    fucntion calculateAscent(){
        ...
        return totalAscent;
    }
    ```

2. 테스트

3. 피호출 함수 안에 반환할 값을 가리키는 새로운 변수를 선언

    ```javascript
    function calculateAscent(){
        let totalAscent = 0;
        ...
        return totalAscent;
    }
    ```

4. 테스트

5. 계산이 선언과 동시에 이뤄지도록 통합

    - 가능하면 불변으로 지정

    ```javascript
    // 호출자
    const totalAscent = calculateAscent();
    ```

6. 테스트

7. 피호출 함수의 변수 이름을 새 역할에 어울리도록 바꿔 줌

    ```javascript
    function calculateAscent(){
        let result = 0;
        ...
        return result;
    }
    ```

8. 테스트
