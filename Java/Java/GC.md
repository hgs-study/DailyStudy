## GC
----

### GC 과정
----
2. 새로운 객체는 Eden(에덴) 영역에 할당된다.
![image](https://user-images.githubusercontent.com/76584547/133103065-8cf4d311-bc60-4e70-b990-dc863459f0cc.png)

3. Eden 영역이 꽉 찰 경우 GC(Minor) 발생 - Mark and Sweep
![image](https://user-images.githubusercontent.com/76584547/133103317-b8ad6f40-771e-41f4-ae50-889481532bd2.png)

4. Eden에 있던 Reachable 객체는 Survival 0(서바이벌 제로)으로 옮겨진다.
  + Eden 영역의 Unreachable 객체는 메모리에서 해제
![image](https://user-images.githubusercontent.com/76584547/133103548-8065f633-7546-4693-b6b1-b0d72d385294.png)

  + 이 과정이 반복돼서 Eden 영역이 또 꽉 차면 Survival 0로 추가적으로 옮겨진다.
  ![image](https://user-images.githubusercontent.com/76584547/133103825-b2d39e06-7561-4a1e-a721-b9c0448a29e2.png)

5. Survival 0이 다 차면 Mark and Sweep이 일어나고 Reachable 객체는 Survival 1으로 옮겨진다.
  + Survival 1으로 옮겨진 객체는 Age 값이 증가한다. 
![image](https://user-images.githubusercontent.com/76584547/133103987-991610d3-68eb-4d80-bd1f-eb9cdaf26b62.png)


 6. Eden 영역이 다시 차서 Survival 1로 옮겨진다.
  + 왜냐하면, 현재 이미 객체가 차있는 Survival 영역으로 옮겨지기 때문이다.
  + 이렇기 때문에 Survival 0 과 Survival 1 중 하나는 항상 비어있는 상태로 유지한다.
  + Survival 1이 다 차면 0으로 옮겨지고 Age 값이 증가한다.
  ![image](https://user-images.githubusercontent.com/76584547/133104367-ab39734a-6fc6-4ba8-b860-bcfd66e1695e.png)

 7. Age 값이 특정 값 이상이 되면 Old Generation 영역으로 옮겨진다.
  + 이 과정을 "Promotion"이라고 한다.(New Generation에서 Old Generation으로 승진하는것)
  ![image](https://user-images.githubusercontent.com/76584547/133104992-7365bb0f-047b-46f3-ad4b-43456e66ec00.png)

8. Old Generation이 꽉 차면 GC 발생(Major)
![image](https://user-images.githubusercontent.com/76584547/133105237-4a083abb-fcd2-4a3c-8243-99754c79f178.png)

=> 위 과정들을 반복하면서 GC가 메모리를 관리한다.

#### 참고
---
```
  https://www.youtube.com/watch?v=vZRmCbl871I
```
