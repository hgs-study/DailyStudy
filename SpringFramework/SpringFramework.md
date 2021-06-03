## SpringFramework
+ 스프링 컨테이너
``` 
  ㅇ 종류
    1. BeanFactory (인터페이스)
      - 클라이언트 요청에 의해서 Bean 객체가 사용되는 시점 (Lazy Loading)에 객체를 생성하는 방식 사용
    2. Application Context (인터페이스)
      - BeanFactory를 상속 받고있다.
      - BeanFactory와 다른 점은 컨테이너가 구동되는 시점에 객체들을 생성한다.(Pre-Loading 방식)
      - 실제로 스프링 컨테이너 또는 IoC 컨테이너라고 말하는 것은 바로 이 ApplicationContext 인터페이스를 구현한 클래스의 오브젝트다.
      - 스프링 애플리케이션은 최소한 하나 이상의 IoC 컨테이너, 즉 애플리케이션 컨텍스트 오브젝트를 갖고 있다.
    
  ㅇ생성  
  1. 스프링 컨테이너 생성
    - AppicationContext ctx = new AnnotationConfigApplicationContext(AppCtx.class);
  2. 빈 객체 가져오기
    - TestBean testBean = new ctx.getBean("testBean",TestBean.class);
```

+ 스프링 컨테이너 생명주기 (Life Cycle)
``` 
  ㅇ 생성
    - ctx.register(); // 빈 추가로 등록
  ㅇ 빈 설정
    - ctx.refresh(); // 갱신해야 반영됨
  ㅇ 사용
    - ctx.close(); // 소멸, 빈도 함께
  ㅇ 소멸
    - ctx.registerShutdownHook(); // JVM소멸시 자동으로 소멸되게끔
```

+ Bean 생명주기 (Life Cycle)
``` 
  ※ 객체 생성 - 초기화 - 사용 - 소멸 순으로 유지된다.
    - 스프링 컨테이너에 의해 객체가 생성되고 초기화와 소멸의 과정을 거친다. 각 초기화와 소멸을 구현하는 방법에는 각각 3가지가 존재한다.

  1. 초기화
    - @PostConstruct 활용
    - InitializingBean 인터페이스 구현하여 사용, afterPropertiesSet 메서드 override
    - 커스텀 init 메서드 (빈 정의 xml에 init-method 로 메서드 명 지정 / @Bean(initMethod="init"), init메서드 활용)
   
   2. 소멸
    - @PreDestroy
    - DisposableBean 인터페이스
    - 커스텀 destroy 메서드 정의

   3. 사용(스프링 컨테이너, 빈 이름 가져오기)
    - ApplicationContextAware
    - BeanNameAware (인터페이스를 활용한다. implement한 클래스에서 사용할 수 있다.)

   4. Bean 범위(scope)
    - Singleton (default)
    - Prototype (매번 호출될 때마다 인스턴스 생성됨, 스프링 컨테이너가 소멸되도 인스턴스들은 계속 유지됨)
```


+ 스프링 DI
```
  1. 생성자 방식
  2. 세터 메서드 방식
  3. 어노테이션
```

+ 스프링 빈은 쓰레드 세이프하지 않다
```
  + 값이 변할 수 있는 인스턴스 멤버 변수가 존재한다면 쓰레드 세이프 하지 않습니다. 
  + 무상태(stateless)'인 빈(bean) 클래스를 만들어야 합니다. 
    + 참조.1: https://siyoon210.tistory.com/140
    + 참조.2: https://doflamingo.tistory.com/44
  + 해결
    1. 무상태인 빈 클래스를 만든다.
    2. scope = prototype을 사용한다 (프로토타입)
```


+ 스프링 Proxy
```
spring -> proxy 의존 많이 하고 있음

다이나믹 프록시 패턴
1. CGLIB - 부트 기본 
2. JDK Dynamic Proxy - BCI (레거시)
 - 인터페이스 기반 (단점 : 인터페이스 강제화)
```

+ 어노테이션
``` 
  ㅇ Target
    - ElementType (사용할 위치)
        - TYPE (클래스, 인터페이스, ENUM)
        - FIELD
        - METHOD
        - PARAMETER
        - CONSTRUCTOR
        ...
   ㅇ Retention (구동)
    - RetentionPolicy (지속 시간)
        - RUNTIME : 런타임시에도 존재, 커스텀 어노테이션 때 주로 사용 (Reflection 사용 가능)
        - CLASS : 컴파일 타임 때만 존재, 런타임 때는 없어진다.
        - SOURCE : 컴파일 후에 정보가 사라진다. EX) @Override와 @SuppressWarnings
```


+ @Valid
``` 
[SpringLegacy-maven]
<dependency> 
	<groupId>javax.validation</groupId> 
	<artifactId>validation-api</artifactId> 
	<version>1.0.0.GA</version> 
</dependency> 

<dependency> 
	<groupId>org.hibernate</groupId> 
	<artifactId>hibernate-validator</artifactId> 
	<version>4.2.0.Final</version> 
</dependency>

[SpringBoot-gradle]
    implementation 'org.springframework.boot:spring-boot-starter-validation'
```

+ @Valid
  - DTO에 들어오는 유효성 검사 확인 가능
  - @NotNull(boolean, int) -null값 허용 안한다
  - @NotEmpty - 공백 허용 안한다.
  - @NotBlank(String)  - null값과 공백 허용 안한다.


+ MapStruct
``` 
- MapStruct & Lombok gradle 의존성 순서&버전 중요 ★

ext {
    mapstructVersion = "1.3.0.Final"
    lombokVersion = "1.18.6"
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation "org.mapstruct:mapstruct:${mapstructVersion}"
    annotationProcessor "org.mapstruct:mapstruct-processor:${mapstructVersion}"
    annotationProcessor "org.projectlombok:lombok:${lombokVersion}"
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```
