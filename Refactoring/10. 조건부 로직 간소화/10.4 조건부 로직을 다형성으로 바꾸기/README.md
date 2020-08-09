# 10.4 조건부 로직을 다형성으로 바꾸기

## 언제

1. 복잡한 조건부 로직(ex. switch문, 복잡한 if/else문)

## 절차

1. 다형적 동작을 표현하는 클래스들이 없으면 만들어준다.

    - 적합한 인스턴스를 알아서 만들어 반환하는 팩터리 함수도 함께 만든다.

    ```javascript
    class Bird{
        ...

        function createBird(bird){
            switch(bird.type){
            case: '유럽 제비' : return new EuropeanSwallow(bird);
            case: '아프리카 제비' : return new AfricanSwallow(bird);
            default: return new Bird(bird);
            }
        }

        class EuropeanSwallow extends Bird{

        }

        class AfricanSwallow extends Bird{

        }
    }
    ```

2. 호출하는 코드에서 팩터리 함수를 사용하게 함.

    - 흠! 3부터 먼저 적용하면 아래처럼 코드 작성 가능

    ```javascript
    function plumage(bird){
        return createBird(bird).plumage;
    }

    function airSpeedVelocity(bird){
        return createBird(bird).airSpeedVelocity;
    }
    ```

3. 조건부 로직 함수 -> 슈퍼클래스로 옮김

    ```javascript
    //in Bird Class
    get plumage(){
        switch(this.type){
        case '유럽 제비' : return '보통이다';
        case '아프리카 제비' : return (this.numberOfCoconuts > 2) ? '지쳤다' : '보통이다';
        default : return '알 수 없다.'
        }
    }

    get airspeedVelocity(){
        switch(this.type){
            ...
        }
    }
    ```

    - 조건부 로직이 온전한 함수로 분리 x -> 함수 추출(6.1)부터

4. 서브 클래스 중 하나 선택 -> 서브클래스에서 슈퍼클래스의 조건부 로직 메서드를 오버라이드. (해당 서브클래스에 해당하는 조건절을 서브클래스 메서드로 복사 후 적절히 수정)

    ```javascript
    // in EuropeanSwallow Class
    get plumage(){
        return "보통이다";
    }

    // in Bird Class
    get plumage(){
        switch(this.type){
        case '유럽 제비' : throw '오류 발생';
        ...
        }
    }
    ```

5. 같은 방식으로 각 조건절을 해당 서브클래스에서 메서드로 구현

    ```javascript
    // in AfricanSwallow Class
    get plumage(){
        return (this.numberOfCoconuts > 2) ? "지쳤다" : "보통이다"
    }
    ```

6. 슈퍼클래스 메서드에는 기본 동작 부분만 남김.

    ```javascript
    // in Bird Class
    get plumage(){
        return "알 수 없다"
    }
    ```

    - 슈퍼클래스가 추상 클래스여야 한다면 -> 추상메서드로 or 서브클래스에서 처리해야 함을 알리는 에러 던지기


> 더 복잡한 변형 동작을 다형성으로 표현하기의 예시는 책을 참고!