## Vue#2

#### 컴포넌트 통신
----
```
  - 컴포넌트끼리 상 하위 관계가 맺어지는데, 컴포넌트끼리 데이터를 주고 받기 위해
  - 상위 -> 하위 : 프롭스,  하위 -> 상위 : 이벤트
```

![image](https://user-images.githubusercontent.com/76584547/135607910-3f9417f7-50f2-401f-83e1-1efbf9e3ed16.png)


#### props
----
+ Root 컴포넌트(상위) message를 app-header 컴포넌트(하위)로 props(데이터)를 내려준다
![image](https://user-images.githubusercontent.com/76584547/135708422-edf3a3b3-f49d-4d2c-b602-4966008d9e73.png)

+ 상위 컴포넌트의 데이터를 바꾸면 하위 컴포넌트의 데이터도 바뀐다 (Reactivity가 일어난다)
![image](https://user-images.githubusercontent.com/76584547/135708552-57f8b401-746e-478d-bf65-52f8c1261a6d.png)

![image](https://user-images.githubusercontent.com/76584547/135708568-a9886482-58e8-473f-aa55-280a4455ae6b.png)


+ app-content 추가 (하위 컴포넌트)
![image](https://user-images.githubusercontent.com/76584547/135708789-a9a93f3d-d99f-414d-8ae4-01788d1f8e43.png)


#### event
---
+ 버튼 클릭 시 상위 컴포넌트로 event를 날리는 emit 생성
![image](https://user-images.githubusercontent.com/76584547/135709252-f1f3a6cd-b888-45ef-b62c-dde1b87a7495.png)

+ event emit을 받아서 출력 - 상위 컴포넌트의 메서드(logText) 지정
![image](https://user-images.githubusercontent.com/76584547/135710161-7a0c4151-2f4d-4fdd-b986-1809880616e0.png)

+ data의 num 증가시키기 (methods에서 this.num으로 접근)
![image](https://user-images.githubusercontent.com/76584547/135710590-fe9ca4de-b657-42cc-af89-96dc5ec42cb1.png)

