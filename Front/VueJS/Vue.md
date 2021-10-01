## Vue.js
---
```
  MVVM 패턴의 뷰모델(ViewModel)레이어에 해당하는 화면(View)단 라이브러리
```
![image](https://user-images.githubusercontent.com/76584547/135585256-19cd412e-c4f4-490b-a67e-f72fab34ef30.png)
+ 사용자가 마우스로 버튼 클릭 시 해당 행위를 DOM Listeners로 Vue(View Model)에서 그런 이벤트들을 잡고 자바스크립트(MODEL)의 특정 로직 실행
+ 자바스크립트의 데이터가 변했을 경우(문자열이나 숫자바 바뀌었다 등) Data Bindings(View Model)를 이용해서 화면(View)에 반영하게 된다

### Reactivity 
----
```
  데이터의 변화를 라이브러리에서 감지해서 화면을 알아서 자동으로 그려주는 것
```
#### Object.defineProperty()
```
  객체의 동작을 재정의하는 API
```
+ Object.defineProperty 

<br>

![image](https://user-images.githubusercontent.com/76584547/135591852-87d57fc7-bf98-41d4-8416-4e785ba4f6aa.png)

+ get : 속성에 접근했을 때의 동작 정의
+ set : 속성에 값을 할당했을 때의 동작 정의
![image](https://user-images.githubusercontent.com/76584547/135592535-b7679331-c7a1-42f4-97ba-11ab84b0c0b3.png)



#### 즉시 실행함수 (Reactivity 라이브러리화)
---
```
  어플리케이션 로직에 노출되지 않게 즉시 실행함수 안에 있는 로직들을 또 다른 스코프(즉시 실행함수 안)에 넣어주면서 
  변수의 유효범위를 관리하고 있다.
```
![image](https://user-images.githubusercontent.com/76584547/135593685-f73ba473-3859-4b37-81dc-713f5c2ac3f7.png)
