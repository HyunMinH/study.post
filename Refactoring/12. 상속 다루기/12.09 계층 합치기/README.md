# 12.9 계층 합치기

## 언제

1. 계층구조가 진화하면서 어떤 클래스와 그 부모가 너무 비슷해져서 더는 독립적으로 존재할 이유가 없을 때

## 절차

1. 두 클래스 중 제거할 것 고름 (더 적합한 이름의 클래스로)

2. 필드 올리기(12.2)와 메서드 올리기(12.1) or 필드 내리기(12.5)와 메서드 내리기(12.4)를 적용하여 모든 요소를 하나의 클래스로 옮긴다.

3. 제거할 클래스를 참조하던 모든 코드 -> 남겨질 클래스를 참조하도록 수정

4. 빈 클래스 제거

5. 테스트