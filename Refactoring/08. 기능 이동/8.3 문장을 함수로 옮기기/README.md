# 8.3 문장을 함수로 옮기기

## 언제

1. 특정 함수 호출 코드들의 앞 뒤에, 똑같은 코드가 추가로 실행되는 모습이 보일 때

## 절차

1. 반복 코드가 함수 호출 부분과 멀리 떨어져 있음 -> 문장 슬라이드로 근처로 옮기기

2. 타깃 함수를 호출하는 곳이 한 곳뿐 -> 소스 위치에서 해당 코드를 잘라내 피호출 함수로 옮기고 테스트. -> 이후 과정 무시

3. 호출자가 둘 이상 -> 호출자 중 하나에서 타깃 함수 호출 부분과 그 함수로 옮기려는 문장들을 함께 -> 다른 함수로 추출

    ```javascript
    function photoDiv(p){
        return [
            "<div>",
            '<p> 제목 : {$p.title}</P>'
            emitPhotoData(p),
            "</div>"
        ].join("\n");
    }
    ```

    ```javascript
    function photoDiv(p){
        return[
            "<div>",
            zznew(p); // 임시 이름
            "</div>"
        ]
    }

    function zznew(p){
        return[
            '<p> 제목 : {$p.title}</P>'
            emitPhotoData(p)
        ].join("\n");
    }
    ```

4. 다른 호출자 모두가 방금 추출한 함수 사용토록 수정 (하나 수정마다 테스트)

    ```javascript
    function renderPerson(outStream,person){
        ...
        result.push(zznew(person.photo));
        ...
    }
    ```

5. 원래 함수를 새로운 함수 안으로 인라인 한후 -> 원래 함수 제거

    ```javascript
    function zznew(p){
        return[
            '<p> 제목 : {$p.title}</P>'
            '<p> 위치 : {$p.location}</P>'
            '<p> 날짜 : {$p.date.toDateString()}</P>'
        ].join("\n");
    }
    ```

6. 함수 이름을 바꾸기(6.5)

    ```javascript
    function emitPhotoData(aPhoto){
         return[
            '<p> 제목 : {$p.title}</P>'
            '<p> 위치 : {$p.location}</P>'
            '<p> 날짜 : {$p.date.toDateString()}</P>'
        ].join("\n");
    }

    // 호출자 1
    function photoDiv(aPhoto){
        return[
            "<div>",
            emitPhotoData(aPhoto) // 임시 이름
            "</div>"
        ]
    }

    // 호출자 2
    function renderPerson(){
        ...
        result.push(emitPhotoData(person.photo));
        ...
    }
    ```