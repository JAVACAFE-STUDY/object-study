# 1.  영화 예매 시스템

### **요구사항 정리**

- 영화
    - 영화에 대한 기본정보를 저장한다. ( 제목, 상영시간, 가격 정보 등 )
- 상영
    - 실제로 관객들이 영화를 관람하는 사건 ( 상영 일자, 시간, 순번 등 )
    - 사용자가 실제로 예매하는 대상은 영화가 아닌 상영이다.
- 하나의 영화는 하루 중 다양한 시간대에 걸쳐 한 번 이상 상영될 수 있다.
- 특정한 조건을 만족하는 예매자는 요금을 할인받을 수 있다. 할인액을 결정하는 두 가지 규칙이 존재한다.
    - `할인 조건 ( discount condition )`
        - 가격의 할인 여부를 결정한다.
        - `순서 조건 ( sequence condition )`
            - 상영 순번을 이용해 할인 여부를 결정하는 규칙
        - `기간 조건 ( period condition )`
            - 영화 상영 시작 시간을 이용해 할인 여부를 결정하는 규칙

  ***→ 다수의 할인 조건을 함께 지정할 수 있고, 순서 조건과 기간조건을 혼합할 수 있다.***
    - `할인 정책 ( discount policy )`
        - 할인의 요금을 결정한다.
        - `금액 할인 정책 ( amount discount policy )`
            - 예매 요금에서 일정 금액을 할인해주는 방식
        - `비율 할인 정책 ( percent discount policy )`
            - 정가에서 일정 비율의 요금을 할인해주는 방식

  **→ 영화별로 하나의 할인 정책만 할당할 수 있다.**

- 할인을 적용하기 위해서는 할인 조건과 할인 정책을 함께 조합해서 사용한다.
- 사용자가 예매를 완료하면 시스템은 예매 정보 ( 제목, 상영정보, 인원, 정가, 결제금액)을 생성한다.

# 2. 협력, 객체, 클래스

### **객체지향이란?**

- 객체를 지향하는것
    - 진정한 객체지향 패러다임으로의 전환은 클래스가 아닌 **객체에 초점을 맞출 때**에만 얻을 수 있다.

    1. 어떤 클래스가 필요한지를 고민하기 전에 **어떤 객체들이 필요한지** 고민해라
        - 클래스의 윤곽을 잡기 위해서는 어떤 객체들이 **어떤 상태와 행동을 갖는지** 먼저 결정해야 한다.
    2. 객체를 독립적인 존재가 아닌, **기능을 구현하기 위해 협력하는 공동체의 일원**으로 봐야한다.
        - 객체는 홀로 존재하는 것이 아니고 다른 객체에게 도움을 주거나 의존하면서 살아가는 **협력적인 존재**이다. 이렇게 사고함으로써 설계를 유연하고 확장 가능하게 만든다.
        - 객체들의 모양과 윤곽이 잡히면 **공통된 특성과 상태를 가진 객체들을 타입으로 분류**하고 **이 타입을 기반으로 클래스를 구현**하라.

# 3. 도메인 구조를 따르는 프로그램 구조

**도메인 ( domain )**

- 문제를 해결하기 위해 사용자가 프로그램을 사용하는 분야를 ***도메인*** 이라고 부른다.

**객체지향 패러다임이 강력한 이유**

- 요구사항을 분석하는 초기 단계부터 프로그램을 구현하는 마지막 단계까지 객체라는 동일한 추상화 기법을 사용할 수 있기 때문이다.

**일반적으로 클래스의 이름은 대응되는 도메인 개념의 이름과 동일하거나 적어도 유사하게 지어야 한다.**

클래스 사이의 관계도 최대한 도메인 개념 사이에 맺어질 관계와 유사하게 만들어서 프로그램의 구조를 이해하고 예상하기 쉽게 만들어야 한다.

# 4. 클래스 구현하기

클래스를 구현하거나 다른 개발자에 의해 개발된 클래스를 사용할 때 가장 중요한 것은 ***클래스의 경계를 구분 짓는 것이다.***

클래스의 내부와 외부로 구분되며 훌륭한 클래스를 설계하기 위한 핵심은 어떤 부분을 외부에 공개하고 어떤부분을 감출지 결정하는것이다.

왜 구분해야 할까?

→ 경계의 명확성이 객체의 자율성을 보장하기 때문이다.

→ 프로그래머에게 구현의 자유를 제공하기 때문이다.

### **자율적인 객체**

1. 객체는 **상태(state)** 와 **행동(behavior)** 을 함께 가지는 복합적인 존재다.
2. 객체가 스스로 판단하고 행동하는 **자율적인 존재**라는 것이다.

많은 사람들은 객체를 상태와 행동을 함께 포함하는 식별 가능한 단위로 정의한다.

객체지향 이전의 패러다임

- **데이터와 기능이라는 독립적인 존재를 서로 엮어 프로그램을 구성**했다.

이후, 객체지향의 패러다임

- 객체라는 단위 안에 데이터와 기능을 한 덩어리로 묶음으로써 문제의 영역의 아이디어를 적절하게 표현할 수 있게 했다.
- 캡슐화 : 데이터와 기능을 객체 내부로 함께 묶는 행위

`접근제어 ( access control )`

- 외부에서의 접근을 통제할 수 있는 매커니즘을 의미한다.

`접근 수정자 ( access modifier )`

- public, protected, private

⇒ 즉, 이 두가지의 경우 외부에서의 무분별한 접근을 제어하기위해 제공되는 요소들이라는 생각이 든다.

객체 내부에 대한 접근을 통제하는 이유는 객체를 **자율적인 존재**로 만들기 위해서다.

⇒ 나라는 사람, x라는 사람의 생각과 판단이 다르듯

“객체지향의 핵심은 **스스로 상태를 관리**하고, **판단**하고, **행동하는 자율적인 객체들의 공동체를 구성**하는 것이다.”

객체가 자율적인 존재로 우뚝서기 위해선 **외부의 간섭을 최소화** 해야 한다.

⇒ 외부의 간섭이 잦아지게 된다면, 결국 외부에 의한 결합이 강하게 형성되어지고 이는 이전 1장에서도 언급되었던 유연함과 거리가 멀어진다고 생각한다.

캡슐화와 접근제어는 객체를 두 부분으로 나눈다.

- **퍼블릭 인터페이스 ( public interface )**
    - 외부에서 접근 가능한 부분
- **구현 ( implementation )**
    - 외부에서는 접근 불가능하고 오직 내부에서만 접근 가능한 부분

**인터페이스와 구현의 분리 ( separation of interface and implementation ) 원칙**은 훌륭한 객체지향 프로그램을 만들기 위해 따라야 하는 핵심 원칙이다.

객체의 상태는 숨기고 행동만 외부에 공개가 이뤄져야한다.

### 프로그래머의 자유

프로그래머의 역할은 **클래스 작성자 ( class creator )** 와 **클라이언트 프로그래머 ( client programer )** 로 구분하는 것이 유용하다.

**클래스 작성자 ( class creator )**

- 새로운 데이터 타입을 프로그램에 추가하는 역할
- **목표**
    - 클라이언트 프로그래머에게 필요한 부분만 공개하고 나머지는 꽁꽁 숨겨야 하는것

**클라이언트 프로그래머 ( client programer )**

- 클래스 작성자가 추가한 데이터 타입을 사용한다.
- **목표**
    - 필요한 클래스들을 엮어 애플리케이션을 빠르고 안정적으로 구축하는것

**구현 은닉 ( implementation hiding )**

- 클라이언트 프로그래머에게 필요한 부분만 공개하고 나머지는 숨겨놓아 마음대로 접근할 수 없도록 방지
- 클라이언트 프로그래머에 대한 영향을 걱정하지 않고도 내부 구현을 마음대로 변경할 수 있는 방식

접근제어 메커니즘은 프로그래밍 언어 차원에서 클래스의 내부와 외부를 명확하게 경계지을 수 있게 하는 동시에 클래스 작성자가 내부구현을 은닉할 수 있게 해준다.

또한, 클라이언트 프로그래머가 실수로 숨겨진 부분에 접근하는 것을 막아준다.

설계가 필요한 진짜 이유는 변경을 관리하기 위해서라는 것을 기억해야 한다.

⇒ 확실한 공개할 영역을 배제한 내부 세부 구현의 경우 변경을 방지하는것이 핵심이라는 생각이 들었다.

# 5. 협력

시스템의 어떤 기능을 구현하기 위해 객체들 사이에 이뤄지는 상호작용

⇒ 해당 부분을 보며 비지니스를 처리하기 위한 Usecase 부분이 떠올랐다.

### **협력에 대한 짧은 이야기**

- 객체는 다른 객체의 인터페이스에 공개된 행동을 수행하도록 **요청(request)** 할 수 있다.
- 요청을 받은 객체는 자율적인 방법에 따라 요청을 처리한 후 **응답(response)** 한다.
- 객체가 다른 객체와 상호작용할 수 있는 유일한 방법은 **메시지를 전송(send a message)** 하는 것 뿐이다.
- 다른 객체에게 요청이 도착할 때 해당 객체가 **메시지를 수신(receive a message)** 했다고 이야기한다.
- 메시지를 수신한 객체는 스스로의 결정에 따라 자율적으로 메시지를 처리할 방법을 결정하며, 이러한 방법을 **메서드(method)** 라고 부른다.

메시지와 메서드를 구분에서부터 **다형성( polymorphism )** 의 개념이 출발한다.

### **추상 클래스**

추상 클래스인 `DiscountPolicy`는 할인 여부와 요금 계산에 필요한 전체적인 흐름은 정의하지만 실제로 요금을 계산하는 부분은 추상 메서드인 `getDiscountAmount()` 메서드에게 위임한다.

실제로는 `DiscountPolicy`를 상속받은 자식 클래스에서 오버라이딩한 메서드가 실행될 것이다.

이와 같이, 부모 클래스에 기본적인 **알고리즘 흐름을 구현**하고 **중간에 필요한 처리를 자식 클래스에게 위임하는 디자인 패턴**을 **TEMPLATE METHOD** 패턴이라고 한다.

# 6. 상속과 다형성

코드의 의존성과 실행 시점의 의존성이 서로 다를 수 있다는 것이다.

- 클래스 사이의 의존성, 객체 사이의 의존성이 동일하지 않을 수 있다.
- 유연하고, 쉽게 재사용할 수 있으며, 확장 가능한 객체지향 설계가 가지는 특징은 코드의 의존성과 실행 시점의 의존성이 다르다는 것

**하지만 !**

- 코드의 의존성과 실행시점의 의존성이 다르면 다를 수록 코드를 이해하기 어려워진다.
    - 코드를 이해하기 위해서는 코드 뿐만이 아닌 객체를 생성하고 연결하는 부분을 찾아야 하기 때문
- 코드의 의존성과 실행시점의 의존성이 다르면 다를수록 코드는 더 유연해지고 확장 가능해진다.

**설계의 트레이드 오프**

- **유연성이 증가**
    - 코드를 이해하고 디버깅하기는 점점 더 어려워진다
- **유연성 억제**
    - 코드를 이해하고 디버깅은 쉬워지지만 재사용성과 확장 가능성은 낮아진다

**상속**

- 객체지향에서 코드를 재사용하기 위해 가장 **널리 사용되는 방법**
- 클래스 사이의 관계를 설정하는 것만으로 기존 클래스가 가지고 있는 **모든 속성**과 **행동**을 새로운 클래스에 **포함** 시킬 수 있다.
- 기존 클래스를 기반으로 새로운 클래스를 **쉽고 빠르게 추가**할 수있는 **간편한 방법** 제공
- 이처럼 , 부모 클래스와 다른 부분만을 추가해 새로운 클래스를 쉽고 빠르게 만드는 방법을 **차이에 의한 프로그래밍**이라고 한다.

**다형성**

- 어떠한 메서드가 실행될 것인지 메시지를 수신하는 객체의 클래스에 따라 다양하게 달라진다.
- 메시지에 응답하기 위해 실행될 메서드를 컴파일 시점이 아닌 실행 시점에 결정한다.
- 메시지와 메서드를 실행 시점에 바인딩
    - **지연 바인딩 ( Lazy binding )**
    - **동적 바인딩 ( dynamic binding )**
- 컴파일 시점에 실행될 함수나 프로시저를 결정
    - **초기 바인딩 (early binding )**
    - **정적 바인딩 (static binding )**

### 상속보는 합성

합성

- 다른 객체의 인스턴스를 자신의 인스턴스 변수로 포함해서 재사용하는 방법

왜 상속 대신 합성을 선호할까?

- 상속은 두 가지 관점에서 설계에 안좋은 영향을 미치기 때문
    1. 캡슐화 위반
        - 부모 클래스의 구현이 자식 클래스에게 노출되므로 **캡슐화가 약화**된다.
        - 캡슐화의 약화는 자식 클래스가 부모 클래스에 **강하게 결합**되게 만드므로, **부모 클래스를 변경할 때 자식 클래스도 함께 변경될 확률이 높아진다**.
        - 결과적으로 상속을 과도하게 사용한 코드는 변경하기 어려워진다.

1. 상속은 설계를 유연하게 하지 못한다.
    - 상속은 부모 클래스와 자식 클래스의 관계를 컴파일 시점에 결정한다.
    - 실행 시점에 객체의 종류를 변경하는것이 불가능하다.

합성의 경우?

- 인터페이스에 정의된 메시지를 통해서만 재사용이 가능하기 때문에 구현을 효과적으로 캡슐화할 수 있다.
- 의존하는 인스턴스를 비교적 쉽게 교체할 수 있어 설계를 유연하게 만든다.

상속은 **클래스**를 통해 강하게 결합되는 데 비해 합성은 **메시지**를 통해 느슨하게 결합된다.

⇒ 합성의 좋은 예시는 일급 컬렉션을 생각했다.

⇒ 또한 Spring을 보면 **Template method 패턴**과, **Strategy pattern** 으로 구성이 되어 유기적으로 버전 업그레이드를 진행할때 최소한의 비용을 통해 개발이 진행될 수 있고 이는 현재까지 잘 유지되어오고 있는 요건중 하나란 생각이 들었다.