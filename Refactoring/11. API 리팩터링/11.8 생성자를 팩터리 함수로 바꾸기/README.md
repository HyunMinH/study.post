# 11.8 생성자를 팩터리 함수로 바꾸기

## 언제

1. 서브클래스의 인스턴스나 프락시를 반환하고 싶을 때

2. 생성자보다 더 적절한 이름을 사용하고 싶을 때

## 절차

1. 팩터리 함수를 만든다. 팩터리 함수의 본문에서는 원래의 생성자 호출

    ```javascript
    function createEmployee(name, typeCode){
        return new Employee(name, typeCode);
    }
    ```

2. 생성자 호출하던 코드를 팩터리 함수 호출로 바꾼다.

    ```javascript
    //호출자
    const candiate = createEmployee(document.name, document.empType);
    ```

3. 하나씩 수정할 때마다 테스트한다.

4. 생성자의 가시 범위가 최소가 되도록 제한한다.

    ```javascript
    // 리터럴 제거
    function createEmployee(name){
        return new Employee(name, 'E');
    }

    //호출자
    const candiate = createEmployee(document.name);
    ```