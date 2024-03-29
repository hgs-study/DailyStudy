## 일정관리
```
ㅇ 과거 일정들을 보면서 내가 언제 무슨 공부를 했고 시간을 효율적으로 분배해서 자기개발을 하고 있는지 확인할 수 있을 것 같다.
ㅇ 진행한 이슈들을 체크/관리하고 명확하게 나 자신을 피드백 할 수 있는 일정관리 모델을 지향한다.

※ 약어 사용
  - KA(Klaytn-API) : POC 프로젝트 (https://github.com/hgs-study/Klaytn-API)
  - KSS(Klay-Staking-Service) : 메인 프로젝트 (https://github.com/hgs-study/Klay-Staking-Service)
```
### 일정
  #### 2021.04 (04.12 ~ )
  ----
  + 2021-04-12 : [KA](https://github.com/hgs-study/Klaytn-API) - POC 프로젝트 API 실습 (Token / Wallet)
  + 2021-04-13 : [KA](https://github.com/hgs-study/Klaytn-API) - POC 프로젝트 API 실습 (Transaction)
  + 2021-04-14 : [KA](https://github.com/hgs-study/Klaytn-API) - POC 프로젝트 API 실습 (Node API) / 사내 스터디( 코딩&공부 중 이슈 토론 / Rest API 공유 / 토이 프로젝트 기획)
  + 2021-04-15 : [KSS](https://github.com/hgs-study/Klay-Staking-Service) - POC 프로젝트 API 용어 & 개념 학습 / Java Reflection Debug 즉흥 스터디
  + 2021-04-16 : [KSS](https://github.com/hgs-study/Klay-Staking-Service) - 토이 프로젝트(ERD 설계)
  + 2021-04-17 : [KSS](https://github.com/hgs-study/Klay-Staking-Service) - 요구사항 & 개발환경 & ERD 세팅 / 로그인,회원가입 화면 개발 완료 (부트스트랩)
  + 2021-04-18 : [KSS](https://github.com/hgs-study/Klay-Staking-Service) - 로그인, 회원가입 기능 구현
  + 2021-04-19 : [KSS](https://github.com/hgs-study/Klay-Staking-Service) - 로그인 예외처리 / 이슈: 시큐리티 적용 후 인덱스 페이지 2번 로드되는 상황
  + 2021-04-20 : [KSS](https://github.com/hgs-study/Klay-Staking-Service) - 로그인 예외처리 / BaseEntity 추가, Test 코드 작성
  + 2021-04-21 : [KSS](https://github.com/hgs-study/Klay-Staking-Service) - 도메인별 엔티티 / 연관관계 설정
  + 2021-04-22 : [KSS](https://github.com/hgs-study/Klay-Staking-Service) - 지갑 생성 API / Test 코드 작성
  + 2021-04-23 : [KSS](https://github.com/hgs-study/Klay-Staking-Service) - Wallet <-> Token 양방향 연관관계 / Test 코드 작성
  + 2021-04-24 : [KSS](https://github.com/hgs-study/Klay-Staking-Service) - 이슈 : Wallet <-> Token 다대다 연관관계로 변경 / 해결 : TokenAmount(토큰수량) 중간 연결 엔티티 생성
  + 2021-04-25 : [KSS](https://github.com/hgs-study/Klay-Staking-Service) - 회원가입시 0.1 Klay 지급 구현 (Test 코드 작성) / API,Transaction 발생시 History 내역에 저장 (Transaction일 경우, TX 정보 Transaction History에 추가 저장) 
  + 2021-04-26 : [KSS](https://github.com/hgs-study/Klay-Staking-Service) - 회원가입 예외 처리 추가 / 내 Klay 수량 조회 기능 구현
  + 2021-04-27 : [KSS](https://github.com/hgs-study/Klay-Staking-Service) - 회원 지갑 주소 조회 / TODO List 
  + 2021-04-28 : [KSS](https://github.com/hgs-study/Klay-Staking-Service) - Account 등록, 삭제, 조회, 수정 완료
  + 2021-04-29 : [KSS](https://github.com/hgs-study/Klay-Staking-Service) - Staking 상품 등록 완료 / ERD 수정

  #### 2021.05
  ----
  + 2021-05-02 : [KSS](https://github.com/hgs-study/Klay-Staking-Service) - Staking 수정, 삭제, 조회 완료 / 컨트롤러 <-> 서비스 ResponseEntity 책임 변경 / Order 등록, 조회 / OrderedProduct 등록 조회 완료
  + 2021-05-03 : [Batch & Scheduler](https://github.com/hgs-study/Batch-Scheduler-Basic) - [KSS Pilot프로젝트] Scheduler 구현 / Batch Job 실행 (h2 -> mysql)
  + 2021-05-04 : [Batch & Scheduler](https://github.com/hgs-study/Batch-Scheduler-Basic) - [KSS Pilot프로젝트] job Parameter 설정 / Batch 메타 데이터 테이블 이해
  + 2021-05-05 : [Batch & Scheduler](https://github.com/hgs-study/Batch-Scheduler-Basic) - [KSS Pilot프로젝트] 배치 성공, 실패 시나리오 / Decide / JpaItemReader
  + 2021-05-06 : [Batch & Scheduler](https://github.com/hgs-study/Batch-Scheduler-Basic) - [KSS Pilot프로젝트] ItemProcessor , ItemWriter
  + 2021-05-08 : [Batch & Scheduler](https://github.com/hgs-study/Batch-Scheduler-Basic) - [KSS Pilot프로젝트] 스케줄러 & 배치를 활용한 1분마다 사용자 Klay Balance 확인 후 업데이트
  + 2021-05-09 : [KSS](https://github.com/hgs-study/Klay-Staking-Service) - Batch & Scheduler 초기 세팅
  + 2021-05-10 : [KSS](https://github.com/hgs-study/Klay-Staking-Service) - 00:00시마다 0.01 Klay 스테이킹 보상 지급 완료

  #### 2021.07
  ----
  + 2021-07-04 : [KSS](https://github.com/hgs-study/Klay-Staking-Service) - [Jwt] Spring Security + Jwt(Access/Refresh) + Redis / 각 도메인 컨트롤러, 서비스 역할 부분 변경
  + 2021-07-05 : [KSS](https://github.com/hgs-study/Klay-Staking-Service) - [Jwt] Spring Security 예외처리 추가#1
  + 2021-07-11 : [KSS](https://github.com/hgs-study/Klay-Staking-Service) - [Swagger] Swagger 추가
