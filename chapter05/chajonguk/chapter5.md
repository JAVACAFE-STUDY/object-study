# 책임 할당하기

- 책임 주도 설계
  - 데이터 중심의 설계에서 책임 중심의 설계로 전환하기 위해서는 다음의 두 가지 원칙을 따라야한다.
    - 데이터보다 행동을 먼저 결정하라.
    - 협력이라는 문맥 안에서 책임을 결정하라.
  - 데이터보다 행동을 먼저 결정하라.
    - 객체에게 중요한 것은 데이터가 아니라 외부에 제공하는 행동이다.
    - 객체지향 설계에서 가장 중요한 것은 적절한 객체에게 적절한 책임을 할당하는 능력이다.
    - 그리고 적절한 책임을 할당할 수 있는 문맥을 제공하는 것이 협력이다.
  - 협력이라는 문맥 안에서 책임을 결정하라.
    - 객체에게 할당된 책임의 품질은 협력에 적합한 정도로 결정된다.
    - 객체에게 할당된 책임이 협력에 어울리지 않는다면 그 책임은 나쁜 것이다.
    - 협력을 시작하는 주체는 메시지 전송자이기 때문에 협력에 적합한 책임이란 메시지 수신자가 아니라 전송자에게 적합한 책임을 의미한다.
  - 객체가 메시지를 선택하는 것이 아니라 메시지가 객체를 선택하게 해야 한다.
    - 메시지가 존재하기 때문에 그 메시지를 처리할 객체가 필요한 것이다.
    - 메시지를 먼저 결정하기 때문에 송신자는 메시지 수신자에 대한 어떠한 가정도 할 수 없으므로, 전송자 관점에서 수신자는 깔끔하게 캡슐화된다.





## 책임 할당을 위한 GRASP 패턴

- GRASP(General Responsibility Assignment Sofware Pattern)
  - 크레이그 라만이 책임 할당 기법을 패턴 형식으로 제안한 것이다.
  - 객체에게 책임을 할당할 때 지침으로 삼을 수 있는 원칙들의 집합을 패턴 형식으로 정리한 것이다.



- 도메인 개념에서 출발하기
  - 설계를 시작하기 전에 도메인에 대한 대략적인 모습을 그려 보는 것이 유용하다.
    - 설계의 시작 단계에서 개념들의 의미와 관계를 정확히 정의할 필요는 없으며, 책임을 할당 받을 객체들의 종류와 관계에 대한 유용한 정보만 알아낼 수 있다면 충분하다.
  - 단 하나의 정답이라 부를 수 있는 도메인 모델이란 존재하지 않는다.
    - 올바른 구현을 이끌어낼 수만 있다면 모두 올바른 도메인 모델이라 할 수 있다.



- 정보 전문가에게 책임을 할당하라.
  - 객체는 어떤 책임을 수행하는 전문가라고 할 수 있으며, 책임을 수행할 정보를 알고 있는 객체에게 책임을 할당해야 한다.
    - GRASP에서는 이를 정보 전문가 패턴이라고 부른다.
    - 객체는 특정한 책임을 수행할 수 있는 행동과 행동을 수행하는 데 필요한 정보를 가지고 있다.
  - 책임 수행에 가장 적절한 정보를 가지고 있는 객체에게 책임을 할당해야 한다.



- 낮은 응집도와 높은 결합도
  - 낮은 결합도 패턴
    - 설계의 전체적인 결합도가 낮게 유지되게 책임을 할당해야 한다는 패턴이다.
    - 여러 가지 방식으로 설계가 가능하다면, 추가적인 의존성이 발생하지 않는 설계가 더 좋은 설계라고 할 수 있다.
  - 높은 응집도 패턴
    - 설계의 전체적인 응집도가 높게 유지되게 책임을 할당해야 한다는 패턴이다.
    - 여러 방식으로 설계가 가능하다면 높은 응집도를 유지할 수 있는 설계를 선택해야 한다.



- 창조자(CREATOR) 패턴
  - 창조자 패턴은 객체를 생성할 책임을 어떤 객체에게 할당할지에 대한 지침을 제공한다.
    - 객체 A를 생성해야 할 때 아래 조건을 최대한 많이 만족하는 B에게 객체 생성 책임을 할당해야한다.
    - B가 A객체를 포함하거나 참조한다.
    - B가 A객체를 기록한다.
    - B가 A 객체를 긴밀하게 사용한다.
    - B가 A 객체를 초기화하는 데 필요한 데이터를 가지고 있다(이 경우 B는 A에 대한 정보 전문가다)
  - 결합도를 높이지 않고 객체를 생성하기 위한 지침이다.
    - 다른 객체를 생성하기 위해서는 많건 적건 생성하려는 객체에 대한 정보를 알고 있어야한다.
    - 따라서 생성될 객체에 대해서 이미 잘 알고 있거나 해당 객체를 사용해야 하는 객체는 이미 생성 할 객체와 연결(결합)되어 있다고 볼 수 있다.
    - 생성 될 객체와 이미 연결되어 있는 객체에게 객체의 생성을 맡기면 결합도의 증가 없이 객체를 생성할 수 있게 된다. 



- 다형성 패턴
  - 객체의 타입에 따라 변하는 행동이 있다면 타입을 분리하고 변화하는 행동을 각 타입의 책임으로 할당하는 것을 GRASP에서는 다형성 패턴이라고 부른다.
  - 다형성 패턴은 객체의 타입을 검사해서 객체의 타입에 따라 여러 대안을 사용하는 조건적인 논리를 사용하지 말라고 경고한다.
    - 프로그램의 if ~ else 등의 조건 논리를 사용해서 설계한다면 새로운 변화가 일어난 경우 조건 논리를 수정해야 한다.
    - 이는 프로그램을 수정하기 어렵고 변경에 취약하게 만든다.
    - 대신 다형성을 이용해 새로운 변화를 다루기 쉽게 확장하라고 권고한다.





## 구현을 통한 검증

- 변경의 이유가 둘 이상인 클래스 찾아내기
  - 일반적으로 설계를 개선하는 작업은 변경의 이유가 하나 이상인 클래스를 찾는 것으로부터 시작하는 것이 좋다.
    - 문제는 클래스 안에서 변경의 이유를 찾는 것이 생각보다 어렵다는 것이다.
    - 다행히도, 변경의 이유가 하나 이상인 클래스에는 위험 징후를 또렷하게 드러내는 몇 가지 패턴이 존재한다.
  - 인스턴스 변수가 초기화되는 시점을 살펴본다.
    - 응집도가 높은 클래스는 인스턴스를 생성할 때 모든 속성을 함께 초기화한다.
    - 반면 응집도가 낮은 클래스는 객체의 속성 중 일부만 초기화되고 나머지는 초기화되지 않은 상태로 남겨진다.
    - 따라서 초기화되는 속성을 기준으로 코드를 분리해야한다.
  - 메서드들이 인스턴스 변수를 사용하는 방식을 살펴본다.
    - 모든 메서드가 객체의 모든 속성을 사용한다면 클래스의 응집도는 높다고 볼 수 있다.
    - 반면 메서드들이 사용하는 속성에 따라 그룹이 나뉜다면 클래스의 응집도가 낮다고 볼 수 있다.
    - 이 경우 클래스의 응집도를 높이기 위해서는 속성 그룹과 해당 그룹에 접근하는 메서드 그룹을 기준으로 코드를 분리해야 한다.



- `DiscountCondition`의 문제점

  ```python
  class DiscountConditionType(str, Enum):
      SEQUENCE = auto()
      PERIOD = auto()
  
  
  class DiscountCondition:
      
      def __init__(self, discount_condition_type: DiscountConditionType, sequence: int=None, 
                   day_of_week: str=None, start_time: time=None, end_time: time=None):
          self._discount_condition_type = discount_condition_type
          self._sequence = sequence
          self._day_of_week = day_of_week
          self._start_time = start_time
          self._end_time = end_time
  	
      def is_satisfied_by(self, screening: Screening):
          if self._discount_condition_type == DiscountConditionType.PERIOD:
              return self._is_satisfied_by_period(screening)
          return self._is_satisfied_by_sequence(screening)
      
      def _is_satisfied_by_period(self, screening: Screening):
          return screening._when_screened.weekday() == self._day_of_week and \
                 self._start_time <= screening._when_screened() and \
                 self._end_time >= screening._when_screened()
      
      def _is_satisfied_by_sequence(self, screening: Screening):
          return self._sequence == screening._sequence
  ```

  - `DiscountCondition`은 코드를 수정해야 하는 이유가 하나를 초과하는 변경에 취약한 코드다.
    - 새로운 할인 조건이 추가되면 `is_satisfied_by` 메서드의 조건문을 추가해야 하며, 새로운 할인 조건이 새로운 데이터를 요구하면 인스턴스 변수도 추가해야 한다.
    - 순번 조건을 판단하는 로직이 변경되면 `_is_satisfied_by_sequence` 메서드의 내부 구현을 수정해야 하며, 순번 조건을 판단하는 데 필요한 데이터가 변경되면 `DiscountCondition.sequence` 역시 변경되어야 한다.
    - 기간 조건을 판단하는 로직이 변경되면 `_is_satisfied_by_period` 메서드의 내부 구현을 수정해야 하며, 기간 조건을 판단하는 데 필요한 데이터가 변경되면 `DiscountCondition`의 `_day_of_week`, `_start_time`, `_end_time` 등도 변경해야 한다.
  - 변경의 이유에 따라 클래스를 분리해야한다.
    - `DiscountCondition`은 하나 이상의 변경 이유를 가지기 때문에 응집도가 낮다.
    - 응집도가 낮다는 것은 서로 연관성 없는 기능이나 데이터가 하나의 클래스에 뭉쳐져 있다는 것을 의미한다.
    - 따라서 낮은 응집도가 초래하는 문제를 해결하기 위해 변경의 이유에 따라 클래스를 분리해야 한다.
  - `DiscountCondtion`의 초기화 시점 살펴보기
    - 순번 조건을 표현하는 경우 `_sequence`는 초기화되지만, `_day_of_week`, `_start_time`, `_end_time`은 초기화되지 않는다.
    - 반면에 기간 조건을 표현하는 경우 `_day_of_week`, `_start_time`, `_end_time`은 초기화되지만, `_sequence`는 초기화되지 않는다.
    - 클래스의 속성이 서로 다른 시점에 초기화되거나 일부만 초기화된다는 것은 응집도가 낮다는 증거다.
    - 따라서 초기화되는 속성을 기준으로 코드를 분리해야한다.
  - `DiscountCondition`의 메서드들이 인스턴스 변수를 사용하는 방식 살펴보기
    - `_is_satisfied_by_sequence` 메서드와 `_is_satisfied_by_period` 메서드는 서로 다른 속성들을 사용한다.
    - 즉 `_is_satisfied_by_sequence`는 `_sequence`는 사용하지만 `_day_of_week`, `_start_time`, `_end_time`은 사용하지 않고,   `_is_satisfied_by_period`는 `_day_of_week`, `_start_time`, `_end_time`은 사용하지만 `_sequence`는 사용하지 않는다.
    - 따라서 속성 그룹과 해당 그룹에 접근하는 메서드 그룹을 기준으로 코드를 분리해야 한다.



- 변경 보호 패턴 적용을 위한 분리

  - `DiscountCondition`을 분리한다.
    - `DiscountCondition`의 가장 큰 문제는 순번 조건과 기간 조건이라는 두 개의 독립적인 타입이 하나의 클래스 안에 공존하고 있다는 점이다.
    - 따라서 아래와 같이 두 타입을 두 개의 클래스로 분리한다.

  ```python
  class PeriodCondition:
  
      def __init__(self, day_of_week: str=None, start_time: time=None, end_time: time=None):
          self._day_of_week = day_of_week
          self._start_time = start_time
          self._end_time = end_time
  
      def _is_satisfied_by(self, screening: Screening):
          return screening._when_screened.weekday() == self._day_of_week and \
                 self._start_time <= screening._when_screened() and \
                 self._end_time >= screening._when_screened()
  
  
  class SequenceCondition:
      
      def __init__(self, sequence: int=None):
          self._sequence = sequence
  
      def _is_satisfied_by(self, screening: Screening):
          return self._sequence == screening._sequence
  ```

  - 클래스를 변경하면 앞서 살펴본 문제점들이 모두 해결된다.
    - 두 개의 클래스는 자신의 모든 인스턴스 변수를 초기화한다.
    - 클래스 내의 모든 메서드는 동일한 인스턴스 변수 그룹을 사용한다.
    - 결과적으로 개별 클래스들의 응집도가 향상됐다.
  - 새로운 문제
    - 수정 전에는 `Movie`와 협력하는 클래스는 `DiscountCondition` 하나뿐이었다.
    - 그러나 수정 후에는 분리 된 두 개의 클래스 모두와 협력할 수 있어야 한다.
  - 새로운 문제를 해결하기 위한 적절하지 않은 방법
    - `Movie` 클래스 안에서 `SequenceCondtion`의 목록과 `PeriodCondition`의 목록을 따로 관리하면 된다고 생각할 수 있을 것이다.
    - 그러나 이는 `Movie`가 양쪽 클래스 모두와 결합되어 결합도가 증가한다. 
    - 또한 새로운 할인 조건을 추가하려면 `Movie`의 인스턴스 변수로 새로운 할인 조건의 목록을 추가해야 한다.
    - 그리고 새로운 할인 목록의 리스트를 이용해 할인 조건을 만족하는지 여부를 판단하는 메서드도 추가해야한다.
    - 마지막으로 이 메서드를 호출하도록 `is_discountable` 메서드를 수정해야한다.

  ```python
  class Movie:
      def __init__(self, title: str, running_time: time, fee: Money, movie_type: MovieType, 
                   discount_amount: Money, discount_percent: float,
                   period_conditions: list[PeriodCondition], sequence_conditions:list[SequenceCondition]):
          # ...
          self._period_conditions = period_conditions
          self._sequence_conditions = sequence_conditions
  
      def is_discountable(self, screening: Screening):
          return self._check_period_conditions(screening) or self._check_sequence_conditions(screening)
      
      def _check_period_conditions(self, screening: Screening):
          return any([discount_condition.is_satisfied_by(screening) 
                      for discount_condition in self._period_conditions])
  
      def _check_sequence_conditions(self, screening: Screening):
          return any([discount_condition.is_satisfied_by(screening) 
                      for discount_condition in self._sequence_conditions])
      
      # ...
  ```



- 정보 보호 패턴과 다형성 패턴 적용하기

  - 역할의 개념 적용하기
    - `Movie` 입장에서 보면 `SequenceCondition`과 `PeriodCondition`은 아무 차이도 없다.
    - 둘 모두 할인 여부를 판단하는 동일한 책임을 수행하고 있을 뿐이다.
    - 두 클래스가 할인 여부를 판단하기 위한 방법이 다르다는 사실은 `Movie` 임장에서는 그다지 중요하지 않다.
    - 할인 가능 여부를 반환해주기만 하면 `Movie`는 상관하지 않는다.
    - `Movie` 입장에서 `SequenceCondition`과 `PeriodCondition`이 동일한 책임을 수행한다는 것은 동일한 역할을 수행한다는 것을 의미한다.
    - 역할은 협력 안에서 대체 가능성을 의미하기 때문에 두 클래스에 역할의 개념을 적용하면 `Movie`가 구체적인 클래스는 알지 못한 채 오직 역할에 대해서만 결합되도록 의존성을 제한할 수 있다.
  - 역할을 사용하여 추상화하기
    - 역할을 사용하면 객체의 구체적인 타입을 추상화할 수 있다.
    - 역할을 대체할 클래스들 사이에 구현을 공유해야 한다면 추상 클래스를 사용하면 되고, 구현을 공유하지 않고 책임만 정의하고 싶다면 인터페이스를 사용하면 된다.

  ```python
  class DiscountCondition(ABC):
  
      @abstractmethod
      def is_satisfied_by(self, screening: Screening):
          ...
  
  
  class PeriodCondition(DiscountCondition):
  
      def __init__(self, day_of_week: str=None, start_time: time=None, end_time: time=None):
          self._day_of_week = day_of_week
          self._start_time = start_time
          self._end_time = end_time
  
      def is_satisfied_by(self, screening: Screening):
          return screening._when_screened.weekday() == self._day_of_week and \
                 self._start_time <= screening._when_screened() and \
                 self._end_time >= screening._when_screened()
  
  
  class SequenceCondition(DiscountCondition):
  
      def __init__(self, sequence: int=None):
          self._sequence = sequence
  
      def is_satisfied_by(self, screening: Screening):
          return self._sequence == screening._sequence
  ```

  - `Movie`가 전송한 메시지를 수신한 객체의 구체적인 클래스가 무엇인지에 따라 적절한 메서드가 실행된다.
    - 즉 `Movie`와 `DiscountCondition` 사이의 협력은 다형적이다.
    - 이제 `Movie`는 협력하는 구체적인 타입을 몰라도 상관 없다.
    - 협력하는 객체가 `DiscountCondition` 역할을 수행할 수 있고, `is_satisfied_by` 메시지를 이해할 수 있다는 사실만 알고 있어도 충분하다.



## 리팩터링

- 리팩터링(Refactoring)

  - 책임을 올바르게 할당하는 것은 어렵고 난해한 작업이다

    - 객체지향 프로그래밍 언어를 이용해 절차형 프로그램을 작성하는 대부분의 이유가 바로 책임 할당의 어려움에서 기인한다.
  - 책임 주도 설계에 익숙하지 않은 사람들은 아래와 같은 대안적인 방법을 사용하는 것도 좋다.

    - 먼저 빠르게 절차형 코드로 실행되는 프로그램을 작성한다.
    - 완성된 코드를 객체지향적인 코드로 변경한다.

    - 즉 일단 실행되는 코드를 얻고 난 후에 코드 상에 명확하게 드러나는 책임들을 올바른 위치로 이동시키는 것이다.
    - 이처럼 이해하기 쉽고 수정하기 쉬운 소프트웨어로 개선하기 위해 겉으로 보이는 동작은 바꾸지 않은 채 내부 구조를 변경하는 것을 리팩터링이라고 부른다.



- 메서드 응집도
  - 긴 메서드는 아래와 같은 이유로 코드의 유지 보수에 악영향을 미친다.

    - 어떤 일을 수행하는지 한눈에 파악하기 어려워 코드를 이해하는 데 시간이 오래 걸린다.
    - 하나의 메서드 안에서 너무 많은 작업을 처리하기에 변경이 필요할 때 수정할 부분을 찾기 어렵다.
    - 메서느 내부의 일부 로직만 수정하더라도 나머지 부분에서 버그가 발생할 확률이 높다.
    - 로직의 일부만 재사용하는 것이 불가능하다.
    - 코드를 재사용하는 유일한 방법은 원하는 코드를 복사해서 붙여넣는 것 뿐이므로 코드 중복을 초래하기 쉽다.
  - 메서드의 응집도를 높여야 하는 이유

    - 클래스의 응집도를 높이는 이유와 같이 메서드의 응집도를 높이는 이유도 변경과 관련이 깊다.
    - 즉 변경을 수용할 수 있는 코드를 만들기 위해서이다.
    - 응집도 높은 메서드는 변경되는 이유가 단 하나여야 한다.

    - 메서드가 잘게 나뉘어져 있으므로 다른 메서드에서 사용될 확률이 높다.
    - 고수준의 메서드를 볼 때 마치 주석들을 나열한 것처럼 보이기 때문에 코드를 이해하기도 쉽다.



- 객체를 자율적으로 만드는 방법
  - 어떤 메서드를 어떤 클래스로 이동시킬지 결정하기 위해서는 객체가 자율적인 존재여야 한다는 사실을 떠올리면 된다.
  - 자신이 소유하고 있는 데이터를 자기 스스로 처리하도록 만드는 것이 자율적인 객체를 만드는 지름길이다.
  - 따라서 메서드가 사용하는 데이터를 저장하고 있는 클래스로 메서드를 이동시킨다.