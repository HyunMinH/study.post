# 6.6 변수 캡슐화하기

## 언제

1. 데이터는 유효범위가 넓어질수록 다루기 어려워짐

2. 데이터 변경 전 검증이나 변경 후 추가 로직을 끼워넣고 싶을 때

- 불변 데이터의 경우, 크게 이 리팩터링 적용할 이유는 없다. 

## 절차

1. 접근, 갱신 전담하는 캡슐화 함수 만든다.

2. 정적 검사
3. 직접 참조하는 부분 -> 캡슐화 함수 호출로, 하나마다 테스트
4. 변수 접근 범위 제한
    - 할수 없을 때 변수 이름 바꿔서 테스트
5. 테스트
6. 변수 값이 레코드 -> 레코드 캡슐화하기할지 고려

## 예시

### 기본 적용

```javascript
let defulatOwner = {firstName: "마틴", lastName: "파울러"};
```

```javascript
//defaultOwner.js 파일 <- 변수 가시 범위 제한
let defaultOwner = {firstName: "마틴", lastName: "파울러"};
export function getDefaultOwner() {return defaultOwner;}
export function setDefaultOwner(arg) {defaultOwner = arg;}
```

### 값 캡슐화하기

#### 1. 데이터 복제본 반환(방법1)

```javascript
let defaultOwner = {firstName: "마틴", lastName: "파울러"};
export function getDefaultOwner() {return Object.assign({}, defaultOwner)};
export function setDefaultOwner(arg) {defaultOwner = arg;}
```

#### 2. 레코드 캡슐화하기

```javascript
let defaultOwner = {firstName: "마틴", lastName: "파울러"};
export function getDefaultOwner() {return new Person(defaultOwner)};
export function setDefaultOwner(arg) {defaultOwner = arg;}

class Person{
    // 갱신 메서드 제공 x
    constructor(data){
        this._lastName = data.lastName;
        this._firstname = data.firstname;
    }

    get lastName() {return this._lastName;};
    get firstName() {return this._firstName;}
```
