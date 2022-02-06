## 1% 네트워크 원리
---

### 네트워크의 전체 모습
```
  브라우저 : XXX라는 페이지의 데이터를 주십시오.
  웹서버 : 예 알겠습니다. 이것이 그 데이터입니다.
```

```
  브라우저 : 이 주문 데이터를 처리해주세요
  웹 서버 : 예 주문 데이터를 받았습니다.
```

```
  브라우저 요청 <-> 웹서버 응답
```
![image](https://user-images.githubusercontent.com/76584547/152686731-d208c072-9dd4-4a69-9810-44c66de1dcf3.png)

+ 패킷이 거치는 경로
![image](https://user-images.githubusercontent.com/76584547/152688673-b8bfaf39-6b88-4cf3-a5f8-9ed457810373.png)
![image](https://user-images.githubusercontent.com/76584547/152688690-8e13675a-eec9-4ccb-9837-9322367915bf.png)

+ 네트워크는 어떻게 접속되는가?
![image](https://user-images.githubusercontent.com/76584547/152688701-026d9cca-a7e0-4b8b-ad25-3ab2a6a48efc.png)



#### 기타 
----
+ 네트워크 도중 리퀘스트나 응답이 삭제되거나 파괴될 수 있는 사태를 고려해야한다.
```
  - 리퀘스트나 응답의 실체는 전기나 빛의 신호이기 때문에, 그 신호가 잡음 등의 영햘을 받아 파괴될 수 있다.
  - 리퀘스트나 응답은 모두 0과 1로 이루어진 디지털 데이터이므로 데이터를 목적지까지 운반하는 구조
```
