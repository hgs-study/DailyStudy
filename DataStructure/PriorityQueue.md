## PriorityQueue
---
```
  일반적인 큐는 제일 먼저 들어간 데이터가 가장 먼저 나오게 되는 자료구조이다. 
  이런 큐의 특성과 달리 우선순위 큐는
  "들어간 순서에 상관없이 일정한 규칙에 따라 우선순위를 선정하고, 우선순위가 가장 높은 데이터가 가장 먼저 나오게 된다."
```
+ 문제
```

매운 것을 좋아하는 Leo는 모든 음식의 스코빌 지수를 K 이상으로 만들고 싶습니다.
모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 Leo는 스코빌 지수가 가장 낮은 두 개의 음식을 아래와 같이 특별한 방법으로 섞어 새로운 음식을 만듭니다.

섞은 음식의 스코빌 지수 = 가장 맵지 않은 음식의 스코빌 지수 + (두 번째로 맵지 않은 음식의 스코빌 지수 * 2)

Leo는 모든 음식의 스코빌 지수가 K 이상이 될 때까지 반복하여 섞습니다.
Leo가 가진 음식의 스코빌 지수를 담은 배열 scoville과 원하는 스코빌 지수 K가 주어질 때,
모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 섞어야 하는 최소 횟수를 return 하도록 solution 함수를 작성해주세요.
```


```java
    public int solution(int[] scoville, int K) {
        PriorityQueue<Integer> queue=new PriorityQueue<>(); //(우선순위가 낮은 숫자 순)

        for(int num:scoville){
            queue.add(num);
        }

        int count=0;

        while(queue.peek()<K && !queue.isEmpty()) {//peek()은 꺼내지 않고 값만 확인
            int lessSpicy=queue.poll();//poll()은 우선순위가 젤 우선인 값을 꺼냄

            if(!queue.isEmpty()){
                int secondLessSpicy=queue.poll();
                queue.add( lessSpicy + secondLessSpicy * 2);
                count++;
            }else{
                return -1;
            }
        }

        return count;
    }
```

+ 참고 : 
  + https://welcomto-dd.tistory.com/50
  + https://coding-factory.tistory.com/603
