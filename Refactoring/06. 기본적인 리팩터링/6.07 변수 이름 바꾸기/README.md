# 6.7 변수 이름 바꾸기

## 언제

1. 적절한 이름이 생각났을 때

## 절차

1. 폭넓게 쓰이는 변수 -> 변수 캡슐화하기 고려

2. 이름 바꿀 변수 참조하는 모든 곳 -> 하나씩 변경

    - 다른 코드베이스에서 참조하는 변수 -> 외부에 공개됨 -> 적용 불가
3. 테스트

## 예시

### 기존 코드

```javascript
let tpHd = "untitled";

result += '<h1>${tpHd}</h1>';

tpHd = obj['articleTitle'];
```

### 1. 캡슐화

```javascript
result += '<h1>${title()}</h1>';

setTitle(obj['articleTitle']);

function title() {return tpHd;};
function setTitle(arg) {tpHd = arg;}
```

### 2. 이름 바꾸기

```javascript
let _title = "untitled";

function title() {return _title;};
function setTitle(arg) {_title = arg;}
```
