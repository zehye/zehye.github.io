---
layout: post
title: CPU Scheduling - FCFS, SJF, Priority, RR, 다중 큐 스케줄링
category: OS
tags: [OS, operating system]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
경성대학교 양희재 교수님 수업 영상을 듣고 정리하였습니다.     

<hr>

## CPU Scheduling

Ready Queue에서 프로세스들이 CPU의 서비스를 받기 위해 기다리는데, 현재 CPU에서 하고 있는 작업이 끝나게되면 어느 프로세스를 가져올지 결정하는 것


- Preemptive vs Non-Preemptive (선점 vs 비선점)
  - Preemptive: CPU가 어떤 프로세스를 막 실행하고 있는데, 아직 끝나지도 않았고 IO를 만나지도 않았음에도 강제로 쫓아내고 새로운게 들어갈 수 있게하는 스케줄링 방식
  - Non-Preemptive: 이미 한 프로세스가 진행중이면 절대 스케줄링이 일어나지 않는 것

- Scheduling criteria(스케줄링 척도)
  - CPU Utilization(CPU 이용률, %): CPU가 얼마나 놀지 않고 일하는가?
  - Throughput(처리율, Jobs/ms): 시간당 몇개의 작업을 처리하는가?
  - Turnaround time(반환시간, m/s): 작업이 들어온 시간부터 끝날때까지의 시간
  - Waiting time(대기시간, m/s): cpu 서비스를 받기 위해 얼마나 기다렸는가?
  - Response time(응답시간): 처음 응답이 나올때까지의 시간, interactive computer에서 중요


## CPU Scheduling Algorithm

- First-Come, First-Served(FCFS): 먼저 온놈을 먼저 작업한다.
- Shortest-Job_First(SJF): 작업시간이 짧은 놈을 먼저 작업한다.
  - Shortest Remaining-Time-First
- Priority: 우선순위가 높은 놈을 먼저 작업한다.
- Round-Robin(RR): 빙빙 돌면서 순서대로 작업한다.
- Multilevel Queue : Queue를 여러개 둔다.
- Multilevel Feedback Queue


### First-Come, First-Served(FCFS)

- simple & fair: 먼저 온 놈을 먼저 작업한다. 제일 간단하고 공평한 방법이다. 그러나 정말 좋은 방법인가?
  - 꼭 좋은 성능을 나타내는 것은 아니다.

- example: Find Average Waiting Time
  AWT = (0+24+27)/3 = 17 msec  cf.3 msec!

3개의 프로세스가 거의 동시에 Ready Queue에 도착했다고 해보자.

- Gantt Chart
Process | Burst Time(CPU 이용시간)
P1 | 24
P2 | 3
P3 | 3

<center>
<figure>
<img src="/assets/post-img/OS/19.jpeg" alt="" width="50%">
</figure>
</center>

- `Convoy Effect(호위효과)`: cpu 실행시간을 많이 쓰는애가 딱 앞에 있으면 나머지 애들이 뒤를 따라다니며 기다리고 있는 모습-> FCFS 의 단점
- Non-Preemptive Scheduling: p1이 끝날때까지 나머지는 못들어감.


### Shortest-Job_First(SJF)

실행시간이 가장 짧은 작업을 가장 먼저 실행함으로써 짧게 끝나는 프로세스를 앞장세워야 전체 대기시간이 줄어들게 함

- Example : QWT = (3+16+9+0)/4 = 7msec
  - ch. 10.25msec(FCFS)

- `Provably optimal!`
: 대기시간을 줄이는 측면에서는 가장 좋은 방법!

- Not realistic, prediction may be needed
: 비현실적이다. 실제로 이 프로세스가 얼마를 사용할지를 우리는 알수가 없다. 실제 돌려보기 전까지는 전혀 알수가 없다. 그래서 가장 이상적이지만 비현실적이다. 실제로 사용하기에는 예측을 할 수 밖에 없다. 예측하기 위해서는 os가 그동안 cpu를 사용했던 시간들을 다 정리해놓고 예측을 하는 것이긴 이 예측을 하기 위해서는 overhead가 많은것이고 그 만큼의 계산도 많이 들게된다.

Process | Burst Time(CPU 이용시간)
P1 | 6
P2 | 8
P3 | 7
P4 | 3

<center>
<figure>
<img src="/assets/post-img/OS/21.jpeg" alt="" width="50%">
</figure>
</center>

- Preemptive or Non-Preemptive
cf. Shortest-Remaining-Time-First(최소잔여시간 우선): 남아있는 시간이 얼마나 짧은가가 우선시가 된다.

- Example
  - Preemptive: AWT =(9+0+15+2)/4 = 6.5msec
  - Non-Preemptive: 7.75 msec

Process | Arrival-Time | Burst Time(CPU 이용시간)
P1 | 0 | 8
P2 | 1 | 4
P3 | 2 | 9
P4 | 3 | 5

<center>
<figure>
<img src="/assets/post-img/OS/22.jpeg" alt="" width="50%">
</figure>
</center>


### Priority

- typically an integer number
  - Low number represents high priority in general(Unix/Linux)
  - 우선순위, 컴퓨터 프로그램에서는 정수값으로 나타내는데 대부분의 운영체제에서는 숫자가 작을수록 우선순위가 높아진다.

- Example
- AWT = 8.2 msec

Process | Burst Time(CPU 이용시간) | Priority
P1 | 10 | 3
P2 | 1 | 1
P3 | 2 | 4
P4 | 1 | 5
P5 | 5 | 2

<center>
<figure>
<img src="/assets/post-img/OS/23.jpeg" alt="" width="50%">
</figure>
</center>

- 우선순위는 어떻게 정하는가?
  - Internal: time limit, memory requirement, i/o to CPU burst...
    - 내부: 짧을수록, 메모리를 적게 사용할수록, io시간이 길고 CPU 사용시간이 적을수록..
  - External: amount of funds being paid, political factors...
    - 외부: 돈을 많이 낸쪽을 먼저, 정치적인 요소일수록..

- Preemptive or Non-Preemptive
둘다 만들 수 있음

- Problem
  - `Indefinite blocking: starvation(기아)`
  : 한 프로세스가 끝나고 그 다음 우선순위가 작동을 하게 되는데, 이 와중에도 외부에서는 계속해서 작업들이 들어올것이다. 그런데 아무리 오래 기다리고 있다고 하더라고 새로 들어오는 애들이 더 우선순위가 높다면 이전에 있던 애들 중에서도 계속해서 메모리에 올라가지 못하는 애들이 있을 것이다.
  - Solution: `aging`
  : 오래 기다릴수록 우선순위를 조금씩 올려주는 것. 레디큐에 오래 머물고 있다면 점진적으로 우선순위를 조금씩 올려줘서 실행을 할 수 있도록 해줌.


### Round-Robin(RR)

빙빙돈다. 빙빙 돌면서 스케줄링한다.

- Time-Sharing system(시분할/시공유 시스템): TSS에서 많이 사용하는 방법
- `Time quantum(Δ) 시간양자 = time slice` (10~100msec) -> 1초동안에 보통 10~100번의 스위칭이 일어난다는 것을 의미!
  - 세 개의 프로세스가 있고 동일한 시간동안 돌아가면서 작업이 이루어지고 이 동일한 시간을 time quantum이라고 한다. 시간양자라고도 한다. 시간의 양. 시간 조각.
- Preemptive scheduling: p1이 안끝났다고 하더라도 일정 시간이 지나면 다음 프로세스로 넘어가기 때문에

- Example
  - AWT = 17/3 = 5.66msec

Process | Burst Time(CPU 이용시간)
P1 | 24
P2 | 3
P3 | 3

<center>
<figure>
<img src="/assets/post-img/OS/24.jpeg" alt="" width="50%">
</figure>
</center>

- Performance depends on the size of the time quantum
: time quantum의 크기에 굉장히 의존적이다.
  - Δ = ∞ : 해당 프로세스가 다 사용되고 나면 스위칭됨으로 FCFS와 같다.
  - Δ = 0 : Processor sharing(* context switching overhead)
    - 스위칭이 워낙 빈번하게 일어나서 3개의 프로세스가 거의 동시에 이루어지는것처럼 보여진다.
    - 실제로 TSS가 굉장히 빈번히 스위칭이 일어나니까 동시에 프로세스를 사용하는 것처럼 보인다.
    - 근데 이는 `context switching overhead`가 굉장히 빈번하게 된다.
    - Dispatcher(p1에서 p2로 넘어갈때 p1의 내용을 저장하고 p2를 로드해오는 시간을 context switching overhead라고 함)가 빈번하게 되면 결코 좋은 방법이 아니다.

**그래서 time slice를 너무 작게 하면 안된다.**

- Example: Average Turnaround time(ATT)
  - ATT = 11.0 msec(time quantum = 1), 12.25msec(time quantum=5)
  : 반환시간. 처음 들어간 시간부터 나온시간까지
  : time quantum을 얼마로 잡는가에 따라서 ATT가 되게 클수도 작을 수도 있다. 이에 따라서 성능이 달라진다. 좋은 time quantum을 잡는것이 중요하다.

<center>
<figure>
<img src="/assets/post-img/OS/26.jpeg" alt="" width="50%">
</figure>
</center>

Process | Burst Time(CPU 이용시간)
P1 | 6
P2 | 3
P3 | 1
P4 | 7

<center>
<figure>
<img src="/assets/post-img/OS/25.jpeg" alt="" width="50%">
</figure>
</center>


### Multilevel Queue Scheduling

- Process group: 실행중인 프로그램들을 그룹할 수 있다.
  - system processes: os안에서 작업하는 것.
  - Interactive processes: 사용자와 대화하는 프로그램, 게임같은 것.
    - 대화하지 않는 프로그램 : compile같은 것.
  - Interactive editing processes: 컴퓨터와 대화를 굉장히 많이한다.
  - Batch processes: 꾸러미, 일괄적으로 처리하는것, 일괄적으로 컴퓨터가 알아서 하니까 내가 관여할 것이 없다.
  - Student processes

프로세스들은 이렇게 그룹을 지을 수 있다. 성격이 다양한 애들을 동일한 줄에 세우는것은 안되는것. 그러니 큐를 하나만 두는것이 아닌 여러개를 두게 되었다.

- Single ready queue -> Several separate queues: 여러차원을 둔다.
  - 각각의 Queue에 절대적 우선순위 존재: system > interactive > batch > Student
  - 또는 CPU time을 각 Queue에 차등배분 : 한정된 CPU시간을 차등으로 배분
  - 각 Queue 는 독립된 scheduling 정책: 각각 프로세스마다 독립된 스케줄링 정책을 사용

<center>
<figure>
<img src="/assets/post-img/OS/27.jpeg" alt="" width="50%">
</figure>
</center>


### Multilevel Feedback Queue Scheduling

Queue를 여러개 두는것은 같으나

- 복수개의 Queue

- 다른 Queue로의 점진적 이동
  - 모든 프로세스는 하나의 입구로 진입
  - 너무 많은 CPU time 사용시 다른 Queue로
  - 기아 상태 우려시 우선순위 높은 Queue로!

<center>
<figure>
<img src="/assets/post-img/OS/28.jpeg" alt="" width="50%">
</figure>
</center>
