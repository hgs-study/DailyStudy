# 프로세스

### 생명주기
    - New
        - 프로세스가 생성된 상태
    - Running
        - Cpu를 점유해서 프로세스의 명령어를 실행시키는 상태
    - Waiting
        - 한 프로세스가 점유하고 있으면 두번째 프로세스는 waiting 상태
        - i/o를 했으면 i/o가 끝날때까지 대기하는 상태 등
    - Ready
        - i/o를 대기하다가 cpu를 바로 점유하는 것이 아니라 ready 큐에 있는 대기상태
    - Terminated
        - 프로세스를 종료하는 상태
        
    ![image](https://user-images.githubusercontent.com/76584547/196720347-7201f7c2-fddc-4c70-aa2c-1a4c002afc75.png)
   
---

### PCB (process control block)
- 프로세스가 가져야하는 모든 정보를 저장하자
- 프로세스 상태
    - new, running ,waiting, ready, terminated
- 프로그램 카운터
    - 프로그램 카운터에 있는 메모리 주소를 가지고 메모리에서 해당 메모리 주소에서 명령어를 fetch해와서 cpu에서 실행
        
![image](https://user-images.githubusercontent.com/76584547/196720310-a9dd6939-d8a6-4a50-a669-fe967020403e.png)
        
- Cpu Register
    - Instruction Register (IR)
    - PCB 등등
- CPU-scheduling information
- memory management information

---

### Context Switch
- PCB 정보
- 어떤 인터럽트가 일어났을 경우, 현재 문맥을 저장해놓고 (PC : 프로그램 카운터(어디까지 실행했는지) , 해당 프로세스는 Ready큐로 감) 다시 CPU를 획득했을 때, 저장해놓은 문맥(컨택스트 사용)
- 현재 프로세스 문맥을 저장하고 새로 프로세스의 문맥을 보관
- 컨텍스트 스위치 프로세스
    1. P0 실행
    2. P1의 인터럽트나 시스템 콜
    3. P0은 현재 상태를 PCB에 저장
    4. P1의 상태를 PCB에 보관
    5. P1 실행
    6. P0의 인터럽트나 시스템 콜
    7. P1의 현재 상태 PCB에 저장
    8. P0의 현재 상태 PCB에 보관
    9. P0 실행
     
 ![image](https://user-images.githubusercontent.com/76584547/196720225-4da89f9d-6782-4f7a-a93a-82ec5b51f40d.png)

---

### CPU 스케줄링

![image](https://user-images.githubusercontent.com/76584547/201706368-89742077-b1ba-4b21-9379-ce086a599f42.png)

- 스케줄링
    - **Ready 큐**에 있는 프로세스에 Cpu를 할당해줄 수 있는 프로세스를 선택하는 것
    - 멀티 프로세스 환경에서 메모리에 여러 프로세스를 띄우고 Cpu를 시분할해서 사용
- Preemptive vs Non-preemptive
    - Preemptive
        - 어떤 사유로 인해 다른 프로세스가 CPU를 선점 가능
    - Non-preemptive
        - 해당 프로세스가 CPU를 점유하고 나오기 전까지는 계속 해당 프로세스가 CPU를 점유
- Dispatcher
    - 현재 선점하고 있는 프로세스에 인터럽트 or 시스템콜이 들어올 경우 해당 프로세스를 Ready 큐나 Wating 큐로 넘기는 것을 Context Switch 라고 하며, 이 컨텍스트 스위치를 해주는 모듈을 **Dispatcher** 라고한다
    - 역할
        - 컨텍스트 스위치
        - 프로세스를 유저모드로 변경해준다.
        - 새로운 프로세스를 적당한 위치에 resume 시키는 것
        - 어떤 프로세스를 변경하는지 선택하는 것은 **Scheduler**
        - 실제로 컨텍스트 스위치를 해주는 것은 **Dispathcher**

- 스케줄링 알고리즘
    - FCFS (First-Come, First-Served)
        - 먼저 온 프로세스부터 처리
    - SJF(Shortest Job First)
        - 남은 시간이 가장 짧은 프로세스부터 처리
        - CPU burst time을 미리 알 수 없다 - CPU사용시간을 미리 알 수 없지만 추정은 가능하다. 과거의 CPU사용한 흔적을 통해 해당 프로세스의 CPU burst time을 예측한다.
    - RR(Round-Robin) : **시분할**을해서 정해진 시간만큼만 사용 **(현대적 컴퓨터 운영체제에서 사용)**
        - Preemptive FCFS
        - 타임 슬라이스 : 시간 분할 (보통 10 to 100 m/s 주기)
        - Ready 큐를 **circular queue**로 구현하면됨
        - A프로세스에게 1초를 부여했지만 1초보다 더 적은시간(0.5초)에 끝났을 경우 0.5초에 A프로세스에서 나옴
        
        ### RR 순서
        
        1. 반대로 1초보다 더 긴 시간 작업을 할 경우 **타이머가 인터럽트를 건다** 
        2. 컨텍스트 스위칭 일어난다
        3. 해당 프로세스는 Ready 큐 끝에 들어가서 대기
    - Priority-based
        - 우선순위
        - 우선 순위 높은 프로세스부터 cpu에 할당
        - SJF가 이미 Priority-based 스케줄링
    - MLQ : Multi-Level Queue
        - 여러개의 우선 순위 높은 순으로 Ready 큐를 가지고 있는 것
        - ex) 핸드폰은 전화가 가장 높은 우선순위, 그다음 어플 등등
    - MLFQ (Multi-Level Feedback Queue)
        - 멀티 레벨 큐에 피드백을 더함 **(현대적인 스케줄러)**
        - MLQ 에서 가장 우선순위 높을 경우, 가장 낮은 우선순위는 실행 못할 수도 있음
        - quantum 을 주기 때문에 우선 순위 높은 큐에서 머무르지 않고 짧은 할당량으로 다른 큐들도 cpu를 사용할 수 있다.
        - CPU-bound 가 많은 프로세스는 quantum을 늘리는 식으로 현대적인 스케줄러이다.
        
    ![image](https://user-images.githubusercontent.com/76584547/202231670-cc92fd01-801d-48f9-8e8c-b6d6b6ceafa7.png)

- Thread Scheduling
    - 사실 현대적 컴퓨터에서 프로세스 스케줄링을 하지 않음
    - 왜? 쓰레드를 지원해서 쓰레드 스케줄링을 하기 때문에
    - 종류
        - 유저 쓰레드
        - 커널 쓰레드가 존재
    - 하지만 커널 쓰레드만 스케줄링하면 됨
        - 이유? 유저 쓰레드는 라이브러리로만 관리하기 때문에 커널 쓰레드는 유저 쓰레드가 어떤게 도는 지 모른다
        - 커널쓰레드를 유저쓰레드랑 매핑만 시켜주는 역할만 하면 된다.
        - 커널쓰레드를 유저쓰레드랑 매핑하고 서비스만 실행시켜주면 되기 때문
    - OS커널은 CPU 스케줄링을 **커널쓰레드로 스케줄링**한다

- Real-Time CPU Scheduling
    - 실시간 OS - 윈(x), 리눅스 (x)
    - 주어진 시간 안에 테스크를 완료 할 수 있는 것
    - Soft real-time : 크리티컬한 프로세스가 무조건 실행되지 않지만 다른 일반 프로세스보단 먼저 실행
    - Hard real-time
        - 어떤 테스크가 반드시 데드라인 안에 실행되는 것
        - 반드시 우선순위가 필요
