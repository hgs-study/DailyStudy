## Vue

### Style
---
+ scoped : 해당 컴포넌트에서만 style 적용

![image](https://user-images.githubusercontent.com/76584547/138550230-f230fc39-549c-4e54-9fe4-97845d29403b.png)


### v-for
---
+ item, index 활용

![image](https://user-images.githubusercontent.com/76584547/138552261-8a6ec697-408f-41ec-b280-7ffc66787f45.png)


### splice
---
+ 배열에서 해당 인덱스, 1개를 지우고 반환

![image](https://user-images.githubusercontent.com/76584547/138552744-401a2192-6471-4038-ab0d-0a6ff2e4aadb.png)

### slot
---
+ modal component에 header, body, footer slot이 나눠져있다.

![image](https://user-images.githubusercontent.com/76584547/138582157-f8ac0d27-08d5-467c-8793-1a3e85ed4101.png)


+ 하지만 이를 상위 컴토넌트에서 재정의 할 수 있다 (header slot)

![image](https://user-images.githubusercontent.com/76584547/138582171-5080bde1-6bf0-4e23-9872-505ba9eb9594.png)


+ 결과

![image](https://user-images.githubusercontent.com/76584547/138582176-fef24021-5443-41e6-b62b-f0e61f20fc29.png)


### Vuex
---
```
  상태 관리 라이브러리
  - 복잡한 애플리케이션의 컴포넌트를 효율적으로 관리하는 라이브러리
  - 무수히 많은 컴포넌트의 데이터를 관리하기 위한 상태 관리 패턴이자 라이브러리
  - React Flux 패턴에서 기인함
```

+ 개요
  + Flux패턴 
  + state (data), getters (computed), mutations (method), actions (비동기 메소드) 학습
  + Helper 소개
  + 프로젝트 구조화 및 모듈 구조화 

+ Flux 패턴 (데이터가 한방향)
```
  MVC 패턴의 복잡한 데이터 흐름 문제를 해결하는 개발 패턴
  - MVC 패턴 문제점 : 수많은 model이 수많은 view를 갱신하기 때문에 데이터의 흐름을 추적할 수 없다.
```
1. action : 화면에서 발생하는 이벤트 또는 사용자의 입력
2. dispatcher : 데이터를 변경하는 방법, 메서드
3. model : 화면에 표시할 데이터
4. view : 사용자에게 비춰지는 화면
