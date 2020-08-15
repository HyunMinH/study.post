# 7.5 클래스 추출하기

## 언제

1. 메서드와 데이터가 너무 많은 클래스

2. 함께 변경되는 일이 많거나 서로 의존하는 데이터들

    - 제거해도 다른 필드나 메서드 논리적 이상 없음 -> 분리 가능

## 절차

1. 클래스의 역할을 분리할 방법 정하기

2. 분리될 역할을 담당할 클래스 새로 만들기

    ```javascript
    class TelephoneNumber{

    }
    ```

3. 원래 클래스의 생성자에서 새로운 클래스의 인스턴스 생성하여 필드에 저장

    ```javascript
    // in Person Class
    constructor(){
        this._telephoneNumber = new TelephoneNumber();
    }
    ```

4. 분리될 역할에 필요한 필드들 -> 새 클래스로 옮기기 (하나 옮길 때마다 테스트)

    ```javascript
    // in TelephoneNumber Class
    get officeNumber() {return this._officeNumber;}
    set officeNumber(arg) {this._officeNumber =arg;}
    ```

5. 메서드들도 새 클래스로 옮기기.

    - 이 때 호출을 당하는 일이 많은 메서드부터 옮기기(하나 옮길때마다 테스트)

    ```javascript
    // in TelephoneNumber Class
    get telephoneNumber() {return '(${this.officeAreaCode}) ${this.officeNumber}';}
    ```

6. 양쪽 클래스 인터페이스를 보며 불필요한 메서드 제거 & 이름도 새로운 환경에 맞게 수정

    ```javascript
    // in TelephoneNumber Class
    get areaCode() {return this._areaCode;}
    set areaCode() {return this._areaCode = arg;}
    get number() {return this._number;}
    set number() {return this._number = arg;}
    toString() {return '(${this.areaCode}) ${this.number}';}
    ```

    ```javascript
    // in Person Class
    get officeAreaCode() {return this._telephoneNumber.areaCode; }
    set officeAreaCode(arg) {...}
    get officeNubmer() {...}
    set officeNumber() {...}
    ```

7. 새 클래스를 외부로 노출할지 정한다.

    - 노출하려거든 새 클래스에 참조를 값으로 바꾸기를 적용할지 고려
