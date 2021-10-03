## Vue
---

### 라우터
---
```
  싱글페이지 어플리케이션을 구현하거나 페이지간 이동할 때 사용
```
+ 설치
  + cdn 
  ```
    <script src="https://unpkg.com/vue-router/dist/vue-router.js">
  ```
  
  + npm
  ```
    npm install vue-router
  ```
  
  + 라우터 생성 및 인스턴스 연결
  ![image](https://user-images.githubusercontent.com/76584547/135714349-5b0d91b5-2fcc-4a41-9930-1fc46ead5158.png)

  + /login Router View 접속
    + router-view 태그 안에 loginComponent가 들어감 
  ![image](https://user-images.githubusercontent.com/76584547/135716310-430e7f6b-d3a9-45f6-ad69-253f05ccf390.png)


  + router-link를 사용하여 "/login" "/main" 접속 (= <a href="">)
  
  ![image](https://user-images.githubusercontent.com/76584547/135717633-803cf81b-c293-4529-ae1f-eac2b3d0fef0.png)

 
### Axios
---
```
  뷰에서 권고하는 HTTP 통신 라이브러리는 액시오스(Axios)입니다. Promise 기반의 HTTP 통신 라이브러리이며 상대적으로 다른 
  HTTP 통신 라이브러리들에 비해 문서화가 잘되어 있고 API가 다양합니다.
```
  + 설치
    + cdn
    ```
      <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
    ```
  
    + npm
    ```
      npm install axios
    ```
