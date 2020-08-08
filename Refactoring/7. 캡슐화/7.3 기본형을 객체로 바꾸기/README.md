# 7.3 기본형을 객체로 만들기

## 언제

1. 기본형이 단순한 출력 이상의 기능이 필요해지는 순간

## 절차

1. 변수 캡슐화 안되있으면 -> 캡슐화(6.6)

2. 단순한 값 클래스 만들기. 생성자는 기존 값 인수 받아 저장, 이 값 반환하는 게터 추가

    ```javascript
    class Priority{
        constructor(value) {this._value = value;}
        toString() {return this._value;}
    }
    ```

3. 정적 검사

4. 값 클래스의 인스턴스 새로 만들어 필드에 저장하도록 -> 세터 수정, 이미 있다면 필드 타입을 적절히 변경

    ```javascript
    // in Order Class(Priority Class 호출하는 클래스)
    ...
    set priority(aString) {this._priority = new Priority(aString);}
    ```

5. 새로 만든 클래스의 게터 호출한 결과를 반환하도록 -> 게터 수정

    ```javascript
    // in Order Class(Priority Class 호출하는 클래스)
    ...
    get priority() {return this._priority.toString();}
    ```

6. 테스트

7. 함수 이름 바꿀지 검토

    ```javascript
    // 여기에서는 priority의 문자열을 반환하는 거니 이름을 바꿔주자
    // in Order Class
    ...
    get priorityString() {return this._prioirity.toString();}
    ```

    - 참조를 값으로 바꾸거나 값을 참조로 바꾸면 새로 만든 객체의 역할이 더 잘 드러나는지 검토

    ```javascript
    // in Order Class
    ...
    get priority() {return this._priority;}

    // 새로운 동작 담아 적절한 메시지 제공
    class Priority{
        constructor(value){
            if(value instanceof Priority) return value;
            if(Prioirity.legalValues().includes(value))
                this._value = value;
            else
                throw new Error('<${value} is invalid for Priority');
        }

        toString() {...}
        get_index() {...}
        static legalValues() {return ['low', 'normal', 'high', 'rush'];}

        equals(other) {...}
        higherThan(other) {...}
        lowerThan(other) {...}
    }
    ```
