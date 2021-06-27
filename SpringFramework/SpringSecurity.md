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
  
  
