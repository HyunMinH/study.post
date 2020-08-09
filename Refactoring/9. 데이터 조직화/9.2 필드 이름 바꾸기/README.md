# 9.2 필드 이름 바꾸기

## 언제

1. 더 좋은 이름이 있을 때

## 절차

1. 레코드의 유효 범위가 제한적 -> 필드에 접근하는 모든 코드 수정 후 테스트, 이후 과정 필요 x

2. 레코드 캡슐화하기(7.1), 안되있다면

    ```javascript
    class Organization{
        constructor(data){
            this._name = data.name;
            this._country = data.country;
        }

        ...
        getter, 
        setter
        ...
    }

    const organization = new Organization({name: "애크미", counrty: "GB"});
    ```

3. 캡슐화된 객체 안의 private 필드명을 변경하고 -> 그에 맞게 내부 메서드들을 수정한다.

    ```javascript
    class Organization{
        constructor(data){
            this._title = data.title;
            ...
        }

        get name() {return this._title;}
        get name(aString) {this._title = aString;}
        get country() {...}
        set country(aCountryCode) {...}
    }

    // 호출하는 쪽도 맞춰주기
    const organization = new Organization({title: "애크미", country: "GB"});
    ```

4. 테스트

5. 생성자의 매개변수 중 필드 이름 겹치는게 있다면 -> 함수 선언 바꾸기(6.5)

6. 접근자들의 이름도 바꾸기(6.5)

    ```javascript
    class Organization{
        constructor(data){
            ...
        }

        get title() {return this._title;}
        set title(aString) {this._title = aString;}
        get country() {...}
        set country(aCountryCode) {...}
    }
    ```