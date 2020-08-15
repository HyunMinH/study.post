# 8.7 반복문 쪼개기

## 언제 

1. 반복문에서 두 가지 일을 수행할 때

2. 여러 값을 계산하는 반복문

3. 함수로 추출하기 전

> 두 번 반복하며 병목이라 밝혀지면 그 때 다시 합쳐도 된다. 하지만 보통 다른 더 강력한 최적화를 위한 길을 열어준다.

## 절차

1. 반복문을 복제해 두 개로 만든다.

2. 반복문이 중복되어 생기는 부수효과를 파악해서 제거한다.

    ```javascript
    ...
    for(const p of people){
        // if(p.age < youngest) youngest = p.age;
        totalSalary += p.salary;
    }

    for(cosnt p of people){
        if(p.age < youngest) youngest = p.age;
        // totalSalary += p.salary;
    }
    ...
    ```

3. 테스트

4. 각 반복문을 함수로 추출(6.1)할지 고민

    ```javascript
    // 함수 추출하기(6.1) + 반복문을 파이프라인으로 교체하기(8.8)
    function totalSalary(){
        return people.reduce((total,p) => total + p.salary, 0);
    }

    function youngestAge(){
        return Math.min(...people.map(p=>p.age));
    }
    ```