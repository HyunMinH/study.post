# 6.1 함수 추출하기

## 언제

1) 목적과 구현 분리

## 절차

1. 목적을 잘 드러내는 함수를 새로 만들기

    ```javascript
    function pringBanner(){

    }
    ```
   - 원래 함수 바깥으로 꺼낼때 -> 함수 옮기기 적용
  
2. 원본 함수에서 복사해서 붙임

    ```javascript
    function pringBanner(){
        console.log("***********************");
        console.log("********* 고객 채무 *********");
        console.log("***********************");
    }
    ```

3. 원본 함수의 지역 변수 참조 or 추출한 함수의 유효범위 벗어나는 변수 있을 시 

   - 변수 참조 -> 매개변수로 전달
   
   ```javascript
   function printDetails(invoice, outstanding){
       console.log('고객명: ${invoice.customer}');
       console.log('채무액: ${outstanding}');
       console.log('마감일: ${invoice.dueDate.toLocaleDateString()}');   
   }
   ```
   
   - 반환 -> 각각을 반환하는 함수 여러개 or 레코드로 묶어서 반환
   
   ```javascript
   let outstanding = 0;
   for(const o of invoice.orders){
       outstanding += o.amount;
   }
   ```
   
   ```javascript
   function calculateOutStanding(invoice){
       let ret = 0;
       for(const o of invoice.orders){
           ret += o.amount;
       }
       return ret;
   }
   ```
   
4. 컴파일

5. 새로운 함수 호출하도록

6. 테스트

7. 다른 코드에 방금 추출한 코드와 비슷한게 있는지 확인 -> 있으면 이를 호출할지 결정
