## SpringFramework
+ 스프링 컨테이너
``` 
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
