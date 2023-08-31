# Spring DI, IoC, Bean

# Dependency Injection (의존관계 주입)

## 의존관계

- 정적인 클래스 의존 관계
    - import 코드만 보고도 판단 가능
    - 애플리케이션을 실행하지 않고도 Class diagram을 통해 분석 가능
- 동적인 객체 의존 관계
    - 실행 시점에 결정

## 의존관계 주입

- 애플리케이션 실행 시점(런타임)에 외부에서 실제 구현 객체를 생성하고 클라이언트에 전달해서 클라이언트와 서버의 실제 의존관계가 연결되는 것
- 객체 인스턴스를 생성하고, 그 참조값을 전달해서 연결
- 객체를 직접 생성하는 것이 아니라 외부에서 생성한 후 주입하는 방식
- 클라이언트 코드를 변경하지 않고 클라이언트가 호출하는 대상의 타입 인스턴스를 변경 가능
- **정적인 클래스 의존관계를 변경하지 않고, 동적인 객체 인스턴스 의존관계를 쉽게 변경 가능**
- 모듈간의 결합도가 낮아지고 유연성이 높아짐 → 유지보수, 확장에 용이
- 단위 테스트가 쉬워짐

## 의존관계 주입 방법

### 1. Constructor Injection

```java
public class A {
	
	private B b;

	public A (B b) {
		this.b = b;
	}

}
```

- 스프링에서 권장하는 방식
- 필수적으로 사용해야하는 Reference 없이는 해당 인스턴스를 생성할 수 없도록 강제하기 때문에 좋음

### 2. Setter Injection

```java
public class A {

	private B b;

	public void setB (B b) {
		this.b = b;
	}	

}
```

### 3. Interface Injection

```java
public interface BInjection {
	void inject (B b);
}

public A implements BInjection {
	
	private B b;

	@Override
	public void inject (B b) {
		this.b = b;
	}

}
```

- 어떤 의존성을 주입할 것인지 인터페이스에 명시하고 의존성을 주입받는 클래스는 해당 인터페이스의 구현체로 만듬

## 정리

- 기본적으로 **생성자 주입**을 사용
- 필수값이 아닌 경우 Setter 주입 방식을 옵션으로 부여 (생성자 주입과 동시 사용 가능)

# Inversion of Control (제어의 역전)

- 프로그램의 진행 흐름과 구체적인 구현을 분리시킬 수 있음
- 개발자는 비즈니스 로직에 집중
- 구현체 사이의 변경이 용이
- 객체 간 의존성이 낮아짐

## 기존의 객체 생성 및 실행 순서

1. 객체 생성
2. 의존성 객체 생성 (구현체 클래스 내부에서 생성)
3. 의존성 객체 메서드 호출

## 스프링의 객체 생성 및 실행 순서

1. 객체 생성
2. 의존성 객체 주입
3. 의존성 객체 메서드 호출

## IoC container **(DI container)**

<aside>
💡 컨테이너는 보통 객체의 생명주기를 관리, 생성된 인스턴스들에게 추가적인 기능을 제공하도록 하는 것

</aside>

- 스프링에서 객체를 생성하고 의존관계를 연결해주는 컨테이너
- 객체 관리 주체가 프레임워크(Container)가 되기 때문에 개발자는 로직에 집중
- 의존성 주입은 IoC 컨테이너 안에 있는 Bean끼리 일어난다!

# Bean

<aside>
💡 **스프링 IoC container가 관리하는 자바 객체**

</aside>

- 직접 new 해서 만드는 인스턴스는 Bean이 아님 → IoC 컨테이너가 관리하지 않음

## Bean 등록 방법

### 1. Annotation 사용 → Component scan

- 스프링에서 권장하는 빈 등록 방법
- **@Component** annotation을 사용하여 등록
- stereotype인 **@Controller, @Service, @Repository**와 같은 어노테이션을 써도 그 안에 **@Component**가 존재하여 스프링이 자동으로 스캔하여 빈으로 등록
- 특정한 인터페이스를 구현하는 경우에 자동으로 등록 (Repository 등)
- **@Component**
    - 싱글톤 클래스 빈을 생성하는 어노테이션
    - 선언적(Declarative) 어노테이션
        - 패키지 스캔 안에서 해당 클래스를 정의했으니 빈으로 등록해달라는 뜻
    - **@Service, @Repository** 등 또한 이곳에 속함

### 2. Configuration에 직접 등록하는 방법

- 이건 필요할 때 직접 찾아보고…

# 의존성 주입 → @Autowired / @Inject

## @Autowired

- 생성자 주입, Setter 주입, Field 주입 모두 @Autowired 사용 가능
- 생성자가 하나일 때 생략 가능