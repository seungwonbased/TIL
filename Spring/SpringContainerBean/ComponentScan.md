# Component Scan

- 스프링은 설정 정보가 없어도 자동으로 스프링 빈을 등록하는 컴포넌트 스캔이라는 기능을 제공
- 의존 관계도 자동으로 주입하는 **`@Autowired`**라는 기능도 제공

---

@ComponentScan은 스프링 프레임워크에서 사용하는 애노테이션 중 하나로, 컴포넌트 스캔을 수행하는 기능을 제공합니다. 컴포넌트 스캔은 스프링 컨테이너에서 빈으로 등록할 클래스를 자동으로 검색하고 등록하는 기능을 의미합니다.

@ComponentScan 애노테이션은 보통 @Configuration 애노테이션이 선언된 클래스에서 사용됩니다. 이를 통해 해당 클래스에서 수행하는 컴포넌트 스캔의 범위를 설정할 수 있습니다.

@ComponentScan 애노테이션은 다음과 같은 속성들을 제공합니다.

- basePackages: 검색할 패키지를 지정합니다.
- basePackageClasses: 검색할 클래스를 지정합니다.
- includeFilters: 검색할 클래스를 필터링할 조건을 설정합니다.
- excludeFilters: 제외할 클래스를 필터링할 조건을 설정합니다.

@ComponentScan 애노테이션을 사용하여 컴포넌트 스캔을 수행하면, 해당 패키지나 클래스를 기반으로 자동으로 빈을 등록할 수 있습니다. 이를 통해 스프링 컨테이너에서 빈으로 등록된 클래스를 쉽게 관리하고, 의존성 주입 등의 스프링 기능을 활용할 수 있습니다.

다음은 @ComponentScan을 사용하여 스프링 컨테이너에서 빈을 자동으로 등록하는 예시입니다.

```java
@Configuration
@ComponentScan(basePackages = {"com.example.myapp.service", "com.example.myapp.dao"})
public class AppConfig {
    // configuration code
}
```

위 예시에서는 @Configuration 애노테이션이 선언된 AppConfig 클래스에서 @ComponentScan 애노테이션을 사용하여, com.example.myapp.service 패키지와 com.example.myapp.dao 패키지에서 빈을 검색하도록 지정하고 있습니다.

이렇게 설정된 @ComponentScan 애노테이션은 해당 패키지에서 모든 클래스를 검색하고, @Component, @Service, @Repository, @Controller, @Configuration (@Component의 하위 애노테이션) 등의 스프링 애노테이션이 선언된 클래스를 스프링 컨테이너에 자동으로 등록합니다.

이를 통해 AppConfig 클래스에서는 다른 빈들과 의존성 주입을 통해 서로 상호작용하는 클래스들을 쉽게 관리할 수 있습니다.

---

# 동작 방법

## 1. @ComponentScan

![Scan](https://github.com/seungwonbased/TIL/blob/main/Spring/assets/scan1.png)

- @ComponentScan은 @Component가 붙은 모든 클래스를 스프링 빈으로 등록
- 스프링 빈의 기본 이름은 클래스가 되되, 맨 앞 글자만 소문자로 변경
    - MemberServiceImpl → memberServiceImpl

## 2. @Autowired 의존 관계 자동 주입

![Scan](https://github.com/seungwonbased/TIL/blob/main/Spring/assets/scan2.png)

- 생성자에 @Autowired를 지정하면 스프링 컨테이너가 자동으로 해당 빈을 찾아서 주입
- 기본으로 타입이 같은 빈을 찾아서 주입
- 생성자에 파라미터가 많아도 다 찾아서 자동으로 주입

# 탐색 위치와 기본 스캔 대상

### 탐색 위치 지정

- basePackages: 탐색할 패키지의 시작 위치를 지정, 이 패키지를 포함해서 하위 패키지를 모두 탐색
    - basePackages={”hello.core”, “hello.service”}와 같이 여러 개의 시작 위치를 지정할 수도 있음
- basePakageClasses: 지정한 클래스의 패키지를 탐색 시작 위치로 지정
- 지정하지 않으면 @ComponentScan이 붙은 클래스의 패키지가 시작 위치가 됨

### 권장하는 방법

설정 정보 클래스의 위치를 최상단에 두어 모든 컴포넌트가 스캔될 수 있도록 하는 것, 스프링 부트도 이 방법을 기본으로 제공

예를 들어, 프로젝트가 다음 구조로 되어 있다면

**`com.hello`**

**`com.hello.service`**

**`com.hello.repository`**

**`com.hello`** → 프로젝트 시작 루트, 여기에 AppConfig와 같은 메인 설정 정보를 두고, @ComponentScan 애노테이션을 붙인 후 basePackage 지정은 생략

이렇게 하면 **`com.hello`**를 포함한 하위는 모두 자동으로 컴포넌트 스캔의 대상이 된다.

참고로 스프링 부트를 사용하면 스프링 부트의 대표 시작 정보인 **`@SpringBootApplication`**을 이 프로젝트 시작 루트 위치에 두는 것이 관례이다 (그리고 이 설정 안에 **`@ComponentScan`**이 들어있음! 그래서 스프링부트를 쓰면 자동으로 스프링 빈이 다 등록됨)

### 기본 스캔 대상

- @Component: 컴포넌트 스캔에서 사용
- @Component 하위 애노테이션
    - @Controller: 스프링 MVC 컨트롤러에서 사용
    - @Service: 스프링 비즈니스 로직에서 사용
    - @Repository: 스프링 데이터 접근 계층에서 사용
    - @Configuration: 스프링 설정 정보에서 사용

<aside>
💡 애노테이션은 사실 상속관계가 있는 것은 아니다. 그래서 애노테이션이 특정 애노테이션을 들고 있는 것을 인식할 수 있는 것은 자바단에서 지원하는 것이 아니라 스프링단에서 지원하는 기능이다.

</aside>

- 컴포넌트 스캔의 용도 뿐만 아니라 다음 애노테이션이 있으면 스프링은 부가 기능을 수행
    - @Controller : 스프링 MVC 컨트롤러로 인식
    - @Repository : 스프링 데이터 접근 계층으로 인식하고, 데이터 계층의 예외를 스프링 예외로 변환
    - @Configuration : 앞서 보았듯이 스프링 설정 정보로 인식하고, 스프링 빈이 싱글톤을 유지하도록 추가 처리
    - @Service : 사실 @Service 는 특별한 처리를 하지 않는다. 대신 개발자들이 핵심 비즈니스 로직이 여기에 있겠구나 라고 비즈니스 계층을 인식하는데 도움이 된다.