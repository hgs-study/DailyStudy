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

### CPU 스케줄링
- 스케줄링
    - **Ready 큐**에 있는 프로세스에 Cpu를 할당해줄 수 있는 프로세스를 선택하는 것
        - FIFO
        - Priority Queue
    - 멀티 프로세스 환경에서 메모리에 여러 프로세스를 띄우고 Cpu를 시분할해서 사용
![image](https://user-images.githubusercontent.com/76584547/201706368-89742077-b1ba-4b21-9379-ce086a599f42.png)

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
