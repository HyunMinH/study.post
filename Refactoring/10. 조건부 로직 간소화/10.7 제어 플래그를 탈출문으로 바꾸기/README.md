# 10.7 제어 플래그를 탈출문으로 바꾸기

## 언제

1. 제어 플래그가 있을 때

    - 무조건 악취로 본다 -> 리팩터링으로 충분히 간소화할 수 있음

## 절차

1. 제어 플래그를 사용하는 코드를 함수로 추출(6.1)할지 고민

    ```javascript
    checkForMiscreants(people);
    ...

    function checkForMiscreants(people){
        let found = flase;
        for(const p of people){
            ...
        }
    }
    ```

2. 제어 플래그를 갱신하는 코드 각각을 적절한 제어문(return, break, continue)으로 바꾼다. (하나 바꿀 때마다 테스트)

    ```javascript
    function checkForMiscreants(people){
        let found = false;
        for(const p of people){
            if(!found){
                if(p === "조커"){
                    sendAlert();
                    // found = true <- return 으로 바꿔주기
                    return; 
                }
                if(p === "사루만"){
                    sendAlert();
                    // found = true <- 마찬가지
                    return;
                }
            }
        }
    }
    ```

3. 모두 수정했다면 -> 제어 플래그 제거

    ```javascript
    function checkForMiscreants(poeple){
        // let found = false
        for(const p of people){
            //if(!found) {
                if(p === "조커"){
                    sendAlert();
                    return;
                }
                if(p === "사루만"){
                    sendAlert();
                    return;
                }
            //}
        }
    }
    ```

> 추가로 더 가다듬기
> ```javascript
> function checkForMiscreants(people){
>   if(["조커", "사루만"].isDisjointWith(people)) sendAlert();
>   // 실제로 위와 같은 코드는 자바스크립트에 없음. 
>   // 대신 다른 파이프라인 연산으로 하면 됨.
> }
> ```