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
  
  + 구현
    + response 받은 data를 users에 담기 위해 "var vm = this"를 명시
    ![image](https://user-images.githubusercontent.com/76584547/136000436-212b0656-5f97-447e-b468-097e2c5b898c.png)

### Computed 속성
---
```
  data의 값에 따라서 바뀌는 값을 처리할 때 사용하는 속성
```
+ doubleNum Function 정의
  
![image](https://user-images.githubusercontent.com/76584547/138040408-2c77088e-fb3c-4a24-898a-379441013c2f.png)

  
### 디렉티브
---
```
  v-bind 처럼 태그에 사용함으로써, Vue객체 접근 가능
```
  
+ id와 class 적용
  
![image](https://user-images.githubusercontent.com/76584547/138043985-f8ee313b-6ad0-47c2-a2ff-765e726fa60b.png)

+ on : click, keyup, keypress 등등 사용 가능
 
![image](https://user-images.githubusercontent.com/76584547/138065374-b87e51ee-defb-48c7-add6-94c9daeb7906.png)
  
+ eventModifier (keyup뒤에 .enter 붙이고 사용 / 더 있으니까 공식 문서 참고)
  
![image](https://user-images.githubusercontent.com/76584547/138065619-1b334e39-da53-4936-93aa-74d85c0f1e11.png)


