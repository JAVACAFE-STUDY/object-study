## 1. 알게된 점, 배운 점

- 상속은 타입 계층을 구조화하기 위해 사용해야 한다.
- 다형성은 하나의 추상 인터페이스에 대해 코드를 작성하고 해당 추상 인터페이스에 대해 서로 다른 구현을 연결할 수 있는 능력이다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/34ea7cb7-d202-4e90-926d-07b7bc241167/7bd26630-8db1-498e-93f1-9238f11a45b6/Untitled.png)

- **오버로딩 다형성** → 하나의 클래스 안에 동일한 이름의 메서드가 존재하는 경우
- **강제 다형성** → 언어가 지원하는 자동 타입 변환이나 사용자 커스텀 타입 변환을 이용해 동일 연산자를 다양한 타입에 사용할 수 있는 방식 (e.g. ‘+’)
- **매개변수 다형성** → 제네릭 프로그래밍과 관련. 임의의 타입으로 선언 후 사용 시점에 구체적 타입으로 지정하는 방식 (e.g. List)
- **포함 다형성(=서브타입 다형성)** → 메시지는 동일하지만 수신 객체타입에 따라 수행 행동이 달라지는 능력

- **클래스와 인스턴스의 개념적인 관계**
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/34ea7cb7-d202-4e90-926d-07b7bc241167/d10c482a-6f4d-4244-8665-0ca21a400d02/Untitled.png)
    
    - 인스턴스는 두개가 생성됐지만 클래스는 단 하나만 메모리에 로드됐다.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/34ea7cb7-d202-4e90-926d-07b7bc241167/dbd730b3-c512-45e2-93c4-9a4c52bdddc1/Untitled.png)
    

- **동적 메서드 탐색 원리**
    1. 자동적인 메시지 위임
        1. 상속 계층을 따라 부모클래스에게 처리를 위임하고, 클래스 사이의 위임은 프로그래머 개입 없이 자동으로 이뤄짐.
    2. 메서드를 탐색하기 위한 동적인 문맥 사용
        1. 메시지 주신시 실제 어떤 메서드를 실행할지 결정하는건 실행 시점에 이뤄지며, 메서드 탐색시 self 참조를 이용해 결정한다.

```kotlin
Lecture lecture = new GradeLecture(...);
lecture.evaluate();
```

- 이 경우 `Lecture`에 정의된 메서드가 아닌 실제 객체를 생성한 `GradeLecture`에 정의된 메서드가 실행된다.
    - 동적 메서드 탐색은 self 참조가 가리키는 객체의 클래스인 `GradeLecture` 에서 시작됨.

- `self 참조` 개념을 잘 이해해야 한다.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/34ea7cb7-d202-4e90-926d-07b7bc241167/5f417bbe-4615-4c0a-b842-811e4822a2c0/Untitled.png)
    
    - `GradeLecture` 내에 `stats` 는 없고, `getEvaluationMethod()` 가 오버라이딩 되어있을 때 `GradeLecture.stat()` 메서드를 실행하면 해당 메서드 내  `getEvaluationMethod()` 실행 구문은 `Lecture` 클래스가 아닌 `GradeLecture` 클래스의 메서드를 실행한다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/34ea7cb7-d202-4e90-926d-07b7bc241167/627d155f-3ff2-4029-95da-51628e509fb2/Untitled.png)

- `super` 참조의 정확한 의도는 ‘지금 이 클래스의 부모 클래스에서부터 메서드 탐색을 시작하세요’다.
    - → **super 전송**

- **처리를 요청할때**
    - `self 참조` 를 전달하지 않는 경우는 ‘포워딩’
    - `self 참조` 를 전달하는 경우엔 ‘위임’

## 2. 궁금한 점

## 3. 같이 논의해보고 싶은 내용, 공유하고 싶은 내용