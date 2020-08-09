# 10.5 특이 케이스 추가하기

## 언제

1. 특정 값(ex. null)을 확인한 후 똑같은 동작을 수행하는 코드가 곳곳에 등장 할 때

## 절차

1. 컨테이너에 특이 케이스인지를 검사하는 속성을 추가하고, false를 반환하도록 한다.

    ```javascript
    // in Customer Class
    get isUnkown() {return false;}
    ```

2. 특이 케이스 객체를 만든다. 이 객체는 특이 케이스인지를 검사하는 속성만 포함하며, 이 속성은 true를 반환한다.

    ```javascript
    // 여기서는 클래스로 만듬
    class UnknownCustomer{
        get isUnknown() {return true;}
    }
    ```

3. 클라이언트에서 특이 케이스인지를 검사하는 코드를 함수로 추출(6.1) -> 모든 클라이언트가 값을 직접 비교하는 대신 방금 추출한 함수 사용하도록

    ```javascript
    function isUnknown(arg){
        if(!((arg instanceof Customer) || (arg === "미확인 고객")))
            throw new Error('잘못된 값과 비교: <${arg}>');
        return (arg === "미확인 고객");
    }
    ```

    ```javascript
    // client1
    if(isUnknown(aCustomer)) customername = "거주자";
    ...

    // client2
    const plan = (isUnknown(acustomer)) ? registry.billingPlans.basic : aCustomer.billingPlan;
    ```

4. 코드에 새로운 특이 케이스 대상 추가. 함수 반환 값으로 받거나 변환 함수로 적용

    ```javascript
    get customer(){
        return (this._customer === "미확인 고객") ? new UnknownCustomer() : this._customer;
    }
    ```

5. 특이 케이스 검사하는 함수 본문 수정 -> 특이 케이스 객체의 속성 사용하도록

    ```javascript
    function isUnknown(arg){
        if(!((arg instanceof Customer) || (arg instanceof UnknownCustomer))
            throw new Error('잘못된 값과 비교: <${arg}>');
        return arg.isUnknown;
    }
    ```

6. 테스트

7. 각 클라이언트에서 수행하는 특이케이스 검사를 일반적인 기본값으로 대체할 수 있을 때 
   
   - 여러 함수를 클래스로 묶기(6.9) or 여러 함수를 변환 함수로 묶기(6.10)를 적용하여 특이 케이스를 처리하는 공통 동작을 새로운 요소로 옮긴다.

    - 여러 함수를 클래스로 묶기(6.9)

    ```javascript
    // client 1
    let customerName;
    if(isUnknown(aCustomer)) customerName = "거주자";
    else customerName = aCustomer.name;
    ```

    ```javascript
    // in UnknownCustomer Class
    get name() {return "거주자"}

    //client 1
    const customerName = aCustomer.name;
    ```

8. 아직도 특이 케이스 검사 함수 이용하는 곳 있으면 -> 검사 함수를 인라인(6.2)

> 위처럼 특이케이스를 위해 클래스를 쓸 수 있지만 데이터 구조를 읽기만 한다면 클래스 대신 리터럴 객체를 사용해도 된다.

> 두 방식 다 모두 클래스와 관련 있지만, 변환 단계를 추가하면 같은 아이디어를 레코드에도 적용할 수 있다.