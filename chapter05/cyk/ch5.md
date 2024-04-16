### 책임 할당하기

4장에서는 데이터 중심 접근 했을때의 문제점을 알아보고, 책임에 초점을 맞추어서 설계를 하는것이 캡슐화를 위반하지 않고, 결합도를 낮추며, 응집도를 높일 수 있다고 함.

책임에 초점을 맞춰 설계할때 직면하는 가장 큰 문제는 어떤 객체에게 어떤 책임을 할당 할지를 결정하기 어렵다는 점이다.

동일한 문제를 해결할수 있는 다양한 책임 할당 방법이 존재해서 어떤 방법이 최선인지는 상황과 문맥에 따라 달라져서 책임 할당 과정은 트레이드 오프 활동이다.

**GRASP 패턴**을 통해 책임 할당의 어려움을 해결할 답을 제시해줄 것이다.

## **1. 책임 주도 설계를 향해**

데이터 중심의 설계에서 책임 중심의 설계로 전환하기 위한 두 원칙

- 데이터보다 행동을 먼저 결정하라
- 협력이라는 문맥(context) 안에서 책임을 결정하라

### **데이터보다 행동을 먼저 결정하라**

우리에게 필요한 것은 객체의 데이터에서 행동으로 무게 중심을 옮기는 기법이다.

가장 기본적인 해결 방법은 객체를 설계하기 위한 질문의 순서를 바꾸는 것이다.

- 데이터 중심
    - "이 객체가 포함해야 하는 데이터가 무엇인가?" 를 결정한 후 "데이터 처리에 필요한 오퍼레이션 결정"
- 책임 중심
    - "이 객체가 수행해야 하는 책임은 무엇인가?"를 결정한 후 "이 책임을 수행하는데 필요한 데이터는 무엇인가"

> “이 객체가 포함해야하는 데이터가 무엇인가” → “이 객체가 수행해야 하는 책임은 무엇인가?” 이라는 질문을 통해서 행동을 먼저 결정하고, 그 후 상태를 결정해야한다.
>

### **협력이라는 문맥 안에서 책임을 결정하라**

비록 객체 입장에서는 할당된 책임이 조금 어색해 보이더라도, 협력에 적합하다면 그 책임은 적합한 책임이다.

**책임은 객체의 입장이 아니라, 객체가 참여하는 협력에 적합해야 한다**.

협력을 시작하는 주체는 **메시지 전송자**이기 때문에, 메시지를 전송하는 **클라이언트의 의도**에 적합한 책임을 할당해야 한다.

- 메시지(클라이언트의 의도)를 결정한 후에, 이 메시지를 받을 객체를 선택할 것
- 메시지가 존재하기 때문에 메시지를 처리할 객체가 필요한것이다.

**"메시지를 전송해야 하는데 누구에게 전송해야 하지?"**

- **설계의 핵심 질문을 통해 메시지 기반 설계를 시작 할 수 있게 해준다.**

메시지를 수신하기로 결정된 객체는 메시지를 처리할 '책임'을 할당받게 되는것이다.

사실 이 흐름은 3장 내용의 책임 주도 설계의 핵심과 같은 내용임.

### **책임 주도 설계**

- 시스템이 사용자에게 제공할 기능인 시스템 책임을 파악한다
- 시스템의 책임을 더 작은 책임으로 분할한다
- 분할된 책임을 수행할 수 있는 적절한 객체를 찾아 할당한다.
- 객체가 책임을 수행하는 도중 다른 객체의 도움이 필요하다면, 이를 책임질 적절한 객체를 다시 찾는다.
- 다른 객체에게 책임을 할당함으로써 두 객체가 협력하게 된다.

## **2. 책임 할당을 위한 GRASP 패턴**

GRASP는 "General Responsibility Assignment Software Pattern(일반적인 책임 할당을 위한 소프트웨어 패턴)"의 약자이다.

시작은 도메인 안에 존재하는 개념들을 정리하는 것으로부터 시작된다.

## **도메인 개념에서 출발하기**

설계 초기에 간략한 도메인 모습을 그리는것이 좋다.

이유는 도메인 개념들을 책임 할당의 대상으로 사용하면 코드에 도메인의 모습을 투영하기가 수월해진다.
![img.png](img.png)

설계 초기에는 의미와 관계가 완벽할 필요가 없고, 책임을 할당받을 객체들의 종류와 관계에 대한 정보를 제공할 수 있다면 충분하다.

- 중요한것은 설계를 시작하는것이지 완벽하게 정리하는 것이 아니고 빠르게 설계와 구현을 진행하는 것이 좋음

### **정보 전문가에게 책임을 할당하라**

**애플리케이션이 제공해야 하는 기능을 애플리케이션의 책임**으로 생각하고, 이 책임을 애플리케이션에 대해 전송된 메시지로 간주하여 이 **메시지를 책임질 첫 객체를 선택하는것이 설계의 시작**이다.

예시에서 시스템이 제공하는 기능은 '영화 예매' 이다.

이를 애플리케이션의 책임으로 간주하고, 이 책임을 수행하는데 필요한 메시지를 결정해야 한다.

**메시지를 전송하는 객체의 의도**를 반영해 전달해야 한다.

- **메시지를 전송할 객체는 무엇을 원하는가? → 영화를 예매하는것**
- 따라서 메시지의 이름으로는 **예매하라**가 적당하다.

 ![img_1.png](img_1.png)

그 다음, 메시지를 받을 객체를 선택해야 한다. 두번째 질문은 다음과 같다.

**메시지를 수신할 적합한 객체는 누구인가?**

객체에게 책임을 할당하는 첫 원칙은, **책임을 수행할 정보를 알고있는 객체(정보 전문가)에게 책임을 할당**하는 것이다.

- 정보를 행동보다 먼저 알고 있다? 여기서 말하는 정보는 데이터와 다르다. 책임을 수행하는 객체가 정보를 '알고' 있다고 해서 그 정보를 '저장'할 필요는 없다.
- 어떠한 방식이건 정보 전문가가 데이터를 반드시 저장하고 있을 필요는 없다는 사실을 이해해야 한다.

**정보 전문가** 패턴에 따르면, 예매하는데 필요한 정보를 가장 많이 아는 객체에게 ***예매하라*** 메시지를 처리할 책임을 할당해야 한다.

상영(Screening)은 영화에 대한 상영시간, 상영 순번 처럼 예매에 필요한 다양한 정보를 알고있다.

따라서 영화 예매를 위한 **정보 전문가** 이다.

여기서는 Screening에게 책임을 할당하자!

 ![img_2.png](img_2.png)

만약 스스로 처리할 수 없는 작업이 있다면, 외부에 도움을 요청해야 한다.

- 이 **요청이 외부로 전송해야 하는 새로운 메시지가 되고, 최종적으로 이 메시지가 새로운 객체의 책임으로 할당**된다.

***예매하라*** 라는 메시지를 완료하기 위해 가격을 계산해야 한다.

따라서 영화 한편의 가격을 알아야 한다. 하지만 Screening은 가격을 계산하는 데 필요한 정보가 없다.

- 외부에 도움 요청하게 되고, 새로운 메시지인 ***가격을 계산하라*** 가 나오게 된다.

 ![img_3.png](img_3.png)

다음으로 다시 메시지를 수신할 정보 전문가 객체를 선택한다.

영화의 가격을 알고있는 Movie가 **정보 전문가**가 된다.

![img_4.png](img_4.png)

요금 계산을 위해 먼저 영화가 할인 가능한지를 판단한 후, 할인 정책에 따라 할인 요금을 빼서 계산해야 한다.

이중 할인 조건에 따라 영화가 할인 가능한지 판단하는 것은 Movie 스스로 처리할수가 없다. => 외부에 도움 요청

Movie는 ***할인 여부를 판단하라*** 라는 메시지를 전송하게 된다.

이후 할인 여부를 판단하는데 필요한 많은 정보를 알고 있는 DiscountCondition이 책임을 할당받게 된다.

 ![img_5.png](img_5.png)

Movie는 DiscountCondition에 전송한 ***할인 여부를 판단하라*** 메시지의 결과로 할인 가능 여부를 반환받게 된다.

할인 가능 여부 중 가능한 조건이 있다면 금액 할인 정책, 비율 할인 정책의 계산식에 따라 계산되어 요금을 반환한다.

### **높은 응집도와 낮은 결합도**

설계는 트레이드오프 활동이고, 정보 전문가 패턴이외 다른 책임 할당 패턴을 생각해볼 수 있다.

예를 들어 Movie 대신 Screening이 직접 DiscountCondition과 협력하게 하는 것은 어떨까?

 ![img_6.png](img_6.png)

위 설계는 기능적으로는 Movie -> DiscountCondition 의 상호작용과 동일하다.

차이는 **응집도와 결합도**에 있다.

책임을 할당할 수 있는 다양한 대안이 있다면, 응집도와 결합도의 측면에서 더 좋은 대안을 선택해야 한다.

**결합도**

맨 위에서 살펴본 **도메인 모델** 을 보면, Movie는 DiscountCondition의 목록을 속성으로 포함하고 있다.

- 따라서 Movie와 DiscountCondition은 이미 결합되어 있기 때문에 결합도를 추가하지 않고도 협력을 완성할수 있다.

하지만 Screening과 DiscountCondition는 **새로운 결합도가 추가**된다. (결합도 증가)

따라서, Movie 와 DiscountCondition 의 협력이 더 좋은 설계가 된다.

**응집도**

만약 Screening과 DiscountCondition이 협력하게 되면 **Screening은** 영화 요금 계산과 관련된 **책임 일부를 떠안아야 할 것**이다.

- 만약 예매 요금을 계산하는 방식이 변경되면, Screening도 함께 변경해야 한다.

### **창조자에게 객체 생성 책임을 할당하라**

영화 예매 협력의 최종 결과로 Reservation 인스턴스를 반환해야 한다.

- 이는 협력에 참여하는 누군가가 Reservation 인스턴스를 생성할 책임을 할당받아야 한다는 의미이다.

GRASP의 **CREATOR(창조자) 패턴**은 이같은 경우 사용할 수 있는 책임 할당 패턴으로서 객체를 생성할 책임을 어떤 객체에게 할당할지 결정하는데 지침을 제공한다.

객체 A를 생성할때, 아래 조건을 최대한 많이 만족하는 B에게 객체 생성 책임을 할당하라.

- B가 A 객체를 포함하거나 참조한다
- B가 A 객체를 기록한다
- B가 A 객체를 긴밀하게 사용한다
- B가 A 객체를 초기화하는데 필요한 데이터를 가지고 있다.(이 경우 B는 A에 대한 정보 전문가다)

결국 **CREATOR 패턴**의 의도는 어떤 방식으로든 생성되는 객체와 연결되어있거나, 관련될 필요가 있는 객체에 해당 객체를 생성하도록 책임을 맡기는 것이다.  

- **이미 두 객체는 서로 결합되어있다. 따라서 전체적인 결합도에 영향을 미치지 않는다**. (**낮은 결합도 유지**)

Screening은 예매 정보(Reservation)를 생성하는 데 필요한 영화, 상영시간, 순번 등의 정보에 대한 전문가이다.

- 따라서, Screening을 Reservation의 CREATOR로 선택하는것이 적절하다.

 ![img_7.png](img_7.png)

## **3. 구현을 통한 검증**

협력의 관점에서 첫 책임을 할당받는 Screening은 다음과 같이 구현된다.

```java
public class Screening {
    private Movie movie;
    private int sequence;
    private LocalDateTime whenScreened;

    public Reservation reserve(Customer customer, int audienceCount){
        return new Reservation(customer, this, calculateFee(audienceCount), audienceCount);
    }

    private Money calculateFee(int audienceCount){
        return movie.calculateMovieFee(this).times(audienceCount);
    }

    public LocalDateTime getWhenScreened() {
        return whenScreened;
    }

    public int getSequence() {
        return sequence;
    }
}
```

Screening이 Movie에 전송하는 메시지의 시그니처를 calculateMovieFee(Screening screening)으로 선언했다.

이 메시지는 수신자인 Movie가 아니라, 송신자인 Screening의 의도를 표현한다.

**여기서 핵심은, Screening은 Movie의 내부 구현에 대한 어떤 사전 지식도 없이 전송할 메시지를 결정했다는 점 이다.**

Movie의 구현을 고려하지 않고 필요한 메시지를 결정하면 Movie 내부 구현을 깔끔하게 캡슐화 할 수 있다.

- 메시지가 변경되지 않으면, Movie의 수정이 Screening에 영향을 미치지 않는다.

Movie는 calculateMovieFee를 응답하기 위해 구현해야한다. (코드 생략)

- 여기서, 할인 여부를 판단하기 위한 정보전문가에게 메시지를 결정(isSatisfiedBy)하고 전송한다.

**할인 여부를 판단하기 위한 isSatisfiedBy를 구현한 DiscountCondition 코드**

```java
public class DiscountCondition {
    private DiscountConditionType type;
    private int sequence;
    private DayOfWeek dayOfWeek;
    private LocalTime startTime;
    private LocalTime endTime;

    public boolean isSatisfiedBy(Screening screening){
        if(type == DiscountConditionType.PERIOD){
            return isSatisfiedByPeriod(screening);
        }

        return isSatisfiedBySequence(screening);
    }

    private boolean isSatisfiedByPeriod(Screening screening) {
        return dayOfWeek.equals(screening.getWhenScreened().getDayOfWeek()) &&
                startTime.compareTo(screening.getWhenScreened().toLocalTime()) <= 0 &&
                endTime.compareTo(screening.getWhenScreened().toLocalTime()) >= 0;
    }

    private boolean isSatisfiedBySequence(Screening screening) {
        return sequence == screening.getSequence();
    }
}
```

이렇게 구현할 수 있지만 이 코드에서는 문제점이 몇가지 있음.

### **DiscountCondition 개선하기**

지금의 DiscountCondition은 서로 다른 3가지 이유로 변경될수가 있다. (**SRP(단일 책임 원칙) 위배**)

- 새로운 할인 조건을 추가할 경우 => isSatisfiedBy 안에 if ~ else 구문을 수정해야함.
- 순번 조건을 판단하는 로직 변경 => isSatisfiedBySequence 메서드 내부 구현을 수정해야 한다.
- 기간 조건을 판단하는 로직이 변경되는 경우 => isStatisfiedByPeriod 메서드의 내부 구현을 수정해야 한다.

DiscountCondition은 하나 이상의 변경 이유를 가지기 때문에 응집도가 낮다.

- **변경의 이유에 따라 클래스를 분리해야 한다**.

설계를 개선하는 작업은 변경의 이유가 하나 이상인 클래스를 찾는 것으로부터 시작하는 것이 좋지만, 변경의 이유를 찾는 것이 생각보다 어려울 수 있음.

- **그러나 변경의 이유를 파악할 수 있는 패턴이 존재함.**

1. **인스턴스 변수 초기화 시점 살피기**

**인스턴스 변수가 초기화되는 시점** 을 살펴보는것이다.

응집도가 높은 클래스는 인스턴스를 생성할때 모든 속성을 함께 초기화한다.

반면, 응집도가 낮은 클래스는 일부만 초기화 하고, 나머지는 초기화 하지 않은 상태로 둔다.

따라서 **함께 초기화 되는 속성을 기준으로 코드를 분리**해야 한다.

ex) DiscountCondition에서는 sequence ↔ dayOfWeek, startTime, endTime의 초기화 시점이 다르다.

1. **메서드가 인스턴스 변수를 사용하는 방식**

모든 메서드가 객체의 모든 속성을 사용한다면 클래스의 응집도는 높다고 볼 수 있다.

반면 메서드들이 사용하는 속성에 따라 그룹이 나뉜다면 클래스의 응집도는 낮다고 볼 수 있다.

ex) isSatisfiedBySequence 와 isStatisfiedByPeriod에서 사용되는 속성이 다르다.

### **타입 분리하기**

DiscountCondition의 가장 큰 문제는 **순번 조건, 기간 조건**이라는 2개의 타입이 하나의 클래스에 공존하고 있다는 점이다.

- sequenceCondition 과 periodCondition이라는 2개의 클래스로 분리할 수 있다.

sequence 속성만 사용하는 메서드 => SequenceCondition 이동

dayOfWeek, startTime, endTime => PeriodCondition 이동

2개의 클래스로 분리함으로써 코드의 품질을 높일 수 있다.

하지만 원래 Movie와 협력하는 클래스는 DiscountCondition 하나였지만, 개선후에는 Movie는 sequenceCondition 과 periodCondition 이라는 2개의 서로 다른 클래스의 인스턴스와 협력할수 있어야한다.

 ![img_7.png](img_7.png)

**해결방법**

1. 목록을 따로 유지한다.

- 결합된 클래스가 1개에서 2개로 증가하여 결합도가 높아짐.
- 새로운 할인 조건 추가가 매우 힘들어졌다. List도 추가해야 하고, 만족 여부 메서드도 추가해야 한다.

2. **다형성을 통해 분리**

Movie 입장에서는 사실 둘다 **할인 여부를 판단하는 동일한 책임**을 수행할 뿐이다.

할인 가능 여부를 반환하기만 하면 Movie는 객체가 sequenceCondition 인지 periodCondition 인지 상관하지 않는다.

Movie 입장에서 sequenceCondition 과 periodCondition 이 동일한 책임을 수행한다는 것은 **동일한 역할을 수행**한다는 것을 의미한다.(같은 메시지를 처리함 → 같은 책임을 가짐 → 동일한 역할을 수행함)

 ![img_8.png](img_8.png)

다음과 같이 인터페이스로 분리해 보자.

```java
public interface DiscountCondition {
    public boolean isSatisfiedBy(Screening screening);
}
```

DiscountCondition의 경우에서 알 수 있듯이 객체의 타입에 따라 변하는 행동이 있다면 타입을 분리하고 변화하는 행동을 각 타입의 책임으로 할당해야 한다.

GRASP 에서는 이를 **POLYMORPHISM(다형성) 패턴**이라 부른다.

다형성 패턴은 객체의 타입을 검사해서 타입에 따라 여러 대안들을 수행하는 조건적인 논리를 사용하지 말라고 경고한다.

대신 다형성을 통해서 새로운 변화에 대응하기 쉽게 확장해야 한다.

### **변경으로부터 보호**

DiscountCondition 이라는 추상화가 구체적인 타입들을 캡슐화 한다.

Movie의 관점에서 DiscountCondition 이라는 추상화가 구체적인 타입을 캡슐화 한다는 것은, 새로운 DiscountCondition 타입을 추가해도 Movie는 어떠한 수정도 필요가 없다.

이처럼 변경을 캡슐화 하도록 책임을 할당하는 것을 GRASP 에서는 **PROTECTED VARIATIONS(변경 보호) 패턴**이라고 부른다.

변화가 예상되는 지점을 식별하고, 그 주위에 안정된 인터페이스를 형성하도록 책임을 할당하다.

변경 보호 패턴은 책임 할당의 관점에서 캡슐화를 설명한 것이다.

"**설계에서 변하는 것이 무엇인지 고려하고 변하는 개념을 캡슐화 하라**"[GOF94] 라는 객체지향의 격언은 본질을 잘 설명한다.

### **Movie 클래스 개선하기**

Movie 역시 DiscountCondition과 동일한 문제가 있다.

해결 방법 또한 DiscountCondition과 동일하다.

```java
public class Movie {
	// 생략

    private MovieType movieType;
    private Money discountAmount;
    private double discountPercent;

    public Money calculateMovieFee(Screening screening) {
        if(isDiscountable(screening)){
            return fee.minus(calculateDiscountAmount());
        }
        return fee;
    }
    
    private Money calculateDiscountAmount() {
        switch (movieType){
            case AMOUNT_DISCOUNT:
                return calculateAmountDiscountAmount();
            case PERCENT_DISCOUNT:
                return calculatePercentDiscountAmount();
            case NONE_DISCOUNT:
                return calculateNoneDiscountAmount();
        }

        throw new IllegalStateException();
    }
		//     ...
}
```

calculateDiscountAmout switch ~ case 문으로 조건을 분리하고 있다.

이는 여러 할인 정책을 하나의 클래스 안에서 구현하고 있기 때문이다.

따라서 POLYMOPHISM 패턴으로 타입을 분리한다. 각각 AmountDIscountMovie, PercentDiscountMovie, NoneDiscountMovie가 된다.

PROTECTED VARIATIONS 패턴을 이용해 Movie내의 속성들을 분리한다.

따라서 최종적으로 구조는 다음과 같아진다.

 ![img_9.png](img_9.png)

### **변경과 유연성**

개발자가 변경에 대비하는 방법은 두 가지 방법이 있다.

- 코드를 이해하고 수정하기 쉽게 단순하게 설계
- 코드를 수정하지 않고 변경을 수용할 수 있도록 코드를 더 유연하게 만드는 것

대부분 전자가 더 좋은 방법이지만, 유사한 변경이 반복적으로 발생한다면 복잡성이 증가하더라도 유연성있는 코드를 만드는것이 좋다.

할인 정책을 구현하기 위해서 상속을 사용하고 있기 때문에 실행 중에 영화의 할인 정책을 변경하기 위해서는 새로운 인스턴스를 생성한 후 필요한 정보를 복사해야 한다.

또한 변경 전후의 인스턴스가 개념적으로는 동일한 객체지만, 물리적으로는 서로 다른 객체이기 때문에 식별자의 관점에서 혼란스러울 수 있다.

- 새로운 할인 정책이 추가되고, 인스턴스를 생성, 복사, 관리하는 것은 번거롭고 오류를 발생시키기 쉽다.

해결 방법은 상속 대신 **합성**을 사용하면 된다.

 ![img_10.png](img_10.png)

- 할인 정책을 독립적인 DiscountPolicy로 분리 후 movie에 합성시키면 기존보다 유연한 설계가 된다.

## **4. 책임 주도 설계의 대안**

대략적인 책임 주도 설계 흐름을 알아봤지만, 책임 주도 설계를 할때 책임을 할당할 객체를 선택하는것은 사실 매우 어려운 일이다.

책임과 객체를 정하지 못하고 방황할때 해결하는 방법은 일단 실행되는 코드를 작성 후 코드 상에 명확하게 드러나는 책임들을 올바른 위치로 이동시키는 것이다.

- 데이터 중심의 설계에서 시작해서 리팩토링 해가는 과정을 통해 이 방식의 장점을 알아보자.

### **메서드 응집도**

ReservationAgency의 reserve 메서드를 살펴보자

reserve 와 같은 긴 메서드는 다양한 측면에서 코드의 유지보수에 부정적인 영향을 준다.

- 어떤 일을 수행하는지 한눈에 파악하기가 어렵다.
- 하나의 메서드 안에서 너무 많은 일을 수행하기 때문에 변경이 필요할때 수정할 부분을 찾기가 어렵다.
- 메서드 내부의 일부 로직만 수정해도 메서드의 나머지에서 버그가 발생할 확률이 높다.
- 로직의 일부 재사용 불가
- 코드를 재사용 하는 유일한 방법은 복붙하는것 뿐이므로 코드 중복이 많아진다.

→ 한마디로 긴 메서드는 응집도가 낮아서 이해하거나 재사용하기 어렵고 변경하기 어렵다 (몬스터 메서드)

응집도가 낮은 메서드는 흐름을 이해하기 위해서는 주석이 필요한 경우가 대부분이다.

- 만약, 메서드의 응집도가 낮다면, 주석을 추가하는 대신 메서드를 더 작은 메서드 들로 분리하자.

짧고 이해하기 쉬운 이름으로 된 메서드

- 메서드가 잘게 나눠져 있을때, 다른 메서드에서 사용될 확률이 높다진다.
- 고수준의 메서드를 볼때 일련의 주석을 읽는 듯한 느낌이 들게할 수 있다.

책임을 분배할 때 가장 먼저 할일은 메서드를 응집도 있는 수준으로 분배하는 것이다.

분해 후 응집도 높은 reserve 메서드

```java

public class ReservationAgency {
    public Reservation reserve(Screening screening, Customer customer,
                               int audienceCount) {
        boolean discountable = checkDiscountable(screening);
        Money fee = calculateFee(screening, discountable, audienceCount);
        return createReservation(screening, customer, audienceCount, fee);
    }

    private boolean checkDiscountable(Screening screening) {
        return screening.getMovie().getDiscountConditions().stream()
                .anyMatch(condition -> condition.isDiscountable(condition, screening));
    }

    private Money calculateFee(Screening screening, boolean discountable,
                               int audienceCount) {
        if (discountable) {
            return screening.getMovie().getFee()
                    .minus(calculateDiscountedFee(screening.getMovie()))
                    .times(audienceCount);
        }

        return  screening.getMovie().getFee();
    }

    private Money calculateDiscountedFee(Movie movie) {
        switch(movie.getMovieType()) {
            case AMOUNT_DISCOUNT:
                return calculateAmountDiscountedFee(movie);
            case PERCENT_DISCOUNT:
                return calculatePercentDiscountedFee(movie);
            case NONE_DISCOUNT:
                return calculateNoneDiscountedFee(movie);
        }

        throw new IllegalArgumentException();
    }

    private Money calculateAmountDiscountedFee(Movie movie) {
        return movie.getDiscountAmount();
    }

    private Money calculatePercentDiscountedFee(Movie movie) {
        return movie.getFee().times(movie.getDiscountPercent());
    }

    private Money calculateNoneDiscountedFee(Movie movie) {
        return movie.getFee();
    }

    private Reservation createReservation(Screening screening,
                                          Customer customer, int audienceCount, Money fee) {
        return new Reservation(customer, screening, fee, audienceCount);
    }
}
```

수정 후에는 각 메서드가 어떤 일을 하는지 한눈에 알아볼 수 있다.

심지어 메서드의 구현이 주석을 모아 높은 것 처럼 보인다.

또한 큰 메서드는 작은 메서드로 나누면 한번에 기억해야 하는 정보를 줄일수가 있다.

수정후의 코드는 변경, 수정하기도 쉽다.

그래서 작고, 명확하며, 한가지 일에 집중하는 응집도 높은 메서드는 변경 가능한 설계를 이끌어 내는 기반이 된다.

메서드 들의 응집도 자체는 높아졌지만**,** 아직 메서드들을 담고있는 ReservationAgency의 응집도는 여전히 낮다.

ReservationAgency 의 응집도를 높이기 위해서는 변경의 이유가 다른 메서드들을 적절한 위치로 분배해야 한다.

적절한 위치란 메서드가 사용하고 있는 데이터를 정의하고 있는 클래스를 의미한다.(행동과 상태가 같이 있는 자율적인 객체)

### **객체를 자율적으로 만들자**

자신이 소유하고 있는 데이터를 스스로 처리하도록 만드는것이 자율적인 객체를 만드는 지름길이다.

어떤 데이터를 사용하는지를 가장 쉽게 알 수 있는 방법은 메서드 안에서 어떤 클래스의 접근자 메서드를 사용하는지 파악하는것이다.

예를 들어 ReservationAgency의 isDiscountable 메서드를 살펴보자.

```tsx
public class ReservationAgency {
    private boolean isDiscountable(DiscountCondition condition, Screening screening) {
    	if(condition.getType() == DiscountConditionType.PERIOD) {
        	return isSatisfiedByPeriod(condition, screening);
        }
    }
}
```

isDiscountable 메서드는 DiscountCondition의 getter를 호출하여 할인 조건 타입을 알아낸다.

따라서 이 메서드는 DiscountCondition에 속한 데이터를 주로 이용한다는 것을 알 수 있다.

따로 DiscountCondition으로 이동시키고, 원래의 ReservationAgency에서는 이부분을 삭제해 주자.

기존의 ReservationAgency에서의 isDiscountable 메서드는 인자로 DiscountCondition을 받아야 했지만,

변경된 이후에는 DiscountCondition의 일부가 되었기 때문에 인자로 전달받을 필요가 없어진다.

```java
private boolean checkDiscountable(Screening screening) {
    return screening.getMovie().getDiscountConditions().stream()
            .anyMatch(condition -> condition.isDiscountable(screening));
}

```

- 이후 다형성 패턴과 변경 보호 패턴을 차례대로 적용하면 최종 설계와 유사한 모습의 코드를 갖게 된다.

여기서 중요한것은 책임 주도 설계 방법에 익숙하지 않다면, 일단 데이터 중심으로 구현 후 이를 리팩토링하더라도 유사한 결과를 얻을 수 있다는 것이다.

- 처음부터 책임 주도 설계를 따르는것보다 이 방법이 더 훌륭할 수 있다.
- 캡슐화, 결합도, 응집도를 이해하고 훌륭한 객체지향 원칙을 적용하기 위해 노력하면 책임주도설계를 따르지 않더라도 유연하고 깔끔한 코드를 얻을 수 있다.