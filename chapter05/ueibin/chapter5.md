# 책임 주도 설계

데이터 중심의 설계에서 책임 중심의 설계로 전환하기 위해 따라야 하는 원칙

- **데이터 보다 행동을 먼저 결정하라**
    - 객체에게 중요한 것은 데이터가 아닌 외부에 제공
    - “이 객체가 포함해야 하는 데이터가 무엇인가”에 초점을 두는것이 아닌 “이 객체가 수행해야 하는 책임은 무엇인가?”를 결정한 후에 필요한 데이터를 결정해야 한다.
    - 즉, 객체의 행동(책임)을 결정한 이후 상태를 결정하도록 해야한다.

- **협력이라는 문맥 안에서 책임을 결정하라**
    - 책임은 객체의 입장이 아닌 객체가 참여하는 협력에 적합해야 한다.
    - 협력을 시작하는 주체는 메시지 전송자이므로, **메시지를 전송하는 클라이언트의 의도에 적합한 책임**을 할당해야 한다.

  ⇒ 비지니스 요구사항을 먼저 고려한 이후, 각 도메인들간의 협력을 고려를 우선적으로 해야한다라고 이해

    - 메시지를 결정 후 → 객체를 선택해야한다.

⇒ 즉, 두 원칙을 비교했을때 데이터가 아닌 책임과 협력에 초점을 맞춰 생각해야한다는걸 다시한번 강조한다고 느낌

단순히 메시지를 먼저 결정하는 것만으로 메시지 송신자는 **메시지 수신자에 대한 어떠한 가정도 할 수 없기에** 메시지 전송자 관점에서 메시지 수신자가 캡슐화할 수 있다.

# GRASP ( General Responsibility Assignment Software Pattern )

grasp : 꽉잡다, 이해하다 라는 의미

일반적인 책임 할당을 위한 소프트 웨어 패턴을 의미한다.

객체에게 책임을 할당할때 지침으로 삼을 수 있는 원칙의 집합을 패턴 형식으로 정리한것을 의미한다.

- 도메인 개념에서 출발
- 정보 전문가에게 책임을 할당해야한다.
    - 애플리케이션이 제공해야하는 기능을 애플리케이션의 책임으로 생각한다.
    - 이 책임을 애플리케이션에 대해 전송된 메시지로 간주하고 이 메시지를 책임질 첫 번째 객체로 선택하는 것으로 설계를 시작
        - 메시지를 전송할 객체는 무엇을 원하는지?
        - 메시지를 수신할 적합한 객체는 누구인지?


## Information Expert Pattern

- 정보를 가장 잘 알고 있는 전문가에게 책임을 할당하라

가장 먼저 애플리케이션이 제공해야 하는 기능을 애플리케이션의 책임으로 생각하는 것으로 시작한다.

이 책임을 애플리케이션에 대해 전송된 메시지로 간주하고 이 메시지를 책임질 첫번째 객체를 선택하자

- 메시지를 전송할 객체는 무엇을 원하는지?
- 메시지를 수신할 적합한 객체는 무엇인지?

이 질문들에 답하고 객체에게 책임을 할당하는 것은 책임을 수행할 정보를 알고 있는 객체에게 책임을 할당하는 것이다. GRASP에서는 이를 **INFORMATION EXPERT(정보 전문가)** 패턴이라고 부른다.

그 다음으론 작업의 흐름을 생각해보며 스스로 책임일 수 없는 작업이 있다면 외부에 요청하는 메시지를 생각해보면 이 메시지가 바로 새로운 객체의 책임으로 할당된다.

INFORMATION EXPERT 패턴을 따르는 것만으로도 **자율성이 높은 객체들로 구성된 협력 공동체를 구축**할 가능성이 높아진다.

## Low COUPLING 패턴 / HIGH COHESION 패턴

낮은 결합도와 높은 응집도를 갖는 설계를 선택하라

**LOW COUPLING 패턴**

- 설계의 전체적인 결합도가 낮게 유지되도록 **책임을 할당**하라
- 낮은 결합도는 모든 설계 결정에서 **염두에 둬야 하는 원리**이다.
- 설계 결정을 평가할 때 적용할 수 있는 **평가 원리다.**
- 현재의 책임 할당을 검토하거나 여러 설계 대안들이 있을 때 **낮은 결합도를 유지할 수 있는 설계를 선택**해라

**HIGH COHESION 패턴**

- 높은 응집도를 유지할 수 있게 **책임을 할당**하라
- 낮은 결합도처럼 높은 응집도 역시 모든 설계 결정에서 염두에 둬야 할 **원리**이다.
- 설계 결정을 평가할 때, **적용할 수 있는 평가 원리이다.**
- 현재의 책임 할당을 검토하고 있거나 여러 설계 대안 중 하나를 선택해야 한다면 **높은 응집도를 유지할 수 있는 설계를 선택**해라

# CREATOR 패턴

객체 A를 **생성할 책임**을 가질 객체는 다음과 같은 기준을 최대한 많이 만족하는 B에게 책임을 할당해라.

- B가 A를 포함하거나 참조한다.
- B가 A를 기록한다.
- B가 A를 긴밀하게 사용한다.
- B가 A를 초기화하는 데 필요한 데이터를 갖고 있다. (이 경우 B는 A에 대한 정보 전문가다.)

어쨌든 생성되는 객체와 연결되거나 관련될 필요가 있는 객체에 해당 객체를 생성할 책임을 맡기자.

# **책임 주도 설계의 대안**

책임 주도 설계에 익숙해지기 위해선 **부단한 노력과 시간**이 필요하다. 그러나 어느 정도 경험을 쌓은 숙련된 설계자조차 **적절한 책임과 객체를 선택하는 일에 어려움**을 느낀다.

책임과 객체 사이에서 방황할 때 돌파구를 찾기 위해 **선택하는 방법**은 **최대한 빠르게** 목적한 기능을 수행하는 **코드를 작성하는 것**이다.

여기서 주의할 점은, 코드를 수정한 후에 겉으로 드러나는 동작이 바뀌어서는 안 된다는 것!

캡슐화를 항상 시키고, 응집도를 높이고, 결합도를 낮춰야 하지만 동작은 그대로 유지해야 한다.

이처럼 이해하기 쉽고 수정하기 쉬운 소프트웨어로 개선하기 위해 **겉으로 보이는 동작은 바꾸지 않은 채 내부 구조를 변경**하는 것을 **리 펙터 링 (Refactoring)**이라고 한다.

# **메서드 응집도**

긴 메서드는 **다양한 측면**에서 코드의 유지보수에 **부정적인 영향**을 미치게 된다.

- 어떤 일을 수행하는지 한눈에 파악하기 어렵기에, 코드를 전체적으로 이해하는데 **너무 많은 시간이 걸린다.**
- 하나의 메서드 안에서 **너무 많은 직업을 처리하기 때문에** 변경이 필요할 때 수정해야 할 부분을 찾기 어렵다.
- 메서드 내부의 일부 로직만 수정하더라도 메서드의 나머지 부분에서 버그가 **발생할 확률이 높다.**
- 로직의 일부만 재사용하는 것이 **불가능**하다.
- 코드를 재사용하는 유일한 방법은 원하는 코드를 복사해서 붙여 넣는 것뿐이므로 코드 **중복을 초래하기 쉽다.**

# **객체를 자율적으로 만들자**

자신이 소유하고 있는 데이터를 **자기 스스로 처리하도록** 만드는 것이 **자율적인 객체를 만드는 지름길**이다.

해당 내용은 이전 계속 이야기했던 **캡슐화와도 연관성이 깊다**고 판단했다. 그만큼 **강조하고 싶은 부분**이 아닐까 싶다.

메서드가 사용하는 **데이터를 저장하고 있는 클래스로 메서드를 이동**시키면 된다.

만약 책임 주도 설계 방법에 익숙하지 않다면, 데이터 중심으로 구현한 후 이를 리 펙터 링 하더라도 **유사한 결과**를 얻을 수 있다.

처음부터 책임 주도 설계 방법을 따르는 것보다 동작하는 코드를 작성한 후에 리 펙터 링 하는 것이 더 **훌륭한 결과물**을 낳을 수 있다.

**캡슐화, 결합도, 응집도**를 이해하고 훌륭한 객체지향 원칙을 적용하기 위해 노력한다면 책임 주도 설계 방법을 단계적으로 따르지 않더라도 유연하고 깔끔한 코드를 얻을 수 있다.