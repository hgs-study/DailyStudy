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
