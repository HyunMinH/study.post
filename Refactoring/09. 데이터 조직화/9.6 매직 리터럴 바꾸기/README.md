# 9.6 매직 리터럴 바꾸기
 
## 언제

1. 단순 숫자보다 의미를 부여하고 싶을 때

2. 중복을 제거하고 싶을 때

> 상수가 특별한 비교 로직에 자주 쓰일 때 -> 함수 호출로 바꾸는게 더 좋을수도 있다.

## 절차

1. 상수를 선언하고 매직 리터럴을 대입한다.

2. 해당 리터럴이 사용되는 곳을 모두 찾는다.

3. 찾은 곳 각각에서 리터럴이 새 상수와 똑같은 의미로 쓰였는지 확인 -> 같은 의미라면 상수로 대체한 후 테스트