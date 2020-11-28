# 6.3 변수 추출하기

## 언제

1) 표현식이 너무 복잡할 때

## 절차

1) 추출하려는 표현식에 부작용은 없는지 확인

2) 불변 변수를 하나 선언하고 표현식 복제본 대입

```javascript
function price(order){
    const basePrice = order.quantity * order.itemPrice;
    return order.quantitiy * order.itemPrice
        - Math.max(0, order.quantity - 500) * order.itemPrice * 0.05
        + Math.min(order.quantitiy * order.itemPrice * 0.1, 100);
}
```

3) 원본 표현식을 새로 만든 변수로 교체

```javascript
function price(order){
    const basePrice = order.quantity * order.itemPrice;
    return basePrice
        - Math.max(0, order.quantity - 500) * order.itemPrice * 0.05
        + Math.min(order.quantitiy * order.itemPrice * 0.1, 100);
}
```

4) 테스트

5) 표현식 들어갈만 한곳 3-4번 반복 적용

```
function price(order){
    const basePrice = order.quantity * order.itemPrice; // 1번째, 만든 후 basePrice 쓰이는 곳 모두 대체
    const quantitiy Discount = Math.max(0, order.quantity - 500) * order.itemPrice * 0.05; // 2번째
    const shiping = Math.min(basePrice * 0.1, 100); // 3번째
    return basePrice - quantityDiscount + shipping;
}
```
