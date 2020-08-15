# 12.7 서브클래스 제거하기

## 언제

1. 더 이상 쓰이지 않는 서브클래스가 있을 때

2. 서브클래스를 슈퍼클래스의 필드로 대체하는게 오히려 나을 때

## 절차

1. 서브클래스의 생성자를 팩터리 함수로 바꾼다(11.8)

    - 생성자를 사용하는 측에서 데이터 필드를 이용해 어떤 서브클래스를 생성할지 결정한다면 

    - 그 결정 로직을 슈퍼클래스의 팩터리 메서드에 넣는다.

    ```javascript
    function createPerson(aRecord){
        switch(aRecord.gender){
            case 'M' : return new Male(aRecord.name); 
            case 'F' : return new Female(aRecord.name);
            default : return new Person(aRecord.name);
        }
    }
    ```

2. 서브클래스의 타입을 검사하는 코드 -> 그 검사 코드에 함수 추출하기(6.1)와 함수 옮기기(8.1)를 차례로 적용하여 슈퍼클래스로 옮김 (하나 변경마다 테스트)

    ```javascript
    // client
    const numberOfMales = people.filter(p => p instanceof Male).length;
    ```

    ```javascript
    // client
    const numberOfMales = people.filter(p => isMale(p)).length;

    // in Person Class
    get isMale() { return this instanceof Male; }
    ```

3. 서브클래스의 타입을 나타내는 필드를 슈퍼클래스에 만든다.

    ```javascript
    // in Person Class
    constructor(name, genderCode){
        ...
        this._genderCode = genderCode || "X";
    }

    get genderCode() { return this._genderCode; }
    ```

4. 서브클래스를 참조하는 메서드가 방금 만든 타입 필드를 이용하도록 수정

    ```javascript
    function createPerson(aRecord){
        switch(aRecord.gender){
            case 'M' : return new Person(aRecord.name, 'M');
            case 'F' : return new Female(aRecord.name);
            default : return new Person(aRecord.name);
        }
    }

    get isMale() { return "M" === this._genderCode; }
    ```

5. 서브클래스 지운다.

6. 테스트

    - 성공시 지우고 싶은 서브클래스들을 모두 없앨 때까지 위 과정을 반복
