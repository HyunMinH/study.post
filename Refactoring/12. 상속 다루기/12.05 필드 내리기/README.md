# 12.5 필드 내리기

## 언제

1. 서브 클래스 하나(혹은 소스) 에서만 사용하는 필드가 있을 때

## 절차

1. 대상 필드를 모든 서브클래스에 정의

2. 슈퍼클래스에서 그 필드를 제거

3. 테스트

4. 이 필드를 사용하지 않는 모든 서브클래스에서 제거

5. 테스트
