# 6.2 함수 인라인하기

## 언제

1) 간접호출을 과하게 사용

2) 리팩터링 도중 잘못 추출한 함수

## 절차

1) 오버라이딩한 메서드는 인라인하면 x

2) 인라인할 함수 호출하는 곳 모두 찾기

3) 각 호출문을 -> 함수 본문으로 교체

```javascript
function reportLines(aCustomer){
    const lines = [];
    gatherCustomerData(lines, aCustomer);
    return lines;
}

function gatherCustomerData(out, aCustomer){
    out.push(["name", aCustomer.name]);
    out.push(["location", aCustomer.location]);
}
```

```javascript
function reportLines(aCustomer){
    const lines = [];
    lines.push(["name", aCustomer.name]); // 1번째로 옮긴 후
    lines.push("[location", aCustomer.location]); // 2번째로 옮기기
    return lines;
}
```

4) 호출문 하나 교체마다 테스트

5) 함수 정의(원래 함수) 삭제
