# 7.7 위임 숨기기

## 언제

1. 서버 객체의 위임객체를 클라이언트가 모르게 하고 싶을 때

## 절차

1. 위임 객체의 각 메서드에 해당하는 위임 메서드 -> 서버에 생성

    ```javascript
    // in Person Class
    get manager() {return this._department.manager;}
    ```

2. 클라이언트가 위임 객체 대신 -> 서버 호출하도록 수정 (하나 바꿀때마다 테스트)

    ```javascript
    manager = aPerson.department.manager;
    ```

    ```javascript
    manager = aPerson.manager;
    ```

3. 모두 수정시 -> 서버로부터 위임 객체 얻는 접근자 제거

4. 테스트
