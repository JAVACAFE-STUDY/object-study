객체지향 설계의 핵심은 협력에 필요한 의존성은 유지하면서 변경을 방해하는 의존성은 제거하는데 있습니다.

충분히 협력적이면서도 유연한 객체를 만들기 위해 의존성을 관리하는 방법을 알아보도록 하겠습니다.

### 의존성 이해하기

의존성은 어떤 객체가 협력하기 위해 다른 객체를 필요로 할때 존재하게 됩니다.

이 때 의존성은 실행 시점과 구현 시점에 다른 의미를 가집니다.

- 실행 시점 : 의존하는 객체가 정상적으로 동작하기 위해서는 실행시에 의존 대상 객체가 반드시 존재해야 한다.
- 구현 시점 : 의존 대상 객체가 변경될 경우 의존하는 객체도 함께 변경된다.

```java
public class PeriodCondition implements DiscountCondition {
	private DayofWeek dayOfWeek;
	private LocalTime startTime;
	private LocalTime endTime;
	
	public boolean isSatisfiedBy(Screening screening) {
		return screening.getStartTime().getDayOfWeek().equals(dayOfWeek) 
		&& startTime.compareTo(screening.getStartTime().toLocalTime()) <= 0 
		&& endTime.compareTo(screening.getStartTime().toLocalTime()) >= 0;
	}
	...
}
```

이 코드를 보면 PeriodCondition가 정상적으로 동작하기 위해서는 Screening 인스턴스가 존재해야한다.

- 어떤 객체가 예정된 작업을 정상적으로 수행하기 위해 다른 객체를 필요로 하는 경우 두 객체 사이에 의존성이 존재한다고 말한다.

![img.png](img.png)

### 의존성 전이

PeriodCondition은 screening에 의존하며 screening이 의존하는 대상에 대해서도 자동적으로 의존하게 됩니다.

![img_3.png](img_3.png)

의존성은 함께 변경 될 수 있는 가능성을 의미합니다.

모든 경우에 의존성이 전이되는 것은 아니며, 의존성이 실제로 전이 될지 여부는 변경의 방향과 캡슐화의 정도에 따라 달라집니다.

의존성은 전이될 수 있기 때문에 의존성의 종류를 직접 의존성과 간접 의존성으로 나누기도 합니다.

- **직접 의존성**: 말 그대로 한 요소가 다른 요소에 직접 의존하는 경우
- **간접 의존성**: 직접적인 관계는 존재하지 않지만 의존성 전이에 의해 영향이 전파되는 경우

예시에서는 클래스를 예로 들어 설명했지만, 변경과 관련 있는 어떤 것에도 의존성 이라는 개념을 적용할 수 있습니다.

### 런타임 의존성과 컴파일타임 의존성

**런타임 의존성**

어플리케이션이 실행되는 시점에 갖는 의존성을 의미합니다.

**컴파일타임 의존성**

작성된 코드를 컴파일 하는 시점을 의미 (또는 문맥에 따라서는 코드 그 차제를 가리키기도 한다.)

![img_1.png](img_1.png)

위 예시에서는 Movie와 DiscountPolicy는 컴파일타임에 의존성을 갖는다.

하지만 런타임 의존성을 확인해보면 조건에 따라 AmountDiscountPolicy, PercentDisCountPolicy 인스턴스와 각각 협력해야 합니다.

Movie 의 인스턴스가 두 클래스(혹은 두개 이상)의 인스턴스와 함께 협력할 수 있게 하는 방법은 두 클래스 중 어떤것도 알지 못하게 하고, 이를 포괄하는 DiscountPolicy 추상 클래스에 의존하도록 하게 만들고 런타임 의존성으로 실제 구체클래스를 의존하게 대체해야 합니다.

이러한 특징이 하나의 객체를 다른 객체와 런타임 시점에 의존성을 갖게하며 **유연한 설계 그리고 다른 클래스들과**

**협력할 가능성을 열어주기 때문에 변경에 유용**한 것이다.

### 컨텍스트 독립성

유연하고 확장 가능한 설계를 만들기 위해서는 컴파일 타임 의존성과 런타임 의존성이 달라야 한다.

객체는 자신과 결합할 구체적인 클래스에 대해 아는 것은 좋지 않다.

구체적인 클래스를 알면 알수록 그 클래스가 사용되는 특정한 문맥에 강하게 결합되기 때문이다.

- ex. PercentDiscountPolicy를 컴파일 타임 의존성을 명시하면 Movie가 비율 할인 정책이 적용된 영화의 요금을 계산하는 문맥에서만 사용될것이다.

**컨텍스트 독립성이란 클래스가 사용될 특정한 문맥에 대해 최소한의 가정만으로 이루어져 있어 다른 문맥에서 재사용하기가 수월해지는 것을 의미한다.**

- 설계가 유연해지기 위해서는 가능한 자신이 실행될 컨텍스트에 대한 구체적인 정보를 적게 알아야한다.
- 컨텍스트에 대한 정보가 적으면 적을수록 더 다양한 컨텍스트에서 재사용 될수 있기 때문이다.

### 의존성 해결하기

컴파일 타임 의존성은 구체적인 런타임 의존성으로 대체되어야 합니다.

이처럼 컴파일 타임 의존성을 실행 컨텍스트에 맞는 적절한 런타임 의존성으로 교체하는 것을 **의존성 해결** 이라고 부릅니다.

**의존성 해결을 위한 3가지 방법**

- 객체를 생성하는 시점에 **생성자**를 통해 의존성 해결
- 객체 생성 후 **setter 메서드**를 통해 의존성 해결
- **메서드 실행 시 인자**를 이용해 의존성 해결
    - 메서드가 실행될 동안만 일시적으로 의존관계가 존재해도 되거나, 실행될때마다 의존 대상이 달라져야 하는 경우 유용하다.

## 2. 유연한 설계

의존성을 관리하는데 유용한 원칙과 기법을 알아보자.

### 의존성과 결합도

객체지향 패러다임에서 근간은 협력이고, 객체들은 협력을 통해 애플리케이션을 구성한다.

객체들이 협력하기 위해서는 서로의 존재와 수행 가능한 책임을 알아야한다.

따라서 모든 의존성은 나쁜것이 아니며, 협력을 가능하게 만드는 매개체라는 관점에서는 바람직한 것이다.

문제는 의존성의 존재가 아니라 의존성의 정도입니다.

```java
public class Movie {
    private PercentDiscountPolicy discountPolicy;

    public Movie(String title, Duration runningTime, Money fee, PercentDiscountPolicy discountPolicy) {
        this.title = title;
        this.runningTime = runningTime;
        this.fee = fee;
        this.discountPolicy = discountPolicy;
    }

		// ...
    public Money calculateMovieFee(Screening screening) {
        return fee.minus(discountPolicy.calculateDiscountAmount(screening));
    }
}
```

이 코드는 PercentDiscountPolicy에 의존하게 되면서 다른 종류의 할인 정책이 필요한 문맥에서 Movie의 재사용 가능성을 없애버렸다.

→ 해결방법은 의존성을 바람직하게 만드는것이다.

여기서는 굳이 PercentDiscountPolicy 가 아닌 자신이 전송하는 calculateDiscountAmount 메시지를 이해할 수 있고 할인된 요금을 계산할 수 있으면 된다.

그래서 DiscountPolicy 타입을 정의하고 문제를 해결할 수 있다.

의존성은 협력을 위해 반드시 필요하며, 바람직한 의존성을 가져야한다.

**그럼 바람직한 의존성이란?**

이는 재사용성과 관련이 있습니다.

- 어떤 의존성이 다양한 환경에서 클래스를 재사용할 수 없도록 제한한다면 그 의존성은 바람직하지 못한 것이다.  즉 컨텍스트에 독립적인 의존성은 바람직한 의존성이고 특정한 컨텍스트에 강하게 결합된 의존성은 바람직하지 않은 의존성이다.
- 특정 컨텍스트에 강하게 의존하는 클래스를 다른 컨텍스트에서 재사용하려면 구현을 변경해야 한다.
    - 이 경우 바람직하지 못한 의존성을 또 다른 바람직하지 못한 의존성으로 대체하는 것이다.

바람직한 의존성을 **느슨한 결합도 or 약한 결합도**라고 하고, 바람직하지 못한경우를 **단단한 결합도 or 강한 결합도**라고 한다.

### 지식이 결합을 낳는다

결합도의 정도는 한 요소가 자신이 의존하고 있는 다른 요소에 대해 알고 있는 정보의 양으로 결정된다.

- 하나의 요소가 다른 정보에 대해 많이 알고 있을 수록 두 요소는 강하게 결합된다. **더 많이 알수록 더 많이 결합된다**

결합도를 느슨하게 유지하려면 협력을 위해 필요한 정보 이외에는 최대한 숨기는 것이 바람직하며, **추상화에 의존해야 합니다.**

### 추상화에 의존하라

추상화란 세부사항을 이해하기 위한 절차나 물체를 의도적으로 생략하거나 감추면서 복잡도를 극복하는 방법입니다.

추상화를 이용하면 현재 다루고 있는 문제를 해결하는 데 불필요한 정보를 감출 수 있다.

따라서 대상에 대해 알아야 하는 지식의 양을 줄일 수 있기 때문에 결합도를 느슨하게 유지할 수 있다.

- Movie에서 DiscountPolicy을 의존할때가 Percent 또는 Amount Policy를 의존할때보다 결합도가 느슨한 이유다.

목록에서 아래쪽으로 갈수록 클라이언트가 알아야하는 지식의 양이 적어지기 때문에 결합도가 느슨해진다.

- 구체 클래스 의존성 (concrete class dependency)
- 추상 클래스 의존성 (abstract class dependency)
    - 구체 클래스 보다는 클라이언트가 알아야 할 지식의 양이 적지만, 클래스 상속 계층이 무엇인지 알아야한다.
- 인터페이스 의존성 (interface dependency)

### 명시적인 의존성

느슨한 결합도를 위해서는 인스턴스 변수의 타입을 추상 클래스나 인터페이스로 선언하고, 클래스 안에서 구체 클래스에 대한 모든 의존성을 제거해야 한다.

인스턴스 변수의 타입은 추상 클래스나 인터페이스로 정의하고,

setter 메서드와 메서드 인자로 의존성을 해결할 때는 추상 클래스를 상속받거나 구체 클래스를 전달하자.

**명시적인 의존성**

- 생성자의 인자로 선언하거나 setter 메시더 또는 메서드 인자를 사용하는 방법은 의존성이 퍼블릭 인터페이스에 노출된다.

**숨겨진 의존성**

- 객체 내부에서 의존하는 객체의 인스턴스를 직접 생성하는 방식은 의존성을 감춘다.

의존성이 명시적이지 않으면 내부 구현을 직접 살펴볼 수밖에 없으며, 클래스를 다른 컨텍스트에서 재사용하기 위해 내부 구현을 직접 변경해야 한다.

의존성은 명시적으로 표현돼야 한다. 퍼블릭 인터페이스를 통해 컴파일타임 의존성을 적절한 런타임 의존성으로 교체할 수 있다.

### new는 해롭다

new 를 사용하게 되면 구체 클래스의 이름을 직접 기술하고, 어떤 인자를 이용해 클래스의 생성자를 호출해야 하는지도 알아야 하기 때문에 지식의 양이 늘어나고 결합도가 높아진다.

예시)

```java
public class Movie {
		private DiscountPolicy discountPolicy;
		
    public Movie(String title, Duration runningTime, Money fee) {
    	this.discountPolicy = new AmountDiscountPolicy(Money.wons(800),
        					new SequenceCondition(1),
                            new SequenceCondition(10),
                            new PeriodCondition(DayOfWeek.MONDAY,
                            	LocalTime.of(10, 0), LocalTime.of(11, 59)),
                            new PeriodCondition(DayOfWeek.THURSDAY,
                            	LocalTime.of(10, 0), LocalTime.of(20,59))));
    }
}
```

Movie는 AmountDiscountPolicy 가 어떤 생성자의 인자 목록을 가지고 있는지 알아야되고, SequenceCondition, PeriodCondition의 변경에도 영향을 받을 수 있다.

**new로 인한 결합도 증가 해결 방법**

- 인스턴스를 생성하는 로직과 생성된 인스턴스를 사용하는 로직을 분리해서 해결할 수 있다.

위 코드에서 Movie는 인스턴스를 생성해서는 안 된다.

단지 해당하는 인스턴스를 전달받아(생성자, setter, 인자를 통해) 사용하기만 해야 한다.

Movie에서 사용할 DiscountPolicy를 생성하는 책임을 Movie의 클라이언트로 옮기고, Movie에서는 생성된 인스턴스를 사용하는 책임만 남은 코드이다.

```java
public class Movie {
	private DiscountPolicy discountPolicy;
    public Movie(String title, Duration runningTime, Money fee, DiscountPolicy discountPolicy) {
    	this.discountPolicy = discountPolicy;
    }
}
```

생성의 책임을 클라이언트로 옮김으로써 이제 Movie는 DiscountPolicy의 모든 자식 클래스와 협력할 수 있게 됐다. -> 설계가 유연해졌다.

사용과 생성의 책임을 분리하고 생성하는 책임을 클라이언트로 옮김, 의존성을 생성자에 명시적으로 드러냄, 구체 클래스가 아닌 추상 클래스에 의존하게 함 -> 설계가 유연해졌다.

### 가끔은 생성해도 무방하다

- 인스턴스를 직접 생성하는 방식이 유용한 경우도 있다. 주로 협력하는 기본 객체를 설정하고 싶은 경우이다.
    - 만야 AmountDiscountpolicy를 주로 사용하고, PercentDiscountPolicy는 가끔 사용하는경우 클라이언트 사이에 중복 코드가 많이 발생 할 수 있다.
- 기본값을 생성하는 메서드와 인스턴스를 인자로 받는 메서드를 함께 사용한다면 클래스의 사용성을 향상시키면서도 다양한 컨텍스트에서 유연하게 사용될 수 있는 여지를 제공할 수 있다.

```java
public class Movie {
	private DiscountPolicy discountPolicy;
    
    public Movie(String title, Duration runnintTime, Money fee) {
		this(title, runningTime, fee, new AmountDiscountPolicy(...));
	}
    
    public Movie(String title, Duration runningTime, Money fee, DiscountPolicy discountPolicy) {
    	this.discountPolicy = discountPolicy;
    }
}
```

이 경우에 결합도와 사용성의 트레이드오프이다.

구체 클래스에 의존하게 되더라도 클래스의 사용성이 더 중요하다면 결합도를 높이는 방향으로 작성할 수 있다.

그럼에도 가급적 구체 클래스에 대한 의존성을 제거할 수 있는 방법을 고민해야하고, 종종 모든 결합도가 모이는 클래스를 추가하면서 사용성과 유연성 모두 잡을 수 있는 경우도 존재한다.

### 표준 클래스에 대한 의존은 해롭지 않다.

의존성이 불편한 이유는 그것이 항상 변경에 대한 영향을 암시하기 때문이다.

따라서 변경될 확률이 거의 없는 클래스라면 의존성이 문제가 되지 않는다.

- 자바라면 JDK 에 포함된 표준클래스를 예시로 들 수 있다.

표준 클래스 대해서는 구체 클래스에 의존하거나 직접 인스턴스를 생성하더라도 문제가 없다.

- 하지만 추상적인 타입을 사용하는 것이 확장성 측면에서 유리하다.

### 컨텍스트 확장하기

Movie를 DiscountPolicy라는 추상화에 의존하게 했고, 생성자를 통해 DiscountPolicy에 대한 의존성을 명시적으로 드러냈으며, new와 같이 구체 클래스를 직접적으로 다뤄야 하는 책임을 Movie 외부로 옮겼다. 이는 유연한 설계를 가능하게 했습니다.

이로 인해 Movie를 수정하지 않고도 새로운 기능을 추가하는 것이 쉬워진다.

다른 컨텍스트에서 Movie를 확장해서 사용하는 예시

1. 할인혜택을 제공하지 않는 영화의 예매 요금을 계산하는 경우

```java
public class NoneDiscountPolicy extends DiscountPolicy {
	@Override
    protected Money getDiscountAmount(Screening screening) {
    	return Money.ZERO;
    }
}

Movie avatar = new Movie("아바타",
			Duration.ofMinutes(120),
            Money.wons(10000),
            new NoneDiscountPolicy());
```

- 할인 혜택을 제공하지 않는 영화의 예매 요금을 계산하기 위해 Movie는 변경될 필요가 없다.

1. 중복 적용이 가능한 할인 정책을 구현하는 것

```java
public class OverlappedDiscountPolicy extends DiscountPolicy {
	
    private List<DiscountPolicy> discountPolicies = new ArrayList<>();
    
    public OverlappedDiscountPolicy(DiscountPolicy... discountPolicies) {
    	this.discountPolicies = Arrays.asList(discountPolicies);
    }
    
	@Override
    protected Money getDiscountAmount(Screening screening) {
    	Money result = Money.ZERO;
        for (DiscountPolicy each : discountPolicies) {
        	result = result.plus(each.calculateDiscountAmount(screening));
        }
    }
}

Movie avatar = new Movie("아바타",
			Duration.ofMinutes(120),
            Money.wons(10000),
            new OverlappedDiscountPolicy(
            	new AmountDiscountPolicy(...),
                new PercentDiscountPolicy(...)));
```

- 중복 적용이 가능한 할인 정책을 구현하기 위해 OverlappedDiscountPolicy를 구현하고 클라이언트에서 사용하면 된다 → Movie 자체의 구현은 변경될 필요가 없다.

결합도를 낮춤으로써 얻게 되는 컨텍스트의 확장이라는 개념이 유연하고 재사용 가능한 설계를 만드는 핵심이다.

### 조합 가능한 행동

다양한 종류의 할인 정책이 필요한 컨텍스트에서 Movie를 재사용할 수 있었던 이유는 코드를 직접 수정하지 않고도 협력 대상인 DiscountPolicy 인스턴스를 교체할 수 있었기 때문이다.

어떤 객체와 협력하느냐에 따라 객체의 행동이 달라지는 것은 유연하고 재사용 가능한 설계가 가진 특징이다.

유연하고 재사용 가능한 설계는 응집도 높은 책임들을 가진 작은 객체들을 다양한 방식으로 연결함으로써 애플리케이션의 기능을 쉽게 확장할 수 있게한다.

유연하고 재사용 가능한 설계는 객체가 어떻게(how) 하는지를 장황하게 나열하지 않고도 객체들의 조합을 통해 무엇(what)을 하는지를 표현하는 클래스들로 구성된다.

훌륭한 객체지향 설계란 객체가 어떻게 하는지를 표현하는것이 아니라 객체들의 조합을 선언적으로 표현함으로써 객체들이 무엇을 하는지를 표현하는 설계다.

이런 설계를 하는데에 있어서 핵심은 의존성을 관리하는 것이다.