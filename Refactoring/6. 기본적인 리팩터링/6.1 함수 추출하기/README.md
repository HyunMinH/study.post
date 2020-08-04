## 6.1 함수 추출하기
### 언제 : 목적과 구현 분리
<br>

### 절차
1. 함수를 새로 만들고, 목적을 잘 드러내는 이름 붙인다.(무엇을 하는지)
   - 중첩 함수 지원하는 언어 -> 원래 함수 안에 중첩 -> 캡슐화
   - 원래 함수 바깥으로 꺼내야 할 때 -> 추출 함수를 함수 옮기기 적용
2. 추출한 코드를 원본 함수에서 복사해서 붙임
3. 추출한 코드 중, 원본 함수의 지역 변수 참조 or 추출한 함수의 유효범위 벗어나는 변수 있는지 검사 
   - 있다면 매개변수로 전달
   - 반환할 값이 여러개 -> 각각을 반환하는 함수 여러개 or 레코드로 묶어서 반환
4. 컴파일
5. 원본 함수 코드를 -> 새로 만든 함수 호출로
6. 테스트
7. 다른 코드에 방금 추출한 코드와 비슷한게 있는지 확인 -> 잇으면 이를 호출할지 결정

<br>

### 유효 범위를 벗어나는 변수가 없을 때
```
function printOwing(invoice){
    let outStanding = 0;

    console.log("################");
    console.log("####고객 채무####");
    console.log("################");

    ...
}
```

```
function printOwing(invoice){
    let outStanding = 0;

    printBanner();
}

function printBanner(){
    console.log("################");
    console.log("####고객 채무####");
    console.log("################");
}
```

### 지역 변수를 사용할 때
```
function printOwing(invice){
    ...

    cosole.log('고객명: ${invoice.customer}');
    cosole.log('채무액: ${invoice.customer}');
    cosole.log('마감일: ${invoice.customer}');
}
```

```
function printOwing(invice){
    ...

    printDetails(invoice, outStanding)
}
    

function printDetails(invoice, outStanding){
    cosole.log('고객명: ${invoice.customer}');
    cosole.log('채무액: ${invoice.customer}');
    cosole.log('마감일: ${invoice.customer}');
}
```


### 지역 변수의 값 변경할 때
```
function printOwing(invoice){
    let OutStanding=0;
    ...

    for(const o of invoice.orders){
        outstanding += o.mount;
    }

    ...
}
```
```
function pringOwing(invoice){
    ...
    const outStanding = calculateOutstanding(invoice);
    ...
}

function calculateOutstanding(invoice){
    let result = 0;
    for(const o of invoice.orders){
        result += o.amount;
    }
    return result;
}
```