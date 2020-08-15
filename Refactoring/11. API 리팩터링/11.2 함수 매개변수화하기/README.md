# 11.2 함수 매개변수화하기

## 언제

1. 두 함수의 로직이 아주 비슷하고 단지 리터럴 값만 다를 때

## 절차

1. 비슷한 함수 중 하나를 선택

    ```javascript
    function bottomBand(usage){
        return Math.min(usage, 100);
    }

    function middleBand(usage){
        return usage > 100 ? Math.min(usage, 200) - 100 : 0;
    }

    function topBand(usage){
        return usage > 200 ? usage - 200;
    }
    ```

    - 위 예처럼 범위를 다루는 로직에서는 대개 중간에 해당하는 함수에서 시작하는게 좋음

2. 함수 선언 바꾸기(6.5)로 리터럴들을 매개변수로 추가

    ```javascript
    function withinBand(usage, bottom, top){
        return usage > 100 ? Math.min(usage, 200) - 100 : 0;
    }
    ```

3. 이 함수를 호출하는 곳 모두에 적절한 리터럴 값 추가

    ```javascript
    ...
    const amount = bottomBand(usage) * 0.03;
                + withinBand(usage, 100, 200) * 0.05
                + topBand(usage) * 0.07
    ```

4. 테스트

5. 매개변수로 받은 값을 사용하도록 함수 본문 수정. (하나 수정마다 테스트)

    ```javascript
    function withinBand(usage, bottom, top){
        return usage > bottom ? Math.min(usage, top) - bottom: 0;
    }
    ```

6. 비슷한 다른 함수를 호출하는 코드 찾아 -> 매개변수화된 함수를 호출하도록 (하나 수정마다 테스트), 원래 함수 제거

    ```javascript
    ...
    const amount = withinBand(usage, 0, 100) * 0.03
                + withinBand(usage, 100, 200) * 0.05
                + withinBand(usage, 200, Infinity) * 0.07;

    // function topBand(usage){ ... }
    // function bottomBand(usage) { ... }
    ```

    - 매개변수화된 함수가 대체할 비슷한 함수와 다르게 동작 

    - -> 그 비슷한 함수 동작도 처리 가능케 본문 코드를 적절히 수정 후 진행