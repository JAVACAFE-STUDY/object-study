# 1. 책임 주도 설계를 향해

## 따라야 하는 두가지 원칙

1. 데이터보다 행동을 먼저 결정하라
2. 협력이라는 문맥 안에서 책임을 결정하라

### 데이터보다 행동을 먼저 결정하라

- 데이터는 객체가 책임을 수행하는 데 필요한 재료를 제공할 뿐
- 질문의 순서를 바꾸어라
    - 데이터 중심의 설계: 이 객체가 포함해야 하는 데이터가 무엇인가 → 데이터를 처리하는데 필요한 오퍼레이션은 무엇인가
    - 책임 중심의 설계: 이 객체가 수행해야 하는 책임은 무엇인가 → 이 책임을 수행하는데 필요한 데이터는 무엇인가

### 협력이라는 문맥 안에서 책임을 결정하라

- 메시지가 객체를 선택하게 해야 한다.

## 책임 주도 설계

- 시스템이 사용자에게 제공해야 하는 기능인 시스템 책임을 파악
- 시스템 책임을 더 작은 책임으로 분할
- 분할된 책임을 수행할 수 있는 적절한 객체 또는 역할을 찾아 책임을 할당
- 객체가 책임을 수행하는 도중 다른 객체의 도움이 필요한 경우 이를 책임질 적절한 객체 또는 역할을 찾음
- 해당 객체 또는 역할에게 책임을 할당함으로써 두 객체가 협력하게 함

# 2. 책임 할당을 위한 GRASP 패턴

(General Responsibility Assignment Software Pattern - 일반적인 책임 할당을 위한 소프트웨어 패턴)

## 도메인 개념에서 출발하기

- 설계 시작 전 도메인의 개략적인 모습을 그려보는 것이 유용
    - 도메인 개념을 완벽하게 정리하는게 아님, 빠르게 설계와 구현을 진행하자
    - 올바른 도메인 모델이란 존재하지 않음

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/34ea7cb7-d202-4e90-926d-07b7bc241167/38ba44f4-229d-415d-b4bf-c20b077fe8f4/Untitled.png)

## 정보 전문가에게 책임을 할당하라

- 책임을 수행할 정보를 알고 있는 객체에게 책임을 할당하라. → INFORMATION EXPERT(정보 전문가) 패턴
    1. 메시지를 전송할 객체는 무엇을 원하는가?
    2. 메시지를 수신할 적합한 객체는 누구인가?
- 연쇄적인 메시지 전송과 수신을 통해 협력 공동체가 구성됨
    - 스스로 처리할 수 없는 작업이 있다면 외부로 새 메시지를 전송해야 하고, 이 메시지가 새로운 객체의 책임으로 할당됨

## 높은 응집도와 낮은 결합도

- 설계는 트레이드 오프 활동임, 동일한 기능을 구현할 수 있는 무수히 많은 설계가 존재함.
    - 다양한 대안이 존재한다면 응집도와 결합도의 측면에서 더 나은 대안을 선택하는 것이 좋음.
        - LOW COUPLING(낮은 결합도)패턴
        - HIGH COHESION(높은 응집도)패턴

## 창조자에게 객체 생성 책임을 할당하라 (CREATOR(창조자) 패턴)

**아래 조건을 최대한 많이 만족하는 B에게 객체 생성 책임을 할당**

- B가 A객체를 포함하거나 참조
- B가 A객체를 기록
- B가 A객체를 긴밀하게 사용
- B가 A객체를 초기화 하는데 필요한 데이터를 가지고 있음(B는 A에 대한 정보 전문가)

# 3. 구현을 통한 검증

- 메시지는 수신자인 `Movie` 가 아니라 송신자인 `Screening`의 의도를 표현
    - `Screening`을 구현하는 과정에서 `Movie` 에 전송하는 메시지의 시그니처를 `calculateMovieFee(Screening screening)` 으로 선언

- 변경의 이유가 하나 이상인 클래스가 드러내는 위험 징후의 패턴
    - 인스턴스 변수가 초기화 되는 시점을 살펴보라
        - 함께 초기화 되는 속성을 기준으로 코드를 분리
    - 메서드들이 인스턴스 변수를 사용하는 방식 살펴보라
        - 속성그룹과 해당 그룹에 접근하는 메서드 그룹을 기준으로 코드 분리

## 타입 분리하기

- 클래스를 분리함으로써 코드의 품질이 높아짐
    - 하지만 장점만 있진 않음 → 전체적인 결합도가 높아짐, 새로운 할인 조건 추가가 힘들어짐

## 다형성을 통해 분리하기

- 역할을 사용하면 객체의 구체적인 타입 추상화 가능
    - 역할에 대해서만 결합되도록 의존성을 제한할 수 있음
- 객체의 암시적인 타입에 따라 행동 분기 필요시, 암시적 타입을 명시적인 클래스로 정의 후 행동을 나눠 응집도문제 해결
    
    → i.e. 다형성(POLYMORPHISM) 패턴
    

## 변경으로부터 보호하기

- 변경을 캡슐화 하도록 책임을 할당 → PROTECTED VARIATIONS(변경 보호) 패턴
    - 변경이 될 가능성이 높다면 캡슐화 하라

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/34ea7cb7-d202-4e90-926d-07b7bc241167/cd323bc0-b7a1-453b-8aa6-7076e1f25a50/Untitled.png)

도메인의 구조가 코드의 구조를 이끈다. 구현을 가이드 할 수 있는 도메인 모델을 선택하라.

## 변경과 유연성

- 상속 대신 합성을 사용하라
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/34ea7cb7-d202-4e90-926d-07b7bc241167/cd959f27-242f-4349-bbb9-c2c860858525/Untitled.png)
    

- 코드의 구조가 바뀌면 도메인에 대한 관점도 함께 바뀐다.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/34ea7cb7-d202-4e90-926d-07b7bc241167/3f304c39-31bf-4669-80e3-3411f3819aac/Untitled.png)
    
    - 도메인 모델은 구현과 밀접한 관계를 맺어야 한다.
    - 도메인 모델은 코드에 대한 가이드를 제공할 수 있어야 하며 코드의 변화와 함께 변화해야한다.

# 4. 책임 주도 설계의 대안

- 책임과 객체 사이에서 방황중이라면 최대한 빠르게 목적한 기능을 수행하는 코드를 작성하는게 돌파구가 될 수 있다.
    - 일단 실행되는 코드를 얻고 난 후, 코드상 책임들을 올바른 위치로 이동하는 것

기존 데이터 지향 설계를 리팩터링한다.

## 메서드 응집도

- 긴 메서드(=몬스터 메서드)의 부정적인 영향
    - 로직이 한눈에 파악이 어려워 이해시 많은 시간 소요
    - 한 곳에서 많은 작업을 처리하여 변경 필요시 수정할 부분 찾기 어려움
    - 사이드 이펙트 날 확률 높음
    - 로직 일부 재사용 불가
    - 재사용을 위한 유일한 방법은 코드 복붙뿐, 중복코드 문제 생김

- 객체로 책임 분배시 가장 먼저 할 일은 메서드를 응집도 있는 수준으로 분해하는 것
    - 클래스의 길이가 길어지더라도 명확성의 가치가 클래스의 길이보다 중요 → 개선된 명확성이 모여 변경하기 쉬운 코드 생성
- 작고 명확하며 한가지 일에 집중하는 응집도 높은 메서드는 변경 가능한 설계를 이끌어내는 기반이 됨

## 객체를 자율적으로 만들자

- 메서드가 사용하는 데이터를 저장하고 있는 클래스로 메서드 이동하여 자율적인 객체를 만든다
    - 어떤 데이터를 사용하는지 쉽게 알 수 있는 방법은 접근자 메서드를 파악하면 됨
    - 메서드를 다른 클래스로 이동시킬 때 대체적으로 인자에 정의된 클래스 중 하나로 이동함

**책임주도 설계 방법에 익숙하지 않다면 일단 데이터 중심으로 구현 후, 이를 리팩터링 하는것도 방법이 될 수 있다.**

→ 더 훌룡한 결과물이 나을 수도 있음