# 12.6 타입 코드를 서브클래스로 바꾸기

## 언제

1. 타입 코드로 객체를 구분하는 클래스가 있을 때

    - 특히 단순한 구분 외에 그에 따른 로직이 다를 때

## 절차

1. 타입 코드 필드를 자가 캡슐화한다.

    - 직접 상속일 때

    ```javascript
    // in Employee Class
    get type() {return this._type;}
    toString() {return '${this._name} (${this._type})';}
    ```

    - 간접 상속일 때

    ```javascript
    class EmployeeType{
        constructor(aString){
            this._value = aString;
        }
        toString() {return this._value;}
    }
    ```

2. 타입 코드 값 하나를 선택하여 그 값에 해당하는 서브클래스를 만든다.

    - 타입 코드 게터 메서드를 오버라이드하여 해당 타입 코드의 리터럴 값 반환하도록

    - 직접 상속일 때

    ```javascript
    class Engineer extends Employee {
        get type() { return "engineer";}
    }
    ```

3. 매개변수로 받은 타입 코드와 방금 만든 서브클래스를 매핑하는 선택 로직 만들기

    - 직접 상속일 때 생성자를 팩터리 함수로 바꾸기(11.8) 적용 후, 선택 로직을 팩터리에 넣는다.

    ```javascript
    function createEmployee(name, type){
        switch(type){
            case "engineer" : return new Engineer(name, type);
        }
        return new Employee(name, type);
    }
    ```

    - 간접 상속일 때는 선택 로직을 생성자에 둔다.

4. 테스트

5. 타입 코드 값 각각에 서브클래스 생성과 선택 로직 추가를 반복 (클래스 하나 완성마다 테스트)

    - 직접 상속일 때

    ```javascript

    class Salesperson extends Employee{
        get type() { return "salesperson"; }
    }

    class Manager extends Employee{
        get type() { return "manager"; }
    }

    function createEmployee(name, type){
        switch(type){
            case "engineer" : return new Engineer(name, type);
            case "salesperson" : return new Salesperson(name, type);
            case "manager" : return new Manager(name, type);
        }
        return new Employee(name, type);
    }
    ```

    - 간접 상속일 때

    ```javascript
    // in Employee Class
    set type(arg) {this._type = Employee.createEmployeeType(arg);}
    static createEmployeeType(aString){
        switch(type){
            case "engineer" : return new Engineer(name, type);
            case "salesperson" : return new Salesperson(name, type);
            case "manager" : return new Manager(name, type);
            default: throw new Error('${aString}라는 직원 유형은 없습니다');
        }
    }

    class EmployeeType{ ... }

    class Engineer extends EmployeeType {
        get type() { return "engineer";}
    }

    class Salesperson extends EmployeeType{
        get type() { return "salesperson"; }
    }

    class Manager extends EmployeeType{
        get type() { return "manager"; }
    }
    ```

6. 타입 코드 필드를 제거

    ```javascript
    // in Employee class
    constructor(name, /* type*/){
        this.validateType(type);
        this._name = name;
        //this._type = type;
    }
    ```

7.  테스트

8. 타입 코드 접근자를 이용하는 메서드 모두에 메서드 내리기(12.4)와 조건부 로직을 다형성으로 바꾸기(10.4)를 적용

    - 이 타입 코드 게터를 사용하는 코드가 어딘가 남아 있을 수 있음.

    - -> 로직을 다형성으로 바꾸기(10.4)와 메서드 내리기(12.4) 로 하나씩 해결해가기

    - -> 마지막에 죽은 코드 제거하기로 없애면 됨.