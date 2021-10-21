## Vue

### 싱글 파일 컴포넌트
---
  
  #### data 객체 -> data function()
  ---
  ```
    .vue 파일을 사용하게 되면 컴포넌트를 재사용하는 경우가 늘어나기 때문에 여러 개의 컴포넌트가 동일한 값을 
    참조하면 안되기 때문에 return으로 새 객체 리턴
  ```
  ![image](https://user-images.githubusercontent.com/76584547/138279955-f9a663e4-e0b9-4b71-9624-d005d43b9121.png)

  
  #### 외부 Component 사용 
  ---  
  ```
    외부 Component를 import해서 app-header에 넣고 <app-header> 태그로 사용한다
  ```
  ![image](https://user-images.githubusercontent.com/76584547/138281175-4a1a35da-d35f-4f2d-adb4-4ede3a267227.png)

  
  #### 외부 Component props
  ---
  + App.vue
  
  ![image](https://user-images.githubusercontent.com/76584547/138284503-8fed9a97-4255-42a0-a3e6-3462ef6c7cb5.png)

  + AppHeader.vue
  
  ![image](https://user-images.githubusercontent.com/76584547/138284583-31ed9029-61df-46ca-873b-7e58c40d2771.png)

  + str 값을 바꾸면 props로 인해 propsdata 값 변경
  
  ![image](https://user-images.githubusercontent.com/76584547/138284724-9a5a7cf1-e2c5-41da-a853-0539cbfa0062.png)


  #### 외부 Component emit
  ---
  + App.vue
  
  ![image](https://user-images.githubusercontent.com/76584547/138285070-abaabb2a-65eb-47fa-9209-a5b19501b448.png)

  + AppHeader.vue
  
  ![image](https://user-images.githubusercontent.com/76584547/138284915-1b844387-0096-4340-8e95-84e4144b46e1.png)

  + 버튼 클릭 시 header -> hi로 텍스트 변경
  
  ![image](https://user-images.githubusercontent.com/76584547/138285154-164f1d18-8614-4d06-8292-9fa5aab77d7a.png)
  

### form으로 데이터 보내기
---
+ form v-on:submit.prevent (새로고침 없이 보내기) , v-model으로 매핑
![image](https://user-images.githubusercontent.com/76584547/138295067-2c61101e-6ff7-40a3-93bf-b30fca82f2d1.png)

+ axios로 데이터 통신
![image](https://user-images.githubusercontent.com/76584547/138295163-6b689dbc-bad8-4a0f-bded-ae164451e12f.png)

+ 결과 OK
![image](https://user-images.githubusercontent.com/76584547/138295251-527a4162-2b84-4128-bc98-968f4043b0f6.png)


### 참고
---
  + 이벤트 버블링 & 캡처링
  ```
    https://joshua1988.github.io/web-development/javascript/event-propagation-delegation/
  ```
