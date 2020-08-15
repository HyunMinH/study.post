# 8.1 함수 옮기기

## 언제

1. 어떤 함수가 자신이 속한 모듈 A의 요소들 보다 다른 모듈 B의 요소들을 더 많이 참조한다면

## 절차

1. 선택한 함수가 현재 컨텍스트에서 사용중인 모든 프로그램 요소 살펴본다 -> 이 요소들 중에도 같이 옮길게 있는지 확인

    - 호출되는 함수 중 함께 옮길게 있음 -> 그 함수부터

    - 하위 함수들의 호출자가 고수준 함수 하나뿐 -> 하위 함수를 인라인 후 -> 옮긴 후 다시 추출

2. 선택한 함수가 다형 메서드인지 확인

3. 선택한 함수를 타깃 컨텍스트로 복사

    ```javascript
    function trackSummary(points){
        ...
    }

    // trackSummary의 중첩함수를 최상위로 옮기기
    function calculateDistance(){
        let result = 0;
        for(let i=1; i<points.length; i++){
            result += distance(points[i-1], points[i]);
        }
        return result;

        function distance(p1, p2) {...} // 단계 1에 해당하는 같이 옮겨야하는 함수 -> 여기서는 중첩함수로 함께 옮김
        function radians(degrees) {...} // 마찬가기
    }
    ```

    - 함수 본문에서 소스 컨텍스트의 요소 사용 -> 해당 요소들을 매개변수 or 소스 컨텍스트 자체를 참조로 넘겨주기

    - 필요시 옮긴 함수를 이름 바꾸기

    ```javascript
    // 최상위 함수에 파라미터 추가 & 이름 바꾸기
    function top_calculateDistance(points){
        ...
    }
    ```

4. 정적 분석

5. 소스 컨텍스트에서 타깃 함수를 참조할 방법 찾아 반영

6. 소스 함수를 타깃 함수의 위임 함수가 되도록 수정

    ```javascript
    function trackSummary(points){
        ...
        function calculateDistance(){
            return top_calculateDistance(points);
        }
    }
    ```

7. 테스트

8. 소스 함수를 인라인할지 고민

    ```javascript
    function trackSummary(points){
        ...
        const pace = totalTime / 60 / top_calculateDistance(points);
        return {
            time : totalTime,
            distance : top_calculateDistance(points),
            pace : pace
        }

        // 위임 함수 삭제
        // function calculateDistance(){
        //    return top_calculateDistance(points);
        //}
    }
    ```

> 소스 컨텍스트에서 가져와야 할 데이터가 많다면
> 소스 자체의 참조를 넘긴다.
