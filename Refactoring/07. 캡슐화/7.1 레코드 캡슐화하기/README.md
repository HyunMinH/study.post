# 7.1 레코드 캡슐화하기

## 언제

1. 객체 사용시 -> 어떤 정보를 저장했는지 숨긴채 각 값을 메서드로 제공 가능

## 절차

1. 레코드를 담은 변수를 캡슐화(6.6)한다.

2. 레코드를 감싼 단순한 클래스로 해당 변수의 내용 교체 -> 원본 레코드 반환하는 접근자 정의 -> 변수 캡슐화 함수들이 이 접근자 사용하도록

    ```javascript
    class Organization{
        constructor(data){
            this._data =data;
        }

        get data(){
            return this._data;
        }
    }

    // 최상위
    const organization = new Organization({name:"애크미", country: "GB"});
    function getRawDataOfOrganization() { return organization._data; }
    ```

3. 테스트

4. 원본 레코드 대신 새로 정의한 클래스 타입의 객체 반환하는 함수 만들기

    ```javascript
    //최상위
    ...
    function getOrganization() { return organization; }
    ```

5. 레코드 반환하는 예전 함수를 -> 4에서 만든 새 함수 사용하도록! 필드 접근시에 객의 접근자 사용(한 부분 바꿀 때마다 테스트)

    ```javascript
    // in Organization Class
    set name(aString) {this._data.name = aString;}
    get name() {return this._data.name; }

    //client
    getOrganization().name = newName;
    ...
    result += '<h1>${getOrganization().name}</h1>';

    ```

    - 중첩된 구조처럼 복잡한 레코드 -> 클라이언트가 데이터만 읽기만 한다면 데이터 복제본이나 읽기전용 프락시 반환할지 고려
  
    - 중첩된 구조처럼 복잡한 레코드의 데이터 세터 만들기(레코드 캡슐화) -> 클래스로 옮기기

    ```javascript
    // 최상위
    setUsage(customerID, year, month, amount){
        getRawDataOfCustomers()[customerID].usages[year][month] = amount;
    }
    ```

    ```javascript
    // in class
    setUsage(customerID, year, month, amount){
         this._data[customerID].usages[year][month] = amount;
    }
    ```

6. 클래스에서 원본 데이터 반환하는 접근자 및 원본 레코드 반환 함수 제거

    ```javascript
    function getRawDataOrganization() {...} // <- 삭제하기
    ```

    - 이후 _data의 필드들을 객체 안에 바로 펼쳐놓으면 더 깔끔

7. 테스트

8. 레코드의 필드도 데이터 구조인 중첩 구조 -> 레코드 캡슐화 & 컬렉션 캡슐화하기 재귀적 적용.

> 위 방법이 만능은 아니다. 데이터 구조가 거대하면 일이 상당히 커짐.
> 게터는 데이터 구조를 깊이 탐색하게 만들되, 원본 데이터를 그대로 반환하지 말고 객체로 감싸거 반환하는게 효과적일 수도 있다.
> "Refatoring Code to Load a Document" 에서 자세히 있다.
