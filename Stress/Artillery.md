### Artillery
----

## 설치
---
  + 1. npm install -g artillery (visual code)
  + 2. test.yaml 생성 후 
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
  + 3. 원하는 측정 api 설정
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
