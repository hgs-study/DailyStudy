### SpringCloud-Gateway
----
```
  - 기존 Netflix Zuul은 비동기 처리하지 못했지만 2점대 버전으로 올라와서 가능하다고 했으나 라이브러리 호환성 문제가 남아있다.
  - SpringCloud-Gateway 비동기 처리 가능하며 SpringBoot 2.4이상에서 사용할 수 있다.
  - 동기서버 (Tomcat) vs 비동기 서버(Netty)
```
+ 의존성


  ![image](https://user-images.githubusercontent.com/76584547/120929435-f699f600-c723-11eb-86b3-4ff7317daff0.png)

+ 설정파일 


  ![image](https://user-images.githubusercontent.com/76584547/120929472-1d582c80-c724-11eb-8c0c-235bac7df53c.png)

  + cloudgate way predicates는 해당 service uri 뒤에 붙어서 매핑된다.

### Filter
----
+ API Gateway에서 필터 사용


![image](https://user-images.githubusercontent.com/76584547/121021204-1987e100-c7dc-11eb-81a3-d6ce4bafea11.png)

![image](https://user-images.githubusercontent.com/76584547/121030478-79828580-c7e4-11eb-9d98-a8ad6e8377a2.png)


+ application.yml 설정 주석 (config파일로 사용하려고)


![image](https://user-images.githubusercontent.com/76584547/121026733-5a362900-c7e1-11eb-8ce9-7d2285963eae.png)


+ FilterConfig 생성

![image](https://user-images.githubusercontent.com/76584547/121026946-88b40400-c7e1-11eb-884c-e55f7432d04b.png)

  + RouteLocator를 반환해야 application.yml에서 설정한 것처럼 라우터 역할을 한다.
  + RouteLocatorBuilder 인자로 받아 빌더를 라우트 설정 후 리턴
  + 각각의 route path와 uri로 라우트 시키고 filter를 작성한다 (request, response header에 테스트 문구 삽입)

+ 결과
![image](https://user-images.githubusercontent.com/76584547/121027328-d7619e00-c7e1-11eb-91cb-a7c7a745cef5.png)


+ application.yml 활용


![image](https://user-images.githubusercontent.com/76584547/121030113-26a8ce00-c7e4-11eb-8fee-4eee7bc2b320.png)
