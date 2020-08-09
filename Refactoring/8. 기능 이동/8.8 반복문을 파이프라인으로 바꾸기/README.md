# 8.8 반복문을 파이프라인으로 바꾸기

## 언제

1. 컬렉션의 순회를 다룰 때(반복문) -> 보통 논리적으로 이해가 쉬워짐

## 절차

1. 반복문에서 사용하는 컬렉션을 가리키는 변수를 하나 만듬 (단순 기존 변수 복사본일수도 있음)

    ```javascript
    const lines = input.split("\n");
    ...
    const loopItems = lines // 변수 만듬
    for(const line of loopItems){
        ...
    }
    return result;
    ```

2. 반복문 첫 줄부터 시작해 -> 각각의 단위 행위를 적절한 컬렉션 파이프라인 연산으로 대체 -> 이 파이프라인 연산은 1에서 만든 컬렉션 변수에서 시작한다. (하나 대체할 때마다 테스트)

    ```javascript
    const lines = input.split("\n");
    // let firstLine = true; 제거
    const result = [];
    const loopItems = lines.slice(1);

    for(cosnt line of loopItems){
        //if(firstLine){
        //    firstLine = false;
        //    continue;
        //}
        ...
    }
    ```

    ```javascript
    const lines = input.split("\n");
    const result = [];
    const loopItems = lines
            .slice(1)
            .filter(line => line.trim() !== "");
    
    for(const line of loopItems){
        // if(line.trim() === "") continue; 제거
        ...
    }
    ```

    ```javascript
    const lines = input.split("\n");
    const result = [];
    const loopItems = lines
            .slice(1)
            .filter(line => line.trim() !== "")
            .map(line => line.split(","))
            .filter(record => record[1].trim() === "India")
            .map(record => ({city: record[0].trim(), phone: record[2].trim()}));
    
    for(const line of loopItems){
        const record = line;
        result.push(line);
    }
    return result;
    ```

3. 반복문의 모든 동작 대체 -> 반복문 자체를 지운다.

    ```javascript
    // 반복문 지우기 + 좀 더 가다듬기
    const lines = input.split("\n"); // 이건 그대로 나두는게 코드가 수행하는 일을 더 잘 설명해준다 판단하여
    return lines
            .slice(1)
            .filter(line => line.trim() !== "")
            .map(line => line.split(","))
            .filter(record => record[1].trim() === "India")
            .map(record => ({city: record[0].trim(), phone: record[2].trim()}));
    ```

    - 반복문이 결과를 누적 변수에 대입했으면 -> 파이프라인 결과를 그 누적변수에 대입