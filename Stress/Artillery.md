### Artillery
----

## 설치
---
  1. npm install -g artillery (visual code)
  2. test.yaml 생성 후 
  ```
  config:
  target: "https://example.com/api"
  phases:
    - duration: 60
      arrivalRate: 5
      name: Warm up
    - duration: 120
      arrivalRate: 5
      rampTo: 50
      name: Ramp up load
    - duration: 600
      arrivalRate: 50
      name: Sustained load
  payload:
    path: "keywords.csv"
    fields:
      - "keyword"

scenarios:
  - name: "Search and buy"
    flow:
      - post:
          url: "/search"
          json:
            kw: "{{ keyword }}"
          capture:
            - json: "$.results[0].id"
              as: "productId"
      - get:
          url: "/product/{{ productId }}/details"
      - think: 5
      - post:
          url: "/cart"
          json:
            productId: "{{ productId }}"
```
  3. 원하는 측정 api 설정
  ```
  config:
  target: "http://측정 아이피"
  phases:
    - duration: 60
      arrivalRate: 1
      name: Warm up
scenarios:
  - name: "just get hash"
    flow:
      - get:
          url: "/hash/123"
  #내 아이피 get 요청으로 "/hash/123"이라는 api를 호출한다.
  #60초 동안 성능을 측정하고 매초 5명의 새로운 visual user를 만든다.
  
  ```
  4. 아틸러리 실행 
    + artillery.cmd run --output report.json .\test.yaml
  5. 테스트 완료 되면 report.json이 생성됨 
  6. 만들어진 report.json -> html로 만드려면
    +  artillery.cmd report .\report.json 
    +  실행하면 성능 측정 html이 뜸
  7. 아틸러리 테스트 중 가장 중요한 목록 "Latency At Intervals" (가로축 - 시간, 세로축 - 지연시간(latency))
    + arrivalRate 1일 경우 (매초 1명)
  ![image](https://user-images.githubusercontent.com/76584547/117819977-106f2700-b2a5-11eb-8561-c140520db425.png)
    + arrivalRate 8일 경우 (매초 8명) - latency가 살짝 튀긴 하나 그래도 안정적인 모습을 보인다
  ![image](https://user-images.githubusercontent.com/76584547/117820531-97bc9a80-b2a5-11eb-917f-74ef90ebac3c.png)
    + arrivalRate 15일경우 (매초 15명) - latency가 불안정하며 계속 진행되거나 visual user가 조금만 늘었어도 에러가 날 것이다.
  ![image](https://user-images.githubusercontent.com/76584547/117821372-790ad380-b2a6-11eb-9b6d-969085a8a86d.png)

  ※ PM(물리서버)과 달리 VM(가상서버)은 인접하고 있는 인스턴스들과 자원을 공유하기 때문에 항상 똑같은 수준의 퀄리티를 보장할 수 없다 => 레이턴시가 중간 중간 튄다.

