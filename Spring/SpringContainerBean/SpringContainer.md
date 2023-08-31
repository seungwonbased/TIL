# Spring Container

스프링 프레임워크는 스프링 기반 애플리케이션의 컴포넌트를 관리하는 책임이 있는 컨테이너를 제공합니다. 이 컨테이너는 **스프링 컨테이너 또는 IoC 컨테이너**라고도 합니다. IoC는 Inversion of Control의 약자입니다.

**스프링 컨테이너는 객체를 생성하고, 서로 연결하고, 구성하며, 생성부터 소멸까지 완전한 라이프사이클 관리를 담당**합니다. **이를 위해 컨테이너는 의존성 주입을 사용**합니다.

스프링 컨테이너에는 두 가지 유형이 있습니다:

![container](https://github.com/seungwonbased/TIL/blob/main/Spring/assets/container1.png)

## 1. BeanFactory

BeanFactory는 가장 간단한 컨테이너로, 의존성 주입, 라이프사이클 관리, 싱글톤 및 프로토타입 처리와 같은 기본 서비스 및 구성 메커니즘을 제공합니다.

- 스프링 컨테이너의 최상위 인터페이스
- 스프링 빈을 관리하고 조회하는 역할 담장
- **`getBean()`**을 제공

## 2. ApplicationContext

![container](https://github.com/seungwonbased/TIL/blob/main/Spring/assets/container2.png)

- BeanFactory의 모든 기능을 상속받아서 제공
- BeanFactory보다 더 다양한 부가기능 제공 → 애플리케이션 개발을 위함
    - 메세지 소스를 활용한 국제화 기능
        - 예를 들어 한국에서 들어오면 한국어로, 영어권에서 들어오면 영어로 출력
    - 환경 변수
        - 로컬, 개발, 운영 서버를 구분해서 처리
    - 애플리케이션 이벤트
        - 이벤트를 발행하고 구독하는 모델을 편리하게 지원
    - 편리한 리소스 조회
        - 파일, 클래스패스, 외부 등에서 리소스를 편리하게 조회

## 스프링 컨테이너의 주요 기능

1. **경량화**: 스프링 컨테이너는 경량화되어 있으며, 개발자가 기본 클래스를 확장하거나 인터페이스를 구현하도록 강요하지 않습니다.
2. **제어의 역전**: 스프링 컨테이너는 IoC를 구현하므로 객체 생성 프로세스를 제어하고 필요한 의존성을 객체에 주입합니다.
3. **의존성 주입**: 스프링 컨테이너는 스프링 기반 애플리케이션의 컴포넌트를 관리하기 위해 의존성 주입을 사용합니다. 이를 통해 모듈화 및 유연한 코드 작성이 용이해집니다.
4. **라이프사이클 관리**: 스프링 컨테이너는 생성부터 소멸까지의 객체의 전체 라이프사이클을 관리합니다. 이로 인해 객체가 적절하게 초기화되고 소멸됩니다.
5. **구성 관리**: 스프링 컨테이너는 XML, 주석 또는 Java 기반 구성을 사용하여 애플리케이션을 구성할 수 있는 유연한 구성 메커니즘을 제공합니다.

결론적으로, 스프링 컨테이너는 스프링 기반 애플리케이션의 컴포넌트를 관리하기 위한 강력하고 유연한 도구입니다. 제어의 역전과 의존성 주입 구현을 위한 경량화 및 비침투적 프레임워크를 제공합니다.

# 기초

- ApplicationContext를 스프링 컨테이너라고 함 (인터페이스임)
- 애노테이션 기반의 자바 설정 클래스로 만들 수 있음
- 스프링 컨테이너는 **@Configuration**이 붙은 클래스를 설정 정보로 사용
    - 여기서 @Bean이라 적인 메서드를 모두 호출하여 반환된 객체를 스프링 컨테이너에 등록
        - 이렇게 등록된 객체가 Spring bean
        - 스프링 빈은 **@Bean**이 붙은 메서드 명을 스프링 빈의 이름으로 사용
- 참고: [Spring DI, IoC, Bean](DIIoCBean.md)

# Bean 등록 과정

1. **`@Component`**나 **`@Service`**, **`@Controller`**, **`@Repository`** 등의 어노테이션을 이용하여 Bean을 등록할 클래스를 지정합니다.
2. 등록할 클래스에서 필요한 의존성을 **`@Autowired`** 어노테이션을 이용하여 자동으로 주입합니다. (의존성 주입은 생성자 주입, 필드 주입, 메서드 주입 방식 등이 있습니다.)
3. **`@Configuration`** 어노테이션을 이용하여 설정 클래스를 만듭니다.
    1. 설정 정보를 참고하여 의존관계를 주입
4. 설정 클래스에서 **`@Bean`** 어노테이션을 이용하여 Bean을 등록합니다. **`@Bean`** 어노테이션을 사용하면 메서드가 반환하는 객체를 Bean으로 등록합니다.
5. Spring 컨테이너는 등록된 Bean을 관리하고, 필요할 때 해당 Bean을 반환합니다.

예를 들어, 다음과 같이 **`@Service`** 어노테이션을 이용하여 UserService 클래스를 Bean으로 등록할 수 있습니다.

```java
@Service
public class UserService {
  @Autowired
  private UserRepository userRepository;

  // ...
}
```

위 코드에서 **`UserService`** 클래스는 **`@Service`** 어노테이션을 이용하여 Bean으로 등록되며, 필요한 **`UserRepository`** 클래스는 **`@Autowired`** 어노테이션을 이용하여 자동으로 주입됩니다.

또한, 다음과 같이 **`@Configuration`** 어노테이션을 이용하여 AppConfig 클래스를 만들어 Bean을 등록할 수 있습니다.

```java
@Configuration
public class AppConfig {
  @Bean
  public UserService userService() {
    return new UserService();
  }

  @Bean
  public UserRepository userRepository() {
    return new UserRepository();
  }

  // ...
}
```

위 코드에서 **`AppConfig`** 클래스는 **`@Configuration`** 어노테이션을 이용하여 설정 클래스로 지정되며, **`@Bean`** 어노테이션을 이용하여 **`UserService`**와 **`UserRepository`** 클래스를 Bean으로 등록합니다. 이후 Spring 컨테이너는 등록된 Bean을 관리하고, 필요할 때 해당 Bean을 반환합니다.

<aside>
🚨 빈 이름은 항상 다른 이름을 부여해야 함, 같은 이름을 부여하면 다른 빈이 무시되거나 덮어버릴 수 있음

</aside>

# Bean 조회

Spring에서 Bean을 조회하는 방법은 크게 두 가지가 있습니다.

1. **`@Autowired`** 어노테이션을 이용한 자동 주입 방법
    - Spring 컨테이너에서 해당 타입의 Bean을 찾아 자동으로 주입합니다.
    - **`@Autowired`** 어노테이션을 필드, 생성자, 메서드에 적용할 수 있습니다.
    
    ```java
    @Autowired
    private UserService userService;
    
    // 또는
    
    private final UserService userService;
    
    public SomeService(UserService userService) {
      this.userService = userService;
    }
    
    // 또는
    
    @Autowired
    public void setUserService(UserService userService) {
      this.userService = userService;
    }
    ```
    
2. Spring 컨테이너에서 직접 Bean을 조회하는 방법
    - **`ApplicationContext`** 인터페이스를 이용하여 Bean을 직접 조회할 수 있습니다.
    - **`getBean()`** 메서드를 이용하여 Bean의 이름 또는 타입을 지정하여 조회할 수 있습니다.
    
    ```java
    // Bean의 이름으로 조회
    UserService userService = applicationContext.getBean("userService", UserService.class);
    
    // Bean의 타입으로 조회
    UserService userService = applicationContext.getBean(UserService.class);
    ```
    
    이 때, Bean 이름은 클래스 이름의 첫 글자를 소문자로 변환한 이름입니다. 예를 들어, **`UserService`** 클래스는 **`userService`**라는 이름으로 등록됩니다. 이름을 지정하지 않은 경우에는 타입으로 Bean을 조회합니다.
    

위 두 가지 방법 중 적절한 방법을 선택하여 Bean을 조회할 수 있습니다.

## 조회 - 상속 관계

- 부모 타입으로 조회하면, 자식 타입도 함께 조회됨
    - 그래서 `**Object**` 타입으로 조회하면 모든 스프링 빈을 조회

# 다양한 설정 형식 지원

![container](https://github.com/seungwonbased/TIL/blob/main/Spring/assets/container3.png)

## 애노테이션 기반 자바 코드 설정 사용

- **`AnnotationConfigApplicationContext(AppConfig.class)`**
- **`AnnotationConfigApplicationContext`** 클래스를 사용하며 자바 코드로 된 설정 정보를 넘기면 됨

## XML 기반 설정 사용

# 스프링 빈 설정 메타 정보 - BeanDefinition

스프링이 다양한 설정 형식을 지원할 수 있는 이유는 BeanDefinition 때문이다.

쉽게 이야기해서 ‘역할과 구현을 개념적으로 나눈 것’

**스프링이 다양한 형태의 설정 정보를 BeanDefinition으로 추상화해서 사용한다는 것만 알아두면 됨**

- XML을 읽어서 BeanDefinition을 만듬
- 자바 코드를 읽어서 BeanDefinition을 만듬
- 스프링 컨테이너는 자바 코드인지, XML인지 몰라도 오직 BeanDefinition만 만들면 된다

## BeanDefinition

- 빈 설정 메타 정보
- **`@Bean`**, **`<bean>`** 당 각각 하나씩 메타 정보가 생성
- 스프링 컨테이너는 이 메타 정보를 기반으로 스프링 빈을 생성
- 스프링 컨테이너는 BeanDefinition에만 의존
- 정보가 무슨 종류가 있는지 보려면 구글링
- 실무에서 사용할 일은 거의 없음