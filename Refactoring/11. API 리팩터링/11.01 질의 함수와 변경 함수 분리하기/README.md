# 11.1 질의 함수와 변경 함수 분리하기

## 언제

1. 값을 반환하면서 부수효과도 있는 함수가 있을 때

    - 단 캐싱의 경우 겉보기에는 어떤 순서로 호출하든 똑같은 값 반환하여 괜찮다.

    - 저자는 100퍼 동의는 안하지만 되도록 따르려고 한다.

    - 나의 생각.. 그러면 추상화가 덜 되는거 아닌가?

## 절차

1. 대상 함수를 복제하고 질의 목적에 충실한 이름 짓기

    ```javascript
    function findMiscreant(people){
        for(const p of people){
            if(p === "조커"){
                setOffAlarms();
                return "조커"
            }
            if(p === "사루만"){
                setOffAlarms();
                return "사루만"
            }
        }
    }
    ```

2. 새 질의 함수에서 부수효과를 모두 제거

    ```javascript
    function findMiscreant(people){
         for(const p of people){
            if(p === "조커"){
                //setOffAlarms();
                return "조커"
            }
            if(p === "사루만"){
                //setOffAlarms();
                return "사루만"
            }
        }
    }
    ```

3. 정적 검사

4. 원래 함수를 호출하는곳 모두 찾기 

   - 반환값 사용한다면 질의 함수를 호출하도록하고, 원래 함수를 호출하는 코드를 바로 아래 줄에 새로 추가 (하나 바꿀 때마다 테스트)

    ```javascript
    const found = alertForMiscreant(people);
    ```

    ```javascript
    const found = findMiscreant(people);
    alertForMiscreant(people);
    ```

5. 원래 코드에서 질의 관련 코드 제거

    ```javascript
    function alertForMiscreant(people){
        for(const p of people){
            if(p === "조커"){
                setOffAlarms();
            }
            if(p === "사루만"){
                setOffAlarms();
            }
        }
    }
    ```

    - 여기서 질의 함수를 사용하도록 알고리즘 교체하기(7.9) 를 적용하면 -> 중북 제거 가능
    ```javascript
    function alertForMiscreant(people){
        if(findMiscreant(people) != "") setAlarms();
    }
    ```

6. 테스트
