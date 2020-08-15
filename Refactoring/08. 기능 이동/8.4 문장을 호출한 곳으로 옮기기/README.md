# 8.4 문장을 호출한 곳으로 옮기기

## 언제

1. 함수가 어느새 둘 이상의 다른 일을 수행할 때

    - 일부 호출자에게는 다르게 동작하도록 바뀌어야 할 때

## 절차

1. 호출자가 한두 개분이고 피호출 함수도 단순한 상황에 

    - 피호출 함수의 처음(혹은 마지막) 잘라내어 호출자로 복붙(필요시 적절히 수정)

    - 테스트 -> 통과시 다음 단계 무시

2. 더 복잡한 상황 -> 이동하지 않길 원하는 모든 문장을 함수로 추출

    ```javascript
    function emitPhotoData(outStream, photo){
        zztmp(outStream, photo); // 남길 원하는 데이터
        outStream.write('<p> 위치 : {$p.location}</P>');
    }

    function zztmp(outStream, photo){
        outStream.write('<p> 제목 : {$p.title}</P>');
        outStream.write('<p> 날짜 : {$p.date.toDateString()}</P>');
    }
    ```

    - 대상 함수가 서브클래스에서 오버라이드됐다면

        1. 오버라이드한 서브클래스들의 메서드 모두에서 동일하게, 남길 부분을 추출

        2. 이 때 남겨질 메서드 본문은 모든 클래스에서 똑같아야 한다
        3. 슈퍼클래스의 메서드만 남기고 서브클래스들의 메서드를 제거

3. 원래 함수를 인라인(6.1)  (하나 수정마다 테스트)

    ```javascript
    // 호출자1
    function renderPerson(outStream, person){
        ...
        zztmp(outStream, photo); // 남길 원하는 데이터
        outStream.write('<p> 위치 : {$p.location}</P>');
        ...
    }

    // 호출자 2
    function listRecentPhotos(outStream, photos){
        ...
        zztmp(outStream, photo); // 남길 원하는 데이터
        outStream.write('<p> 위치 : {$p.location}</P>');
        ...
    }
    ```

4. 추출한 함수의 이름을 원래 함수의 이름으로 변경(6.5) 

    - 더 적절한게 있다면 그 이름으로