# 1장 - 객체, 설계

## 티켓 판매 애플리케이션 구현하기

초대장이라는 개념을 구현한 Invitation은 공연을 관람할 수 있는 초대일자(when)을 인스턴스 변수로 포함하는 간단한 클래스이다.

```java
public class Invitation {
	private LocalDateTime when;
}
```

공연을 관람하기 원하는 모든 사람들은 티켓을 소지하고 있어야만 한다. Ticket 클래스를 추가하자.

```java
public class Ticket {
	private Long fee;

	// getter
}
```

관람객은 소지품을 가지고 있다. 초대장, 현금, 티켓 소지품을 보관할 Bag 클래스를 추가하자.

```java
public class Bag {
	private Long amount;
	private Invitation invitation;
	private Ticket ticket;

  public boolean hasInvitation() {
      return invitation != null;
  }

  public boolean hasTicket() {
      return ticket != null;
  }

  public void setTicket(Ticket ticket) {
      this.ticket = ticket;
  }

  public void plusAmount(Long amount) {
      this.amount += amount;
  }

  public void minusAmount(Long amount) {
      this.amount -= amount;
  }
}
```

만약 이벤트에 당첨되지 않았다면 초대장이 없을 것이다. 제약사항을 강제할 수 있도록. 생성자를 추가하자.

```java
public class Bag {
	public Bag(long amount) { // 제약사항
		this(null,amount);
	}

public Bag(Long amount, Invitation invitation) {
	  this.amount = amount;
	  this.invitation = invitation;
	}
}
```

관람객은 소지품을 보관하기 위해 가방을 소지할 수 있다.

```java
public class Audience {
	private Bag bag;

	public Audience(Bag bag) {
        this.bag = bag;
  }

	public Bag getBag() {
        return bag;
  }
}
```

관람객이 소극장에 입장하기 위해서는 매표소에서 초대장을 티켓으로 교환하거나 구매해야 한다.

매표소를 구현하기 위해 TicketOffice 클래스를 추가하자.

TicketOffice는 판매하거나 교환해줄 목록과 판매 금액을 인스턴스 변수로 포함한다.

```java
public class TicketOffice {
  private Long amount;
  private List<Ticket> tickets = new ArrayList<>();

  public TicketOffice(Long amount, Ticket... tickets) {
      this.amount = amount;
      this.tickets.addAll(Arrays.asList(tickets));
  }

  public Ticket getTicket() {
      return tickets.remove(0);
  }

  public void plusAmount(Long amount) {
      this.amount += amount;
  }

  public void minusAmount(Long amount) {
      this.amount -= amount;
  }
}
```

판매원은 매표소에서 초대장을 티켓으로 교환해주거나 티켓을 판매하는 역할을 수행한다. 판매원을 구현한 TicketSeller 클래스는 자신이 일하는 매표소를 알고 있어야 한다.

```java
public class TicketSeller {
  private TicketOffice ticketOffice;

  public TicketSeller(TicketOffice ticketOffice) {
      this.ticketOffice = ticketOffice;
  }

  public TicketOffice getTicketOffice() {
      return ticketOffice;
  }
}
```

소극장을 구현하는 클래스는 Theater다. 관람객을 맞이할 수 있도록 enter 메서드를 구현하자.

```java
public class Theater {
  private TicketSeller ticketSeller;

  public Theater(TicketSeller ticketSeller) {
      this.ticketSeller = ticketSeller;
  }

  public void enter(Audience audience) {
      if (audience.getBag().hasInvitation()) { // 초대장이 들어있는지 확인
          Ticket ticket = ticketSeller.getTicketOffice().getTicket(); // 티켓을 꺼낸다
          audience.getBag().setTicket(ticket); // 티켓을 관람객의 가방에 넣어준다
      } else {
          Ticket ticket = ticketSeller.getTicketOffice().getTicket();
          audience.getBag().minusAmount(ticket.getFee()); // 초대장이 없으면 판매
          ticketSeller.getTicketOffice().plusAmount(ticket.getFee()); // 매표소 금액 증가
          audience.getBag().setTicket(ticket); // 가방에 넣어준다
      }
  }
}
```

## 무엇이 문제일까?

마틴에 의하면 소프트웨어 모듈은 제대로 실행돼야 하고, 변경이 용이해야 하며, 이해하기 쉬워야 한다.

책에서는 변경 용이성과 읽는 사람과의 의사소통이라는 목적은 만족시키지 못한다고 한다.

## 예상을 빗나가는 코드

### 수동적인 존재

Theater 클래스와 enter 메서드가 수행하는 일을 말로 풀어보면 관람객과 판매원이 소극장의 통제를 받는 수동적인 존재다. 현실과 비유해보면 현재의 코드는 우리의 상식과 다르게 동작한다.

### 코드를 이해하기 어렵다

이 코드를 이해하기 위해서는 객체의 세부적인 내용들을 한번에 기억하고 있어야 한다.

### 변경에 취약하다

만약 관람객이 현금이 아니라 신용카드로 결제한다면 Audience의 Bag에 직접 접근하는 Theater의 enter 메서드까지 수정해야 한다. 지나치게 세부적인 사실에 의존하고 있다. 이처럼 다른 클래스의 내부에 대해 더 많이 알면 알수록 변경이 어려워진다.

이것을 객체 사이의 의존성이라고 한다. 의존성은 변경에 대한 영향을 암시한다. 의존성이라는 말 속에는 어떤 객체가 변경될 때 그 객체에게 의존하는 다른 객체도 함께 변경될 수 있다는 사실이 내포돼 있다.

객체 사이의 의존성이 과한 경우를 가리켜 결합도가 높다고 말한다. 결합도는 의존성과 관련돼 있기 때문에 결합도 역시 변경과 관련이 있다.

## 설계 개선하기

관람객과 판매원을 자율적인 존재로 개선해보자.

```java
public class Theater {
  private TicketSeller ticketSeller;

  public Theater(TicketSeller ticketSeller) {
      this.ticketSeller = ticketSeller;
  }

  public void enter(Audience audience) {
      ticketSeller.sellTo(audience);
  }
}
```

```java
public class TicketSeller {
  private TicketOffice ticketOffice;

  public TicketSeller(TicketOffice ticketOffice) {
      this.ticketOffice = ticketOffice;
  }

  public void sellTo(Audience audience) {
      if (audience.getBag().hasInvitation()) {
          Ticket ticket = ticketOffice.getTicket();
          audience.getBag().setTicket(ticket);
      } else {
          Ticket ticket = ticketOffice.getTicket();
          audience.getBag().minusAmount(ticket.getFee());
          ticketOffice.plusAmount(ticket.getFee());
          audience.getBag().setTicket(ticket);
      }
  }
}
```

TicketOffice의 가시성이 private로 변경되었고, getter 가 존재하지 않기 때문에 외부에서는 ticketOffice에 직접 접근할 수 없다. 오직 TicketSeller 안에만 존재하기 되고, 티켓을 꺼내거나 판매 요금을 적립하는 일을 스스로 수행할 수 밖에 없다.

이처럼 개념적이나 물리적으로 객체 내부의 세부적인 사항을 감추는 것을 캡슐화라고 부른다.

캡슐화의 목적은 변경하기 쉬운 객체를 만드는 것이다. 캡슐화를 통해 객체 내부로의 접근을 제한하면 객체와 객체 사이의 결합도를 낮출 수 있기 때문에 설계를 좀 더 쉽게 변경할 수 있게 된다.

- 오히려 외부 접근이 자유로운게 변경하기 쉬운 객체가 아닌가?

Theater는 ticketOffice가 TicketSeller 내부에 존재한다는 사실을 알지 못한다. 단지 sellTo 메시지를 이해하고 응답할 수 있다는 사실만 알고 있을 뿐이다.

```java
public class Theater {
  private TicketSeller ticketSeller;

  public Theater(TicketSeller ticketSeller) {
      this.ticketSeller = ticketSeller;
  }

  public void enter(Audience audience) {
      ticketSeller.sellTo(audience);
  }
}
```

Theater는 오작 TicketSeller 인터페이스에만 의존한다. 내부에 인스턴스를 포함하고 있다는 사실은 구현의 영역에 속한다. 객체를 인터페이스와 구현으로 나누고 인터페이스만을 공개하는 것은 객체 사이의 결합도를 낮추고 변경하기 쉬운 코드를 작성하기 위해 따라야 하는 가장 기본적인 설계 원칙이다.

- 나는 인터페이스 설계가 부족하다

동일한 방법으로 Audience의 캡슐화를 개선할 수 있다.

```java
public class Audience {
  private Bag bag;

  public Audience(Bag bag) {
      this.bag = bag;
  }

  public Long buy(Ticket ticket) {
      if (bag.hasInvitation()) {
          bag.setTicket(ticket);
          return 0L;
      } else {
          bag.setTicket(ticket);
          bag.minusAmount(ticket.getFee());
          return ticket.getFee();
      }
  }
}
```

변경된 코드에서는 가방 안에 초대장이 들어있는지를 스스로 확인한다. 외부의 제3자가 자신의 가방을 열어보도록 허용하지 않는다. 외부에서는 더 이상 Audience가 Bag을 소유하고 있다는 사실을 알 필요가 없다.

이제 TicketSeller가 Audience의 인터페이스에만 의존하도록 수정하자.

```java
public class TicketSeller {
  private TicketOffice ticketOffice;

  public TicketSeller(TicketOffice ticketOffice) {
      this.ticketOffice = ticketOffice;
  }

  public void sellTo(Audience audience) {
      ticketOffice.plusAmount(audience.buy(ticketOffice.getTicket()));
  }
}
```

### 무엇이 개선됐는가

수정된 코드는 자신이 가지고 있는 소지품을 스스로 관리한다. 코드를 읽는 사람과의 의사소통이라는 관점에서 이 코드는 확실히 개선된 것으로 보인다. 더 중요한 점은 내부 구현을 변경하더라도 다른 코드를 함께 변경할 필요가 없어졌다는 것이다. 가방이 아니라 지갑으로 바뀐 경우, 매표소가 아니라 은행에 돈을 보관하더라도 내부만 변경하면 된다.

### 어떻게 한 것인가

자기 자신의 문제를 스스로 해결하도록 코드를 변경한 것이다. 객체의 자율성을 높이는 방향으로 설계를 개선했다.

그 결과 이해하기 쉽고 유연한 설계를 얻을 수 있었다.

### 캡슐화와 응집도

핵심은 객체 내부의 상태를 캡슐화하고 객체 간에 오직 메시지를 통해서만 상호작용하도록 만드는 것이다.

단지 메시지에 응답할 수 있고 자신이 원하는 결과를 반환할 것이라는 사실만 알고 있을 뿐이다.

밀접하게 연관된 작업만 수행하고 연관성 없는 작업은 다른 객체에게 위임하는 객체를 가리켜 응집도가 높다고 말한다. 자신의 데이터를 스스로 처리하는 자율적인 객체를 만들면 결합도록 낮출 수 있을뿐더러 응집도를 높일 수 있다.

외부의 간섭을 최대한 배제하고 메시지를 통해서만 협력하는 자율적인 객체들의 공동체를 만드는 것이 훌륭한 객체지향 설계를 얻을 수 있는 지름길인 것이다.

### 절차지향과 객체지향

프로세스와 데이터를 별도의 모듈에 위치시키는 방식을 절차적 프로그래밍이라고 한다.

절차적 프로그래밍의 세계에서는 자신의 일을 스스로 처리하지 않고 수동적인 존재다. 데이터의 변경을 고립시키지 못해 내부 구현이 변경되면 다른 메서드를 함께 변경해야 한다. 변경은 버그를 부르고 코드를 변경하기 어렵게 만든다.

변경하기 쉬운 설계는 한 번에 하나의 클래스만 변경할 수 있는 설계다. 데이터와 프로세스가 동일한 모듈에 위치하도록 프로그래밍 하는 방식을 객체지향 프로그래밍이라고 한다. 객체지향 설계의 핵심은 캡슐화를 이용해 의존성을 적절히 관리함으로써 객체 사이의 결합도를 낮추는 것이다.

### 책임의 이동

두 방식 사이에 근본적인 차이를 만드는 것은 책임의 이동이다. 객체지향 세계의 용어를 사용해서 표현하면 책임이 하나의 클래스에 집중돼 있는 것이다. 객체지향 설계에서는 각 객체가 자신이 맡은 일을 스스로 처리했다. 이것이 바로 책임의 이동이 의미하는 것이다. 따라서 각 객체는 자신을 스스로 책임진다.

### 그래, 거짓말이다!

비록 현실에서는 수동적인 존재라고 하더라도 일단 객체지향의 세계에 들어오면 모든 것이 능동적이고 자율적인 존재로 바뀐다. 이를 의인화라고 부른다.

### 객체지향 설계

좋은 설계란 오늘 요구하는 기능을 온전히 수행하면서 내일의 변경을 매끄럽게 수용할 수 있는 설계다. 요구사항은 항상 변경되기 때문이다. 그리고 변경을 수용할 수 있는 설계가 중요한 또 다른 이유는 코드를 변경할 때 버그가 추가될 가능성이 높기 때문이다.

객체지향이 과거의 다른 방법보다 안정감을 준다는 사실에 공감해보자.

변경 가능한 코드란 이해하기 쉬운 코드다. 만약 코드를 변경해야 하는데 코드를 이해할 수 없다면 그 코드가 변경에 유연하다고 하더라도 코드를 수정하겠다는 마음이 들지 않는다.

훌륭한 객체지향 설계란 협력하는 객체 사이의 의존성을 적절하게 관리하는 설계다. 데이터와 프로세스를 하나의 덩어리로 모으는 것은 훌륭한 설계로 가는 첫걸음일 뿐이다.