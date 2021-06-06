### SpringCloud-Gateway
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
