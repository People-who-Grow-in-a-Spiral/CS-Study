# Process(프로세스)

> 프로세스는 실행중인 프로그램이다.

<img src = "https://velog.velcdn.com/images/jonghyeok98/post/54cf0ad6-08f5-4140-9e8d-6c8d13a36359/image.png" width="500"/>

<br>

운영 체제에서 프로세스는 두 가지 메모리 공간을 사용합니다.
- 유저 스택(User Stack): 사용자 모드에서 실행되는 코드가 사용하는 스택
- 커널 스택(Kernel Stack): 커널 모드에서 실행되는 코드가 사용하는 스택
- 커널 스택은 시스템 호출(System Call)이나 하드웨어 인터럽트가 발생했을 때, 운영 체제의 커널이 필요한 데이터를 저장하고 작업을 수행할 수 있도록 합니다.
- 각 프로세스마다 고정된 크기로 할당되며 일반적으로 몇 KB정도 입니다. 

## 프로세스의 문맥(context)
 - cpu 수행 상태를 나타내는 하드웨어 문맥으로
 - Program Counter, 각종 register에 대한 정보를 가지고 있습니다.

- 프로세스의 주소 공간
  - 프로세스는 실행 시작 시 프로세스만의 독자적인 주소공간이 할당됨
  - code, data, stack

- 프로세스 관련 커널 자료 구조
  - PCB(Process Control Block)
  - Kernel stack 


### ***프로세스 context는 진행중인 프로세스의 현재 상태를 알기 위해 필요하다***



## 프로세스의 상태

### 프로세스의 상태는 크게 5가지로 정의할 수 있다
- New: 프로세스가 생성중인 상태
- Running: cpu를 할당받아 명령어(instruction)을 수행중인 상태
- Waiting: cpu를 할당해주어도 당장 명령어를 수행할 수 없는 상태   
    - Process 자신이 요청한 event(I/O)가 즉시 만족되지 않아 기다리는 상태   
    - ex) 디스크에서 file을 읽어와야 하는 경우
- Ready: cpu를 할당받길 기다리고 있는 상태(메모리 등 다른 조건을 모두 만족시키는 경우)   
    - 당장 cpu만 할당하면 실행 가능한 경우
- Terminated: 수행(execution)이 끝난 상태



<img src = "https://velog.velcdn.com/images/jonghyeok98/post/d832e6b5-0e56-433a-b820-b87cf7744413/image.png" width = "500"/>



## Process Control Block(PCB)
 - 운영체제가 각 프로세스를 관리하기 위해 유지하고 있는 프로세스 정보   


1. OS가 관리상 사용하는 정보
    - Process State, Process ID
    - scheduling information, priority

2. cpu 수행 관련 하드웨어 값
    - Program Counter, register 

3. 메모리 관련
    - code, stack, data의 위치 정보

4. 파일관련
    - Open file descriptors 등 

<img src = "https://velog.velcdn.com/images/jonghyeok98/post/b9bd7e72-b4b3-482d-8f35-20a006608e13/image.png" width="500"/>  


### ***PCB는 context switch이 이루어졌을 때 기존에 실행하던 process의 정보를 불러오기 위해 필요하다***

* **그렇다면 context switch는 뭘까?**


## Context switch
 - cpu를 한 프로세스에서 다른 프로세스로 넘겨주는 과정
    1. 수행중인 프로세스의 상태를 PCB에 저장
    2. 새롭게 실행할 프로세스의 PCB를 읽어온다
 

<img src="https://velog.velcdn.com/images/jonghyeok98/post/8814208b-f123-4b1f-b718-4e23a8046a6d/image.png" width = "500"/>




### `system call이나 interrupt 발생시 반드시 context switch가 일어나는 것은 아니다`

* system call: 프로세스가 본인이 필요해서 운영체제에게 무언가 요청하는 것
* 위 두 가지 사항 중 하나가 발생하면 cpu에 대한 권한이 사용자 프로세스로부터 운영체제 kernel 로 넘어가게 된다
    - 이것은 context switch가 아님 ! 


> interrupt of system call이 발생하면 kernel mode에서 ISR(interrupt service routine) or system call 함수를 실행한 후 context switch를 할지 user mode로 복귀할지 결정한다.
> * 이 때 user mode로 복귀했다면 context switch는 일어나지 않은 것.
> 하지만 time interrupt가 발생했다면 kerner mode에서 context switch를 발생시킨다.

### ***두 가지 경우 모두 context의 일부를 pcb에 저장해야 하지만 context switch가 일어나는 경우엔 cost가 더 높다***   
* ###  ex) cache memory flush 




## process scheduling 
- 실행 가능한 프로세스 중 다음 실행을 위해 프로세스를 고르는 작업   
- ready queue: main memory에 남아있거나 실행하기 위해 ready 혹은 waiting 상태인 모든 프로세스의 집합   
- wait queue: event 발생으로 인해 기다리고 있는 모든 프로세스의 집합
    - ex) I/O request


<img src = "https://velog.velcdn.com/images/jonghyeok98/post/1fd44d1c-daf9-4145-a08a-7ec0c5017205/image.png" width = "600"/>


## 스케줄러
 - 스케줄러는 운영체제에서 CPU와 같은 시스템 자원을 효율적으로 관리하고 분배하는 역할로 어떤 프로세스가 언제 실행될지 결정하는데 관여합니다.


### Long-term scheduler (장기 스케줄러 or job scheduler)
> * 시작 프로세스 중 어떤 것들을 ready queue로 보낼지 결정
>   - new 상태인 프로세스에 memory를 할당하여 ready queue로 보내는 작업
> * 프로세스에 memory(및 각종 자원)을 주는 문제
> * degree of multiprogramming을 제어 
>   - memory에 올라가 있는 프로세스의 수 -> memory에 프로세스가 너무 많거나 적어도 좋지 않다
> * time sharing system에는 보통 장기 스케줄러가 없음 (무조건 ready)

### short-term scheduler (단기 스케줄러 or CPU scheduler)
> * 어떤 프로세스를 다음번에 running시킬지 결정
> * 프로세스에 cpu를 주는 문제
> * 충분히 빨라야 함(ms 단위)

### medium-term scheduler (중기 스케줄러 or Swapper)
> * 여유 공간 마련을 위해 프로세스를 통째로 메모리->디스크로 쫓아냄
> * 프로세스에게서 memory를 뺏는 문제
> * degree of multiprogramming을 제어 
>   - 대부분 장기 스케줄러가 없고 프로세스가 생성되자마자 memory에 올려 ready queue에 진입하기 때문에
>     memory에 너무 많은 프로세서가 올라가 있는 것을 방지한다

### 스케줄러와 스케줄링 알고리즘

|스케줄러|주요 알고리즘|설명|
|---|---|---|
|장기 스케줄러|FCFS, Priority|어떤 프로세스를 메모리에 올릴지 결정|
|중기 스케줄러|RR, Priority-Based Swapping, LRU|메모리와 디스크 사이에서 스와핑 작업 수행|
|단기 스케줄러|FCFS, SJF, RR, Priority, Multilevel Queue|CPU 할당 순서를 결정하여 효율적 자원 관리|


### FCFS(First-Come, First-Served)
 - 준비 큐(Ready Queue)에 도착한 순서대로 할당
 - 단순하고 공평하지만 특정 작업이 오래 걸리면 다른 프로세스의 대기 시간이 길어질 수 있음음

### LRU(Least Recently Used)
 - 가장 오랫동안 사용되지 않은 프로세스를 Swap Out

### SJF(Shortest Job First)
 - 실행 시간이 가장 짧은 프로세스에게 먼저 할당
 - 평균 대기 시간이 짧지만 실행 시간을 미리 알아야 함

### Round Robin
 - 프로세스에게 `고정된 시간(time Quantum)`을 할당하여 순서대로 실행하거나 고정된 시간 간격으로 스와핑
 - 모든 프로세스에게 공정하며 주로 타임 쉐어링 시스템에 사용

### Priority Scheduling
 - 우선순위가 높은 프로세스에게 먼저 할당
 - 우선순위가 낮은 프로세스는 기아(Starvation)상태에 빠질 수 있음
 - Starvation(기아)는 운영체제에서 특정 프로세스가 자원을 오랜 시간 동안 할당받지 못하는 상태


### Suspended(stopped) 상태
 - Swapper에 의해 새롭게 정의한 process state   
 - 외부적인 이유로 프로세스의 수행이 정지된 상태   
 - 프로세스는 통쨰로 디스크에 swap out 된다   
 - ex) 사용자가 프로그램을 일시 정지시킨 경우 (break key)   
 - 시스템이 여러 이유로 프로세스를 잠시 중단시킴(메모리에 너무 많은 프로세스가 올라와 있을 때)



# 면접 대비 
### 1. 프로세스의 특징을 설명하세요.
> - 프로세스는 각각 독립된 메모리 영역(Code, Data, Stack, Heap의 구조)을 할당받는다.
> - 기본적으로 프로세스당 최소 1개의 스레드(메인 스레드)를 가지고 있다.
> - 각 프로세스는 별도의 주소 공간에서 실행되며, 한 프로세스는 다른 프로세스의 변수나 자료구조에 접근할 수 없다.
> - 한 프로세스가 다른 프로세스의 자원에 접근하려면 프로세스 간의 통신(IPC, inter-process communication)을 사용해야 한다. (Ex. 파이프, 파일, 소켓 등을 이용한 통신 방법 이용)


### 2. 프로세스와 스레드의 차이점은 무엇인가요 ? 
> 프로세스는 운영체제로부터 자원을 할당받는 작업의 단위이고, 스레드는 프로세스가 할당받은 자원을 이용하는 실행의 단위 이다.   
> 프로세스는 운영체제로부터 메모리, 주소 공간 등을 할당받고 쓰레드는 할당받은 자원들을 내부 스레드끼리 공유하면서 실행된다. 

| **구분**              | **프로세스(Process)**                                          | **스레드(Thread)**                                         |
|-----------------------|-----------------------------------------------------------------|------------------------------------------------------------|
| **정의**              | 프로그램 실행의 **독립적인 단위**                               | 프로세스 내에서 작업을 실행하는 **실행 단위**              |
| **메모리 구조**       | 독립적인 메모리 공간(코드, 데이터, 스택, 힙)                    | 코드, 데이터, 힙을 **공유**, 스택과 레지스터는 개별적      |
| **자원 할당**         | 운영체제로부터 **독립적으로 자원 할당**                          | 프로세스 내 자원을 **공유**                                 |
| **생성/관리 비용**    | 생성 및 관리 비용이 **높음**                                     | 생성 및 관리 비용이 **낮음**                                |
| **문맥 교환 비용**    | 문맥 교환 시 오버헤드가 **크다**                                | 문맥 교환 시 오버헤드가 **적다**                            |
| **독립성**            | 각 프로세스는 **독립적**으로 실행, 다른 프로세스에 영향 없음     | 한 스레드의 오류가 **프로세스 전체에 영향**을 줄 수 있음   |
| **병렬 처리**         | 병렬 처리 효율이 상대적으로 낮음                                 | **병렬 처리에 유리**, 빠른 작업 수행 가능                  |
| **예시**              | 웹 브라우저, 게임, 텍스트 에디터 등 독립 실행 프로그램          | 웹 브라우저의 탭, 다운로드와 UI 갱신 등                    |
| **장점**              | - 독립적 실행으로 **안정성** 높음                               | - 자원 공유로 **효율적**<br>- 병렬 작업 처리에 유리        |
| **단점**              | - **자원 소모**가 큼<br>- 생성 및 관리 비용이 높음              | - 한 스레드 오류가 **전체 프로세스에 영향**               |


### 3. 멀티 프로세스 대신 멀티 스레드를 사용하는 이유는 무엇인가요?
 - 쉽게 설명하면, 프로그램을 여러 개 키는 것보다 하나의 프로그램 안에서 여러 작업을 해결하는 것이다.

#### 자원의 효율성 증대
 - 멀티 프로세스로 실행되는 작업을 멀티 스레드로 실행할 경우, 프로세스를 생성하여 자원을 할당하는 시스템 콜이 줄어들어 자원을 효율적으로 관리할 수 있다. (프로세스 간의 Context Switching시 단순히 CPU 레지스터 교체 뿐만 아니라 RAM과 CPU 사이의 캐시 메모리에 대한 데이터까지 초기화되므로 오버헤드가 크기 때문)
 - 스레드는 프로세스 내의 메모리를 공유하기 때문에 독립적인 프로세스와 달리 스레드 간 데이터를 주고받는 것이 간단해지고 시스템 자원 소모가 줄어들게 된다.

#### 처리 비용 감소 및 응답 시간 단축
 - 프로세스 간의 통신(IPC)보다 스레드 간의 통신의 비용이 적으므로 작업들 간의 통신의 부담이 줄어든다. (스레드는 Stack 영역을 제외한 모든 메모리를 공유하기 때문)
 - 프로세스 간의 전환 속도보다 스레드 간의 전환 속도가 빠르다. (Context Switching시 스레드는 Stack 영역만 처리하기 때문)

<img src="https://velog.velcdn.com/images/jonghyeok98/post/d74f041f-f5a9-43a2-8b9b-616931aaf06f/image.png" width = "600"/>

### 4. 멀티 프로세싱과 멀티 프로그래밍의 차이점은 무엇인가요?
 - 멀티 프로세싱은 여러개의 처리장치(CPU)를 장착하여 동시에 여러 작업을 병렬로 실행하는 방법.   
 - 멀티 프로그래밍은 다수 개의 프로그램의 같이 주기억장치에 있도록 한 방식.

### 5. 힙 영역과 스택 영역의 차이점은 무엇인가요? 
 - 프로그램이 실행되기 위해 프로그램이 메모리에 로드가 되어야한다.   
 - 따라서 운영체제에서 프로그램의 실행을 위해 다향한 메모리 공간을 제공한다.   
 - 코드, 데이터, 스택, 힙 영역이 할당되고 각 역할은 다음과 같습니다.


#### 코드
 - 실행할 프로그램의 코드가 저장되는 텍스트 영역이다. CPU는 코드영역에서 저장된 명령어를 하나씩 가져가서 처리한다.
#### 데이터
 - 전역변수와 정적변수가 이해 해당된다. 프로그램의 시작과 함께 할당되며 프로그램이 종료되면 소멸된다.
#### 스택
 - 스택영역은 함수의 호출과 관계되는 지역변수와 매개변수가 저장되는 영역이다.   
 - 함수의 호출과 함께 할당되며, 함수의 호출이 종료될때 해제된다.
#### 힙
 - 힙 영역은 사용자가 직접 관리할 수 있는 메모리 영역이다. 힙 영역은 사용자에 의해 메모리공간이 동적으로 할당되고 해제된다.


<img src="https://velog.velcdn.com/images/jonghyeok98/post/c2b7a4a1-4548-4f3a-ad82-162cbbd32780/image.png" width = "500"/>



참고자료)
- KOCW 공개강의 (2014-1. 이화여자대학교 - 반효경)