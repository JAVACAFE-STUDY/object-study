# 의존성 관리하기

- 변경과 의존성

  - 의존성
    - 어떤 객체가 예정된 작업을 정상적으로 수행하기 위해 다른 객체를 필요로 하는 경우 두 객체 사이에 의존성이 존재한다고 말한다.
    - 두 객체 사이에 의존성이 있다는 것은 의존되는 요소가 변경될 때 의존하는 요소도 함께 변경될 수 있다는 것을 의미한다.
    - 즉 의존성은 함께 변경될 수 있는 가능성을 의미한다.
    - 의존성은 방향성을 가지며 단방향이다.


    - 영화 예매 시스템에서
      - `PeriodCondition.is_satisfied_by`는 `Screening`인스턴스에게 메시지를 전송하여 상영 시작 시간을 받아온다.
      - 즉 `PeriodCondition`은 `Screening`에게 의존한다.
      - 실행 시점에 `PeriodCondition`의 인스턴스가 정상적으로 동작하기 위해서는 `Screening`의 인스턴스가 존재해야 한다.
      - 만약 `Screening`의 인스턴스가 존재하지 않거나 상영 시작 시간을 알려달라는 메시지를 이해할 수 없다면 `is_satisfied_by` 메서드는 예상했던 대로 동작하지 않을 것이다.
      - 또한 `PeriodCondition`은 `DiscountCondition`을 상속 받으므로, `DiscountCondition`에게 의존한다.
      - 따라서 만약 `Screening`의 인터페이스가 변경되거나, `DiscountCondition`이 변경된다면 `PeriodCondition`도 변경되어야 한다.

  ```python
  class PeriodCondition(DiscountCondition):
  
      def __init__(self, day_of_week: str=None, start_time: time=None, end_time: time=None):
          self._day_of_week = day_of_week
          self._start_time = start_time
          self._end_time = end_time
  
      def is_satisfied_by(self, screening: Screening):
          return screening._when_screened.weekday() == self._day_of_week and \
                 self._start_time <= screening._when_screened() and \
                 self._end_time >= screening._when_screened()
  ```

    - 변경될 확률이 거의 없는 클래스라면 의존성이 문제가 되지 않는다.
      - 의존성이 불편한 이유는 그것이 항상 변경에 대한 영향을 암시하기 때문이다.
      - Java라면 JDK에 포함된 표준 클래스가 이 부류에 속한다.
      - 이런 클래스들에 대해서는 구체 클래스에 의존하거나 직접 인스턴스를 생성하더라도 문제가 없다.



- 의존성 전이(Transitive Dependency)
  - 의존성은 전이될 수 있다.
    - 의존성 전이가 의미하는 것은 A가 B에 의존하고 B가 C에 의존할 때 A는 B가 의존하는 C에 대해서도 자동적으로 의존하게 된다는 것이다.
    - 다만 의존성은 함께 변경될 수 있는 **가능성**을 의미하기 때문에 모든 경우에 의존성이 전이되는 것은 아니다.
  - 의존성이 전이될지 여부는 변경의 방향과 캡슐화의 정도에 따라 달라진다.
    - 내부 구현이 효과적으로 캡슐화될수록 의존성 전이가 발생하지 않을 확률이 높다.



- 런타임 의존성과 컴파일 타임 의존성
  - 런타임 의존성은 객체 사이의 의존성을, 컴파일 타임 의존성은 클래스 사이의 의존성을 주제로 한다.
    - 런타임 의존성과 컴파일 타임 의존성이 다를 수 있다.
    - 유연하고 재사용 가능한 코드를 설계하기 위해서는 두 종류의 의존성을 서로 다르게 만들어야 한다.
  - 영화 예매 시스템에서
    - `Movie`는 가격 계산을 위해 비율 할인 정책과 금액 할인 정책 모두를 적용할 수 있게 설계해야 한다.
    - 즉 `Movie`는 `AmountDiscountPolicy`와 `PercentDiscountPolicy` 모두와 협력할 수 있어야 한다.
    - 이를 위해 두 할인 정책 클래스가 `DiscountPolicy`라른 추상 클래스를 상속받게 한 후 `Movie`가 이 추상 클래스에 의존하도록 클래스 관계를 설계했다.
    - 중요한 점은 `Movie`에서 `AmountDiscountPolicy`와 `PercentDiscountPolicy`로 향하는 어떤 의존성도 존재하지 않는다는 것이다.
    - `Movie`는 오직 `DiscountPolicy`에만 의존한다.
    - 하지만 런타임에는 두 클래스 모두와 협력할 수 있다.
    - 코드 작성 시점의 `Movie` **클래스**는 두 클래스의 존재를 모르지만 실행 시점의 `Movie` **객체**는 두 클래스의 인스턴스와 협력할 수 있게 된다.
  - 실제로 협력할 객체가 어떤 것인지는 런타임에 해결해야한다.
    - 어떤 클래스의 인스턴스가 다양한 클래스의 인스턴스와 협력하기 위해서는 협력할 인스턴스의 구체적인 클래스를 알아서는 안 된다.
    - 클래스가 협력할 객체의 클래스를 명시적으로 드러내고 있다면 다른 클래스의 인스턴스와 협력할 가능성 자체가 없어진다.
    - 따라서 컴파일 타임 구조와 런타임 구조 사이의 거리가 멀면 멀수록 설계가 유연해지고 재사용 가능해진다.



- 컨텍스트 독립성
  - 클래스는 자신과 협력할 객체의 구체적인 클래스에 대해 알아서는 안 된다.
    - 구체적인 클래스를 알면 알수록 그 클래스가 사용되는 특정한 문맥에 강하게 결합되기 때문이다.
  - 구체 클래스에 대해 의존하는 것은 클래스의 인스턴스가 어떤 문맥에서 사용될 것인지를 구체적으로 명시하는 것과 같다.
    - 예를 들어 `Movie` 클래스 안에 `PercentDiscountPolicy` 클래스에 대한 컴파일타임 의존성을 명시적으로 표현하는 것은 `Movie`가 비율 할인 정책이 적용된 영화의 요금을 계산하는 문맥에서 사용될 것이라는 것을 가정하는 것이다.
  - 컨텍스트 독립성은 특정한 문맥에 결합되지 않는 다는 것을 의미한다.
    - 클래스가 특정한 문맥에 강하게 결합될수록 다른 문맥에서 사용하기는 더 어려워진다.
    - 설계가 유연해지기 위해서는 가능한 한 자신이 실행될 컨텍스트에 대한 구체적인 정보를 적게 알아야 한다.



- 의존성 해결하기

  - 컴파일타임 의존성은 구체적인 런타임 의존성으로 대체되어야 한다.

    - 예를 들어 `DiscountPolicy`에 의존하는 `Movie`의 컴파일타임 의존성은 런타임에 `AmountDiscountPolicy`와 `PercentDiscountPolicy` 같은 구체적인 런타임 의존성으로 대체된다.
    - 이처럼 컴파일타임의 의존성을 실행 컨텍스트에 맞는 적절한 런타임 의존성으로 교체하는 것을 의존성 해결이라고 부른다.

  - 의존성 해결에는 일반적으로 아래와 같은 방법들이 사용된다.

    - 객체를 생성하는 시점에 생성자를 통해 의존성 해결
    - 객체 생성 후 setter 메서드를 통해 의존성 해결
    - 메서드 실행 시 인자를 이용해 의존성 해결

  - 생성자를 사용하는 방식

    - 생성자의 인자로 객체를 전달 받아 인스턴스 변수에 할당하는 방식이다.

  - Setter 메서드를 사용하는 방식

    - 객체를 생성한 이후에 의존하고 있는 대상이 변경될 가능성이 있는 경우에 유용하다.
    - 그러나 객체가 생성된 후에 협력에 필요한 의존 대상을 설정하기 때문에 객체를 생성하고 의존 대상을 설정하기 전까지는 객체의 상태가 불완전할 수 있다는 단점이 있다.
    - 예를 들어 의존 대상이 설정되지 않은 상태에서, 의존 대상이 필요한 코드가 실행되면 에러가 발생할 수도 있다.

    - 더 좋은 방법은 객체 생성시에 먼저 의존성을 해결해서 완전한 상태의 객체를 생성하고, 이후 필요에 따라 setter를 이용해 의존 대상을 변경하는 것이다.

  - 메서드 실행 시 인자를 이용하는 방식

    - 협력 대상에 대해 지속적으로 의존 관계를 맺을 필요 없이 메서드가 실행되는 동안만 일시적으로 의존 관계가 존재해도 무방하거나 메서드가 실행될 때마다 의존 대상이 달라져야 하는 경우에 유용하다.
    - 그러나 매번 동일한 객체를 인자로 전달하고 있다면 생성자나 setter를 이용해서 의존성을 지속적으로 유지하는 방식으로 변경하는 것이 낫다.



- 의존성과 결합도

  - 객체들이 협력하기 위해서는 서로의 존재와 수행 가능한 책임을 알아야 한다.
    - 그리고 이런 지식들이 객체 사이의 의존성을 낳는다.
    - 의존성은 객체들의 협력을 가능하게 만드는 매개체라는 관점에서는 바람직한 것이다.
    - 문제는 의존성 그 자체가 아니라 의존성의 정도다.
  - 바람직한 의존성은 재사용성과 관련이 있다.
    - 어떤 의존성이 다양한 환경에서 재사용할 수 있다면 그 의존성은 바람직한 것이다.
    - `DiscountPolicy`라는 추상 클래스를 사용하는 기존의 코드는 코드의 변경 없이도 재사용이 가능하므로 바람직한 의존성이라고 볼 수 있다.
    - 어떤 의존성이 다양한 환경에서 클래스를 재사용할 수 없도록 제한한다면 그 의존성은 바람직하지 못한 것이다.
    - 즉 컨텍스트에 독립적인 의존성은 바람직한 의존성이고, 특정 컨텍스트에 결합된 의존성은 바람직하지 않은 의존성이다.
    - 다른 환경에서 재사용하기 위해 내부 구현을 변경하게 만드는 모든 의존성은 바람직하지 않은 의존성이다.
    - 예를 들어 아래 코드에서 `PercentDiscountPolicy`를 `AmountDiscountPolicy`로 변경하려면(즉, 다른 환경에서 재사용 하려면) `Movie`의 `_discount_policy` 인스턴스 변수에 `AmountDiscountPolicy`를 할당하도록 변경해야 한다(내부 구현을 변경해야 한다).

  ```python
  class Movie:
      def __init__(self, title: str, running_time: time, fee: Money, percent_discount_policy: PercentDiscountPolicy):
          # ...
          self._discount_policy = percent_discount_policy
  
      def calculate_movie_fee(self, screening: Screening):
          return self._fee.minus(self._discount_policy.calculate_discount_amount(screening))
  ```

  - 결합도
    - 어떤 두 요소 사이에 존재하는 의존성이 바람직할 때 두 요소가 느슨한 결합도(loose coupling) 또는 약한 결합도(weak coupling)를 가진다고 말한다.
    - 반대로 두 요소 사이의 의존성이 바람직하지 못할 때 단단한 결합도(tight coupling) 또는 강한 결합도(strong coupling)를 가진다고 말한다.
    - 어떤 의존성이 재사용을 방해한다면 결합도가 강하다고 표현하고, 재사용을 쉽게 허용한다면 결합도가 느슨하다고 표현한다.
  - 지식이 결합을 낳는다.
    - 서로에 대해 알고 있는 지식의 양이 결합도를 결정한다.
    - 한 요소가 다른 요소에 대해 더 많은 정보를 알고 있을수록 두 요소는 강하게 결합된다.
    - 결합도를 느슨하게 만들기 위해서는 협력하는 대상에 대해 필요한 정보 외에 최대한 감추는 것이 중요하다.



- 추상화에 의존하라

  - 추상화는 결합도를 낮추는 데 도움을 준다.
    - 추상화를 사용하면 현재 다루고 있는 문제를 해결하는 데 불필요한 정보를 감출 수 있다.
    - 따라서 대상에 대해 알아야 하는 지식의 양을 줄일 수 있기 때문에 결합도를 느슨하게 유지할 수 있다.
  - 영화 예매 시스템에서
    - `DiscountPolicy` 클래스는 `PercentDiscountPolicy`와  `AmountDiscountPolicy`가 어떤 방식으로 할인 요금을 계산하는지를 감춰주기 때문에 두 클래스의 추상화다.
    - 따라서 `Movie`의 관점에서 알아야 하는 지식의 양은 `PercentDiscountPolicy`와  `AmountDiscountPolicy` 보다 추상화 된 `DiscountPolicy`가 더 적다.
    - `Movie`와 `DiscountPolicy` 사이의 결합도가 더 느슨한 이유는 추상화에 의존하기 때문이다.
  - 추상 클래스와 구체 클래스
    - 추상 클래스는 메서드의 내부 구현과 자식 클래스의 종류에 대한 지식을 클라이언트로부터 숨길 수 있다.
    - 따라서 클라이언트가 알아야 하는 지식의 양이 더 적기 때문에 구체 클래스보다 추상 클래스에 의존하는 것이 결합도가 더 낮다.
    - 하지만 추상 클래스의 클라이언트는 여전히 협력하는 대상이 속한 클래스 상속 계층이 무엇인지에 대해서는 알고 있어야한다.
  - 인터페이스
    - 인터페이스에 의존하면 상속 계층을 모르더라도 협력이 가능해진다.
    - 인터페이스 의존성은 협력하는 객체가 어떤 메시지를 수신할 수 있는지에 대한 지식만을 남기기 때문에 추상 클래스 의존성보다 결합도가 낮다.
    - 이는 다양한 클래스 상속 계층에 속한 객체들이 동일한 메시지를 수신할 수 있도록 컨텍스트를 확장할 수 있게 해준다.

  - 실행 컨텍스트에 대해 알아야 하는 정보를 줄일수록 결합도가 낮아진다.
    - 결합도를 느슨하게 만들기 위해서는 구체적인 클래스보다 추상 클래스에, 추상 클래스보다 인터페이스에 의존하도록 만들어야한다.
    - 의존하는 대상이 더 추상적일 수록 결합도는 더 낮아진다.



- 명시적 의존성
  - 의존성의 대상을 생성자의 인자로 전달받는 방법과 생성자 안에서 직접 생성하는 방법의 차이
    - 퍼블릭 인터페이스를 통해 할인 정책을 설정할 수 있는 방법을 제공하는지 여부다.
    - 생성자의 인자로 선언하는 방법은 생성자라는 퍼블릭 인터페이스에 자신이 의존하는 대상에 대한 정보를 드러낸다.
    - 이는 의존성 해결 방법 중 setter 메서드를 사용하는 방식이나 메서드의 인자로 전달하는 방식의 경우에도 동일하다.
    - 모든 경우에 퍼블릭 인터페이스에 의존성이 명시적으로 노출된다.
    - 반면에 생성자 안에서 직접 생성하는 방식은 의존성이 퍼블릭 인터페이스에 표현되지 않는다.
  - 명시적 의존성과 숨겨진 의존성
    - 의존성을 퍼블릭 인터페이스에 드러내는 것을 명시적 의존성이라 한다.
    - 반면에 의존성을 퍼블릭 인터페이스에 드러내지 않는 것을 숨겨진 의존성이라 한다.
    - 의존성은 명시적으로 표현되어야 한다.
  - 명시적 의존성을 사용해야 하는 이유
    - 의존성이 명시적이지 않으면 의존성을 파악하기 위해 내부 구현을 직접 살펴볼 수 밖에 없다.
    - 또한 의존성이 명시적이지 않으면 클래스를 다른 컨텍스트에서 재사용하기 위해 내부 구현을 직접 변경해야 한다.
  - 숨겨진 의존성은 디버깅이 어렵고 이해하기 힘들다.
    - 테스트 코드를 작성하기도 까다롭다.
    - 가장 큰 문제는 의존성을 이해하기 위해 코드의 내부 구현을 이해할 것을 강요한다는 것이다.



- `new`는 해롭다

  - 대부분의 언어에서 클래스의 인스턴스를 생성할 수 있는 `new` 연산자를 제공한다.
  - 결합도 측면에서 `new`가 해로운 이유는 크게 두 가지다.
    - `new` 연산자를 사용하기 위해서는 구체 클래스의 이름을 직접 기술해야 하므로 `new`를 사용하는 클라이언트는 구체 클래스에 의존할 수밖에 없기에 결합도가 높아진다.
    - `new` 연산자는 생성하려는 구체뿐만 아니라 어떤 인자를 이용해 클래스의 생성자를 호출해야 하는지도 알아야 하므로, 클라이언트가 알아야 하는 지식의 양이 늘어나기에 결합도가 높아진다.
  - `Movie`가 `AmountDiscountPolicy`의 인스턴스를 직접 생성한다고 가정해보자.
    - `Movie`는 `AmountDiscountPolicy`의 인스턴스를 생성하기 위해 생성자에 전달되는 인자를 알고 있어야한다.
    - 이는 `Movie` 클래스가 알아야 하는 지식의 양을 늘리기 때문에 `Movie`가 `AmountDiscountPolicy`에 더 강하게 결합되게 만든다.
    - 또한 두 구체 클래스인 `SequenceCondition`과 `PeriodCondition`에도 직접 의존하도록 만든다.
    - 그리고 이 두 클래스를 생성하는 데 필요한 인자들의 정보에 대해서도 `Movie`를 결합시킨다.

  ``` python
  class Movie:
      def __init__(self, title: str, running_time: time, fee: Money):
          self._discount_policy: DiscountPolicy = AmountDiscountPolicy(Money.wons(800),
                                                     [SequenceCondition(1), SequenceCondition(2),
                                                      PeriodCondition(2, datetime(2024, 3, 27, 10), datetime(2024, 3, 27, 12))]
                                                  )
  ```

  - 해결 방법은 인스턴스를 생성하는 로직과 인스턴스를 사용하는 로직을 분리하는 것이다.
    - `AmountDiscountPolicy`를 사용하는 `Movie`는 인스턴스를 생성해서는 안 된다.
    - 단지 해당하는 인스턴스를 사용하기만 해야 한다.
    - 이를 위해 `Movie`는 이미 생성된 `AmountDiscountPolicy`의 인스턴스를 전달받아야 한다(그리고 인스턴스의 생성 책임은 클라이언트로 옮긴다).
    - 외부에서 인스턴스를 전달받는 방법은 앞에서 살펴본 의존성 해결 방법과 동일하다.
    - 생성자의 인자로 전달하거나, setter 메서드를 사용하거나, 실행 시에 메서드의 인자로 전달하면 된다.
    - 아래 코드는 생성자의 인자로 전달하는 방식을 사용했다.

  ```python
  class Movie:
      def __init__(self, title: str, running_time: time, fee: Money, discount_policy: DiscountPolicy):
          self._discount_policy = discount_policy
  ```

  - 협력하는 기본 객체를 설정하고자 할 경우 클래스 안에서 협력 대상을 직접 생성하는 것이 유용할 수 있다.
    - 예를 들어 `Movie`가 대부분의 경우에는 `AmountDiscountPolicy` 인스턴스와 협력하고 가끔싹만 `PercentDiscountPolicy` 인스턴스와 협력한다고 가정해보자.
    - 이런 경우 인스턴스를 생성하는 책임을 클라이언트로 옮긴다면 클라이언트들 사이에 중복 코드가 늘어나고 `Movie`의 사용성도 악화된다.
    - 이 문제를 해결하는 방법은 기본 객체를 생성하는 생성자를 추가하고 이 생성자에서 `DiscountPolicy`의 인스턴스를 인자로 받는 생성자를 체이닝 하는 것이다.
    - Python의 경우 `discount_policy` 인자에 기본값을 주면 된다.

  ```java
  public class Movie {
      private DiscountPolicy discountPolicy;
      
      public Movie(String title, Duration runningTime, Money fee) {
          this(title, runningTime, fee, new AmountDiscountPolicy(...));
      }
      
      public Movie(String title, Duration runningTime, Money fee, DiscountPolicy discountPolicy) {
          this.discountPolicy = discountPolicy
      }
  }
  ```

