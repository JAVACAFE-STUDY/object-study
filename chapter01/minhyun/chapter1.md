# 1. 티켓 판매 애플리케이션 구현

어떤 분야든 초기 단계에서는 아무것도 없는 상태에서 이론을 정립하기 보단, 실무를 관찰한 결과를 바탕으로 이론을 정립하는 것이 최선.

→ 하지만 이론도 소홀히 할 순 없다.. 실무가 다는 아님.

설계 분야에서 실무는 이론을 압도한다. **설계에 관해 설명할 때 가장 유용한 도구는 이론으로 덕지덕지 치장된 개념과 용어가 아니라 `코드` 그 자체이다.**

# 2. 무엇이 문제인가

- **모듈**
    - 크기와 상관 없이 클래스나 패키지, 라이브러리와 같이 프로그램을 구성하는 임의의 요소를 의미

- **소프트웨어 모듈의 세가지 목적**
    1. 실행 중에 제대로 동작하는 것
    2. 변경을 위해 존재하는 것
    3. 코드를 읽는 사람과 의사소통하는 것

## 예제로 나온 영화관 프로그램의 문제

1. 각 객체가 수동적으로 동작하고 있음
2. `Theater`의 `enter` 라는 하나의 메서드가 너무 많은 세부사항을 다루고 있다.
    1. 이해하는데 큰 부담이 된다.
3. 결합도가 높아 코드가 유연하지 못하다. (= 변경에 취약하다)
    1. 내부 객체를 변경할 경우 해당 객체에 의존성이 있는 객체도 함께 변경해야한다.
    2. 내부 로직이 지나치게 세부적인 사실에 의존해서 동작한다.

# 3. 설계 개선하기

### 개선 방법

- **코드의 변경이 용이하고, 이해가 쉽도록 개선한다.**
    - 각 객체들끼리 세세한 로직을 알지 못하도록 정보를 차단한다. (캡슐화)
        1. 각 객체를 자율적인 존재로 만든다.
        2. 인터페이스에 의존하도록 한다. (구현은 내부로 숨긴다.)

## 캡슐화와 응집도

- **응집도**
    - 밀접하게 연관된 작업만을 수행하고 연관성 없는 작업은 다른 객체에게 위임하는 객체를 `응집도` 가 높다고 한다.
    - 외부의 간섭을 최대한 배제하고 메시지를 통해서만 협력하는 자율적인 객체들의 공동체를 만드는 것

## 절차지향과 객체지향

### 절차적 프로그래밍

- 프로세스와 데이터를 별도의 모듈에 위치시키는 방식

단점

- 데이터의 변경으로 인한 영향을 지역적으로 고립시키기 어렵다.
    - 변경은 버그를 부르고 버그에 대한 두려움은 코드를 변경하기 어렵게 만든다.

**변경하기 쉬운 설계는 한 번에 하나의 클래스만 변경할 수 있는 설계이다.**

### 객체지향 프로그래밍

- 데이터와 프로세스가 동일한 모듈 내부에 위치하는 방식

**장점**

- 의존성이 적절히 통제되어 하나의 변경으로 생기는 사이드 이펙트를 효율적으로 억제한다.

**훌륭한 객체지향 설계의 핵심은 캡슐화를 이용해 의존성을 적절히 관리하여 객체 사이의 결합도를 낮추는 것이다.**

## 책임의 이동

- 객체지향은 스스로 책임을 수행하는 자율적인 객체들의 공동체를 구성함으로써 완성된다.
    - 데이터와 데이터를 사용하는 프로세스가 동일한 객체 안에 위치한다면 해당 방식일 가능성이 높다.
- 객체지향 설계의 핵심은 적절한 객체에 적절한 책임을 할당하는 것
    - 어떤 데이터를 가지느냐보다는 객체에 어떤 책임을 할당할 것인지 초점을 맞추자.
- 불필요한 의존성을 제거함으로써 객체 사이의 결합도를 낮추자.
    - 설계를 어렵게 만드는 것은 의존성이다.

### 훌륭한 객체지향 설계

**세부사항을 객체 내부로 캡슐화**하여 **객체의 자율성**을 높이고 **높은 응집도를 가지고 협력하도록** **최소한의 의존성만을 남기자.**

## 개선을 하다보니

개선을 하다보니 기존에 없었던 의존성이 추가되어 버렸다. 객체의 자율성을 높이기 위해 개선을 했지만 전체 설계 관점에서는 결합도가 상승한 것이다.

이 경우 우선순위를 두고 어느것이 중요한 것인지 가늠해보아야 한다.

### 여기서 얻은 사실

1. 어떤 기능을 설계하는 방법은 한가지 이상일 수 있다.
2. 설계는 트레이드오프의 산물이다.
    - i.e. 모든 사람들을 만족시킬 수 있는 설계를 만들 순 없다.

**설계는 균형의 예술이다. 훌륭한 설계는 적절한 트레이드오프의 결과물이라는 사실을 명심하자.**

## 의인화

현실에서는 수동적이더라도 객체지향의 세계에 들어오면 능동적이고 자율적인 존재로 변경된다.

이러한 원칙을 `의인화` 라고 부른다.

# 4. 객체지향 설계

- 설계란 코드를 배치하는 것이다.
    - 설계는 코드 작성의 일부이며 코드를 작성하지 않곤 검증이 불가능하다.
- 좋은 설계란 현재 요구하는 기능을 온전히 수행하면서 이후의 변경을 매끄럽게 수용하는 것이다.
- 변경에 유연하게 대응할 수 있는 코드란 이해하기 쉬운 코드다.
- 훌륭한 객체지향 설계는 협력 객체 사이의 의존성을 적절하게 관리하는 설계이다.

**데이터와 프로세스를 하나의 덩어리로 모아, 협력하는 객체들 사이의 의존성을 적절하게 조절하여 변경에 용이한 설계를 만드는 것.**