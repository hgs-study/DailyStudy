## Vue

### 라우터
---
+ 설치
```
  npm i vue-router --save
```

+ [router] - [index.js]

![image](https://user-images.githubusercontent.com/76584547/138820717-5e1208e9-3e0e-43d4-a913-8a3af71456a1.png)
```vue
import Vue from 'vue';
import VueRouter from 'vue-router';
import NewsView from '../views/NewsView.vue'
import AskView from '../views/AskView.vue'
import JobsView from '../views/JobsView.vue'

Vue.use(VueRouter);

export const router = new VueRouter({
  routes : [
    {
        path : '/news', // url 주소 
        component : NewsView, // url주소로 갔을 때 표시될 컴포넌트
    },
    {
        path : '/ask',
        component : AskView,
    },
    {
        path : '/jobs',
        component : JobsView,
    },
  ]
});
```


+ [main.js] : 라우터 등록

![image](https://user-images.githubusercontent.com/76584547/138819830-df22e018-6331-4bd4-9bb8-958b89741ad6.png)


### this의 4가지와 애로우 펑션의 this
---
+ 첫번째 this : window (전역객체)

![image](https://user-images.githubusercontent.com/76584547/138842978-9de25e26-f210-48d6-b0e2-e0ee8a59a157.png)

+ 두번쨰 this : 함수 안에서도 this는 전역객체를 가리킴

![image](https://user-images.githubusercontent.com/76584547/138843357-8e39d261-d75c-494f-aaf9-43b6aa10469e.png)

+ 세번째 this : 생성자 객체를 가리킴

![image](https://user-images.githubusercontent.com/76584547/138843835-46aea1b1-7110-49b9-963b-43fadd20537f.png)

+ 네번째 this : 비동기 처리에서의 this
  + created()안에서 비동기 호출 전 this : Vue 객체를 가리킴
  + 비동기 호출 후 this : undefined  
  + 화살표 함수일 경우 : Vue 객체를 가리킴 (호출되는 함수를 가져옴 , 바인딩을 안 해줘도됨 ex) var vm = this;)
