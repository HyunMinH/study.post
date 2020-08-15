# 11.6 세터 제거하기

## 언제

1. 객체 생성 후 수정되지 않길 원하는 필드가 있을 때

## 절차

1. 설정해야 할 값을 생성자에서 받지 않는다면 그 값을 받을 매개변수를 생성자에 추가(6.5) -> 그 다음 생성자 안에서 적절한 세터 호출

    ```javascript
    constructor(id){
        this.id = id; // 자바스크립트에서는 이게 생성자 호출임.
    }
    ```

2. 생성자 밖에서 세터 호출하는 곳 찾아 제거 -> 대신 새로운 생성자 사용하도록 (하나마다 테스트)
   
    ```javascript
    // 호출자
    const martin = new Person("1234");
    martin.name = "마틴";
    //martin.id = "1234";
    ```

3. 세터 메서드를 인라인. 가능하면 해당 필드를 불변으로 만든다.

    ```javascript
    constructor(id){
        this.id = id;
    }

    //set id(arg) {this._id = arg;}
    ```

4. 테스트