# 12.3 생성자 본문 올리기

## 언제

1. 서브클래스들 간 중복된 생성자 내용

> 이 리팩터링이 간단히 끝날 것 같지 않다면 
> 생성자를 팩터리 함수로 바꾸기(11.8)를 고려

## 절차

1. 슈퍼 클래스에 생성자가 없다면 하나 정의. 서브클래스의 생성자들에서 이 생성자가 호출되는지 확인

2. 문장 슬라이드(8.6)로 공통 문장 모두를 super()호출 직후로 옮김

    ```javascript
    class Employee extends Party{
        constructor(name, id, monthlyCost){
            super();
            this._name = name;

            this._id = id;
            this._monthlyCost = monthlyCost;
        }
    }
    ```

3. 공통 코드를 슈퍼클래스에 추가 & 서브클래스들에서는 제거. 

    - 생성자 매개변수 중 공통 코드에서 참조하는 값들을 모두 super()로 건넴

    ```javascript
    class Party{
        constructor(name){
            this._name = name;
        }
    }

    class Employee extends Party{
        constructor(name, id, monthlyCost){
            super(name);
            this._id = id;
            this._monthlyCost = monthlyCost;
        }
    }

    class Department extends Party{
        constructor(name, staff){
            super(name);
            this._staff = staff;
        }
    }
    ```

4. 테스트

5. 생성자 시작 부분으로 옮길 수 없는 공통 코드에는 함수 추출하기(6.1)와 메서드 올리기(12.1)을 차례로 적용

    ```javascript
    class Manager extends Party{
        constructor(name, grade){
            super(name);
            this._grade = grade;
            this.finishConstruction();
        }

        finishConstruction(){
            if(this.isPrivileged) this.assignCar();
        }
    }
    ```

    ```javascript
    // in Employee Class
    finishConstruction(){
        if(this.isPrivileged) this.assignCar();
    }

    // in Manager Class
    // finishConstruction() {...}
    ```