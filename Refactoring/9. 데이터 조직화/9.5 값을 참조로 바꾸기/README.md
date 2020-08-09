# 9.5 값을 참조로 바꾸기

## 언제

1. 값 객체 사용으로 메모리가 부족할 때 (흔한 일은 아니다.)

2. 데이터를 갱신하는데, 여러 다른 객체들이 같은 객체를 사용하고 있을 때

## 절차

1. 같은 부류에 속하는 객체들을 보관할 저장소를 없으면 만든다.

    ```javascript
    let _repositoryData;

    export function initialize(){
        _repositoryData = {};
        _repositoryData.customers = new Map();
    }

    export function registerCustomer(id){
        if(! _repositoryData.customers.has(id)){
            _repositoryData.customers.set(id, new Customer(id));
        }
        return findCustomer(id);
    }

    export function findCustomer(id){
        return _repositoryData.customers.get(id);
    }
    ```

2. 생성자에서 이 부류의 객체들 중 특정 객체를 정확히 찾아내는 방법 있는지 확인

3. 호스트 객체의 생성자들을 수정하여 필요한 객체를 이 저장소에서 찾도록 한다 (하나 수정마다 테스트)

    ```javascript
    class Order{
        constructor(data){
            this._nubmer = data.number;
            this._customer = registorCustomer(data.customer);
            ...
        }

        get customer() {return this._customer; }
    }
    ```
    > 위처럼 전역 저장소와 결합됨 -> 걱정된다면 저장소를 생성자 매개변수로 전달하도록 수정하자.