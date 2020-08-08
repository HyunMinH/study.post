# 7.2 컬렉션 캡슐화하기

## 언제

1. 가변 컬렉션이 존재 할 때

2. 데이터 구조가 언제 어떻게 수정되는지 파악하기 쉽게 하기 위해

## 절차

1. 컬렉션 캡슐화하지 않았다면 변수 캡슐화(6.6)부터

2. 컬렉션에 원소를 추가/제거하는 함수 추가

    ```javascript
    // in Person Class
    addCourse(aCourse){
        this._courses.push(aCourse);
    }
    removeCourse(aCource, fnInfAbsend = () => {throw new RangeError();}){
        const index = this._courses.indexOf(aCourse);
        if(index == -1) fnInfAbsent();
        else this._courses.splice(index, 1);
    }
    ```

    - 컬렉션 자체를 통째로 바꾸는 세터는 제거

3. 정적 검사

4. 컬렉션 참조 부분 중 컬렉션 변경자 호출하는 코드 모두 -> 앞에서 추가한 추가/제거 함수 호출하도록(하나씩 수정할 때마다 테스트)

    ```javascript
    // client
    for(const name of readBasicCourseNames(fileName)){
        aPerson.addCourse(new Course(name, false));
    }
    ```

5. 컬렉션 게터 수정 -> 읽기 전용 프락시나 복제본 반환

    ```javascript
    // in Person Class
    get courses() {return this._courses.slice();}
    ```

6. 테스트
