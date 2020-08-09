# 9.4 참조를 값으로 바꾸기

## 언제

1. 값 객체를 통해 불변성을 얻고 싶을 때 -> 분산 시스템과 동시성 시스템에 유용

2. 하지만 특정 불변 아닌 객체를 여러 객체에게 공유하고자 하면 -> 공유 객체 값 변경했을 때 이를 관련 객체에 알려줘야함 -> 공유 객체를 참조로

## 절차

1. 후보 클래스가 불변인지, 혹은 불변이 될 수 있는지 확인

2. 각각의 세터를 하나씩 제거(11.7) -> 불변으로 만드는 과정

    ```javascript
    class TelephoneNumber{
        constructor(areaCode, number){
            this._areaCode =areaCode;
            this._number = number;
        }

        //get areaCode() {...}
        //set areaCode(arg) {...}
        //get number() {...}
        //set number(arg) {...}
    }


    // in PersonClass
    ...
    set officeAreaCode(arg){
        this._telephoneNumber = new TelephoneNumber(arg, this.officeNumber);
    }
    set officeNumber(arg){
        this._telephoneNumber = new TelephoneNumber(this.officeAreaCode, arg);
    }
    ```

3. 이 값 객체의 필드들을 사용하는 동치성 비교 메서드 만들기(값 객체로 제대로 사용위해)

    ```javascript
    // in TelephoneNumber Class
    equals(other){
        if(!(other instanceof TelephoneNumber)) return false;
        return this.areaCode == other.areaCode && this.number == other.number;
    }
    ```

    - 대부분의 언어는 오버라이딩 가능한 동치성 비교 메서드 제공함.