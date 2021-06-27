## SpringSecurity
```
  
```

### 스프링 시큐리티 구조
----
![image](https://user-images.githubusercontent.com/76584547/123547724-e0201100-d79c-11eb-813d-70985ef6479a.png)

+ 동작 플로우
  ```
  1. 사용자가 로그인 정보와 함께 인증 요청(Http Request) 
  
  2. AuthenticationFilter가 이 요청을 가로챕니다. 
     이 때 가로챈 정보를 통해 UsernamePasswordAuthenticationToken이라는 인증용 객체를 생성합니다. 
  
  3. AuthenticationManager의 구현체인 ProviderManager에게 UsernamePasswordAuthenticationToken 객체를 전달합니다. 
  
  4. 다시 AuthenticationProvider에 UsernamePasswordAuthenticationToken 객체를 전달합니다. 
  
  5. 실제 데이터베이스에서 사용자 인증정보를 가져오는 UserDetailsService에 사용자 정보(아이디)를 넘겨줍니다. 
  
  6. 넘겨받은 사용자 정보를 통해 DB에서 찾은 사용자 정보인 UserDetails 객체를 만듭니다.  
  
     이 때 UserDetails 는 인증용 객체와 도메인용 객체를 분리하지 않고 인증용 객체에 상속해서 사용하기도 합니다. 
  
  7. AuthenticationProvider는 UserDetails를 넘겨받고 사용자 정보를 비교합니다. 
  
  8. 인증이 완료되면 권한 등의 사용자 정보를 담은 Authentication 객체를 반환합니다. 
  
  9. 다시 최초의 AuthenticationFilter에 Authentication 객체가 반환됩니다. 
  
  10. Authentication 객체를 SecurityContext에 저장합니다.

  출처 : https://webfirewood.tistory.com/115
  ```
  
  ### 스프링 시큐리티 필터
  ---
  
  ![image](https://user-images.githubusercontent.com/76584547/123547969-dd71eb80-d79d-11eb-8b40-800012050068.png)
  
  ```
  - SecurityContextPersistentFilter : SecurityContextRepository에서 SecurityContext를 가져와서 
    SecurityContextHolder에 주입하거나 반대로 저장하는 역할을 합니다.
    
  - LogoutFilter : logout 요청을 감시하며, 요청시 인증 주체(Principal)를 로그아웃 시킵니다.
  
  - UsernamePasswordAuthenticationFilter : login 요청을 감시하며, 인증 과정을 진행합니다.
  
  - DefaultLoginPageGenerationFilter : 사용자가 별도의 로그인 페이지를 구현하지 않은 경우, 스프링에서 기본적으로 설정한 로그인 페이지로 넘어가게 합니다.
  
  - BasicAuthenticationFilter : HTTP 요청의 (BASIC)인증 헤더를 처리하여 결과를 SecurityContextHolder에 저장합니다.
  
  - RememberMeAuthenticationFilter : SecurityContext에 인증(Authentication) 객체가 있는지 확인하고 
  - RememberMeServices를 구현한 객체 요청이 있을 경우, RememberMe를 인증 토큰으로 컨텍스트에 주입합니다.
  
  - AnonymousAuthenticationFilter : 이 필터가 호출되는 시점까지 사용자 정보가 인증되지 않았다면 익명 사용자로 취급합니다.
  
  - SessionManagementFilter : 요청이 시작된 이후 인증된 사용자인지 확인하고, 인증된 사용자일 경우 SessionAuthenticationStrategy를 
    호출하여 세션 고정 보호 매커니즘을 활성화 하거나 여러 동시 로그인을 확인하는 것과 같은 세션 관련 활동을 수행합니다.
  
  - ExceptionTranslationFilter : 필터체인 내에서 발생되는 모든 예외를 처리합니다.
  
  - FilterSecurityInterceptor : AccessDecisionManager로 권한부여처리를 위임하고 HTTP 리소스의 보안 처리를 수행합니다.

  출처 : https://webfirewood.tistory.com/115
```

