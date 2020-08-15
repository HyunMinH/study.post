# 11.13 예외를 사전확인으로 바꾸기

## 언제

1. 함수 수행 시 문제가 될 수 있는 조건을 함수 호출 전에 검사할 수 있을 때

## 절차

1. 예외를 유발하는 상황을 검사할 수 있는 조건문 추가.

    - catch 블록의 코드를 조건문의 조건절 중 하나로 옮기고, 남은 try 블록의 코드를 다른 조건절로 옮긴다.

    ```javascript
    public Resource get(){
        Resource result;
        try{
            result = available.pop();
            allocated.add(result);
        }catch(NoSuchElementException e){
            result = Resource.create();
            allocated.add(result);
        }
        return result;
    }
    ```

    ```javascript
    public Resource get(){
        Resource result;
        if(available.isEmpty()){
            result = Resource.create();
            allocated.add(result);
        }else{
            try{
                result = available.pop();
                allocated.add(result);
            }catch(NoSuchElementException e){

            }
        }
        return result;
    }
    ```

2. catch 블록에 어서션을 추가하고 테스트

    ```javascript
    public Resource get(){
        Resource result;
        if(available.isEmpty()){
            result = Resource.create();
            allocated.add(result);
        }else{
            try{
                result = available.pop();
                allocated.add(result);
            }catch(NoSuchElementException e){
                throw new AssertionError("도달 불가")
            }
        }
        return result;
    }
    ```

3. try와 catch 블록을 제거

    ```javascript
    public Resource get(){
        Resource result;
        if(available.isEmpty()){
            result = Resource.create();
            allocated.add(result);
        }else{
            result = available.pop();
            allocated.add(result);
        }
        return result;
    }
    ```

4. 테스트

## 예시 더 가다듬기

1. 문장 슬라이드

```javascript
public Resource get(){
    Resource result;
    if(available.isEmpty()){
        result = Resource.create();
    }else{
        result = available.pop();
    }
    allocated.add(result);
    return result;
}
```

2. 3항 연산자로 바꾸기

```javascript
public Resource get(){
    Resource result = available.isEmpty() ? Resource.create() : available.pop();
    allocated.add(result);
    return result;
}
```