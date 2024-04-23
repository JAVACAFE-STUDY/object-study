# 해당 장의 목표
- 유연하고 재사용이 가능한 퍼블릭 인터페이스를 만드는데 도움이 되는 설계 원칙과 기법을 알아보자

# 클라이언트 - 서버 모델

두 객체 사이의 협력관계를 설명하기 위해 사용하는 전통적인 메타포를 의미한다.

⇒ 즉, 협력의 관점으로 객체는 2가지의 종류를 의미한다고 생각

클라이언트

- 협력 안에서 **메시지를 전송**하는 객체

서버

- 메시지를 **수신**하는 객체

## 메시지 구성요소

메시지의 수신자

Operation name [ 오퍼레이션 명 ]

Argument [ 인자 ]

ex ) shop.isShopNameUsed(shopName)

shop : 수신자

isShopNameUsed : 오퍼레이션 명

shopName : 인자

### 메서드

메시지를 수신했을 때 실제로 실행되는 함수 또는 프로시저를 의미한다.

객체는 메시지와 메서드라는 다른 개념을 런타임에 연결하기 때문에 컴파일 시점과 의미가 달라질 수 있다.

- 메시지 전송을 코드에 표기하는 시점엔 어떤 코드가 실행될 것인지 정확히 알 수 없다.
    - 이는 책임에 대해 메시지를 전송하기 때문
- 다만, 메시지 송신자는 누구에게 메시지를 보내야 하는지 몰라도 되기에 메시지 자체에 집중할 수 있다. 메시지 수신자 또한 누가 보냈는지 몰라도 된다.

⇒ 각 메시지를 보내는 출처에서 자유로워 질 수 있다

### 퍼블릭 인터페이스

객체가 외사소통을 위해 외부에 공개하는 메시지의 집합을 말한다.

**operation**

- 퍼블릭 인터페이스에 포함된 메시지
- 수행 가능한 어떤 행동에 대한 추상화

**메서드**

- 메시지를 수신했을 때 실제로 실행되는 코드를 말한다.

오퍼레이션이란 실행하기 위해 객체가 호출될 수 있는 변환이나 정의에 관한 명세를 말한다.

인터페이스의 각 요소는 오퍼레이션이다.

오퍼레이션이란 구현이 아닌 추상화이고 메서드는 오퍼레이션에 대한 구현을 말한다.

다형성을 잘 활용하기 위해 오퍼레이션에 대해 다양한 메서드를 구현해야 한다.

**시그니처**

- 오퍼레이션 + 메서드의 이름과 파라미터 목록을 합쳐 이야기한다.
    - ex ) shop.**isShopNameUsed(shopName)**

# 인터페이스와 설계 품질

디미터 법칙 ( Law of Demeter )

- 객체 내부 구조에 강하게 결합되지 않도록 협력 경로를 제한하라
    - 클래스가 특정한 조건을 만족하는 대상에게만 메시지를 전송하도록 해야 한다.

⇒ 다른 계층에서 객체의 내부 정보를 꺼내어 주는것에 조심해야 한다라고 생각함

```jsx
// before
Ticket ticket = ticketSeller.getTicketOffice().getTicket();
audience.getBag().minusAmount(ticket.getFee());

// after
public class Theater {
    public void enter(Audience audience) {
        ticketSeller.sellTo(audience);
    }
}

public class TicketSeller {
    public void sellTo(Audience audience) {
        ticketOffice.plusAmount(
                audience.buy(ticketOffice.getTicket())
        );
    }
}

public class Audience {
    public Long buy(Ticket ticket) {
        return bag.hold(ticket);
    }
}

public class Bag {
    public Long hold(Ticket ticket) {
        if (hasInvitation()) {
            this.ticket = ticket;
            return 0L;
        } else {
            this.ticket = ticket;
            minusAmount(ticket.getFee());
            return ticket.getFee();
        }
    }

    private boolean hasInvitation() {
        return invitation != null;
    }

    private void minusAmount(Long amount) {
        this.amount -= amount;
    }
}
```

## Tell Don’t Ask

Hollywood Principle

의존성 부패를 방지한다.

묻게되면 필요 이상 많은 걸 알게 된다

→ 묻는다는 것 ⇒ 메시지 수신자의 상태에 대해 알고싶다는 의미와 같다 ⇒ 절차지향적인 코드

- 상태를 묻는 오퍼레이션을 행동을 요청하는 오퍼레이션으로 대체해 인터페이스를 향상시켜라

→ getter가 나쁜 이유 ⇒ 더 많은 의존성들을 추가적으로 만들어낸다

## 의도를 드러내는 인터페이스

메서드의 이름을 지을 때는 ‘어떻게’가 아니라 ‘무엇’을 하는지 드러내야 한다.

⇒ 다른 메서드들이 같은 책임을 가진 것을 깨닫고 추상화해 메시지로 만들 수 있다 ⇒ 더 유연한 설계

- PeriodCondition.isSatisfiedByPeriod → PeriodCondition.isSatisfiedBy
- 오퍼레이션은 **클라이언트가 객체에게 무엇을 원하는지 표현해야 한다**
    - setTicket이 아니라, sellTo, buy, hold

routine

어떤 절차를 묶어 호출 가능하도록 이름을 부여한 모듈을 말한다.

- procedure
    - 부수효과를 발생시킬 수 있지만 값을 반환할 수 없다.
- command
    - 객체의 상태를 수정하는 오퍼레이션
        - 명령은 상태를 변경할 수 있지만 상태를 반환해서는 안된다.

function

- 값을 반환할 수 있지만 부수효과를 발생시킬 수 없다.
    - query
        - 객체와 관련된 정보를 반환하는 오퍼레이션 ( 질문이 답변을 수정해선 안된다. )

event

- 특정 일자에 실제로 발생하는 사건

recurring schedule

- 특정 시간 간격으로 발생하는 사건 전체를 포괄적으로 말한다. ( 반복 일정 )

명령-쿼리 인터페이스

- 명령과 쿼리를 명확하게 분리해 부수효과를 제거한다.

참조 투명성 (referential transparency)

- 어떤 표현식 e가 있을 때 e의 값으로 e가 나타나는 모든 위치를 교체하더라도 결과가 달라지지 않는 특성을 말한다.
    - 동일한 입력에 대해 항상 동일한 값을 반환하는 특성
    - 수학은 참조 투명성을 엄격하게 준수한다.

불변성 (immutability)

값이 변하지 않는 성질을 말한다.

- 부수효과가 발생하지 않다는 말과 동일하다.
- 객체지향 패러다임은 객체 상태 변경이라는 부수효과를 기반으로 하기 때문에 참조 투명성이 깨진다.
- 명렁-쿼리 분리 원칙을 활용해 제한적이나마 참조 투명성을 이용할 수 있다.
    - **명령형 프로그래밍 ( imperative programming )**
        - 부수효과를 기반으로 하는 프로그래밍 방식
    - **함수형 프로그래밍**
        - 부수효과가 존재하지 않는 수학적 함수에 기반

**참조 투명성의 장점**

- 모든 함수를 이미 알고 있는 결과값으로 대체 가능해 쉽게 계산이 가능하다.
- 모든 곳에서 함수 결과값이 동일하기 때문에 식의 순서를 변경해도 식 결과가 달라지지 않는다.