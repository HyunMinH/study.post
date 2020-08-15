# 설계 개선하기

## 자율성을 높이자

1. Theator의 enter 메서드에서 TicketOffice에 접근하는 모든 코드를 -> TicketSeller 내부로 숨기기

    ### 소극장
    ```java
    public class Theator{
        ...

        public void enter(Audience audience){
            ticketSeller.sellTo(audience);
        }
    }
    ```

    ### 티켓 판매원
    ```java
    public class TicketSeller{
        ...

        //public TicketOffice getTicketOffice(){
        //    return ticketOffice;
        //}

        public void sellTo(Audience audience){
            if(audience.getBag().hasInviation()){
                Ticket ticket = ticketSeller.getTicketOffice().getTicket();
                audience.getBag().setTicket(ticket);
            }else{
                Ticket ticket = ticketSeller.getTicketOffice().getTicket();
                audience.getBag().minusAmount(ticket.getFee());
                ticketSeller.getTicketOffice().plusAmount(ticket.getFee());
                audience.getBag().setTicket(ticket);
            }
        }
    }
    ```

2. 동일한 방법으로 Audience의 캡슐화 개선 -> Bag에 접근하는 모든 로직을 Audience 내부로

    ### 티켓 판매원

    ```java
    public class TicketSeller{
        ...

        public void sellTo(Audience audience){
            ticketOffice.plusAmount(audience.buy(ticketOffice.getTicket()));
        }
    }
    ```

    ### 관람객

    ```java
    public class Audience{
        ...

        public Long buy(Ticket ticket){
            if(bag.hasInvitation()){
                bag.setTicket(ticket);
                return 0L;
            }else{
                bag.setTicket(ticket);
                bag.minusAmount(ticket.getFee());
                return ticket.getFee();
            }
        }
    }
    ```

## 무엇이 개선되었는가

1. 우리의 직관과 일치 -> 이해하기 쉬워짐

2. Audience나 TicketSeller의 내부 구현을 변경하더라고 Theator를 함께 변경할 필요  X -> 변경 용이성 증가

## 캡슐화와 응집도

- 객체의 응집도를 높이기 위해서는

    - 객체 스스로 자신의 데이터를 책임져야 한다.


- 훌룡한 객체지향 설계는
    -  외부의 간섭을 최대한 배제하고 메시지를 통해서만 협력하는 자율적인 객체들의 공동체 만들기

## 절차지향과 객체지향

-  절차지향 프로그래밍

   - 개선 전 코드처럼 프로세스와 데이터를 별도의 모듈에 위치시키는 방식

-  객체지향 프로그래밍

   - 데이터와 프로세스가 동일한 모듈 내부에 위치하도록 프로그래밍하는 방식

## 책임의 이동

- 두 방식 사이의 근본적은 차이를 만드는 것은 책임의 이동이다!

    - 자기 일은 자기가 하도록

## 의인화

- 능동적이고 자율적인 존재로 소프트웨어를 객체를 설계하는 원칙

  - 수동적인 존재(현실 세계) -> 능동적이고 자율적인 존재로(객체지향 세계)