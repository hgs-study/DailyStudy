## 트랜잭션

#### @Transactional 동작방식

<img width="1019" alt="image" src="https://user-images.githubusercontent.com/76584547/174469271-c47924cf-add8-45f2-b188-e34d4a76859f.png">

- AOP를 사용해서 템플릿 콜백패턴으로 프록시를 생성
- 트랜잭션 가져옴 (플랫폼 트랜잭션 매니저로 각 db 밴더사 트랜잭션 매니저 생성)
- 커넥션 생성, 오토커밋 false
- 트랜잭션 동기화 매니저에서 쓰레드 로컬을 사용해서 각 트랜잭션 동시성 보장
- 결과를 트랜잭션 매니저로 가져와서 커밋 or 롤백
