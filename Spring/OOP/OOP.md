# Object Oriented Programming

# 특징

- Abstraction
- Encapsulation
- Inheritance
- Polymorphism

# SOLID

## SRP, Single Responsibility Principle

- 하나의 클래스는 하나의 책임만 가져야 한다.
- 하나의 책임이라는 것은 모호하다.
    - 클 수도 있고, 작을 수도 있다.
    - 문맥과 상황에 따라 다르다.
- 중요한 것은 **변경**, 변경이 있을 때 파급 효과가 적으면 단일 책임 원칙을 잘 따른 것
- 예: UI 변경, 객체의 생성과 사용을 분리

## OCP, Open / Closed Principle

- 소프트웨어 요소는 확장에는 열려 있으나 변경에는 닫혀 있어야 한다.
- **다형성을 활용하여 확장**
- 인터페이스를 구현한 새로운 클래스를 하나 만들어서 새로운 기능을 구현
- 지금까지 배운 역할과 구현의 분리를 생각
- 객체를 생성하고 연관관계를 맺어주는 별도의 조립, 설정자가 필요
    
    → Spring container
    

## LSP, Liskov Substitution Principle

- 프로그램의 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 한다.
- 다형성에서 하위 클래스는 인터페이스 규약을 다 지켜야 한다는 것, 다형성을 지원하기 위한 원칙, 인터페이스를 구현한 구현체를 믿고 사용하려면 이 원칙이 필요
- 단순히 컴파일을 성공하는 것을 넘어서는 이야기

## ISP, Interface Segregation Principle

- 특정 클라이언트를 위한 인터페이스 여러 개가 범용 인터페이스 하나보다 낫다.
    - 자동차 인터페이스 → 운전 인터페이스, 정비 인터페이스
    - 사용자 클라이언트 → 운전자 클라이언트, 정비사 인터페이스
- 분리하면 정비 인터페이스가 변해도 운전사 인터페이스에 영향이 없음
- 인터페이스가 명확해지고 대체 가능성이 높아진다.

## DIP, Dependency Inversion Principle

- 프로그래머는 추상화에 의존해야지, 구체화에 의존하면 안 된다.
- 쉽게 말해 구현 클래스에 의존하지 말고, 인터페이스에 의존하라는 뜻
- 역할에 의존하게 해야 한다는 것과 같다.
- 객체 세상도 클라이언트가 인터페이스에 의존해야 유연하게 구현체를 변경할 수 있다. 구현체에 의존하면 변경이 아주 어려워진다.

# 객체지향 설계와 스프링

- 다형성만으로는 OCP, DIP를 지킬 수 없음
- 스프링은 DI, DI container와 같은 기술로 다형성 + OCP, DIP를 가능하게 지원
    - 클라이언트 코드의 변경 없이 기능 확장
    - 쉽게 부품을 교체하듯이 개발