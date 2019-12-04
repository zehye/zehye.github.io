---
layout: post
title: 프로세스의 정의, CPU 스케줄러, 멀티 프로그래밍
category: OS
tags: [OS, operating system]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
경성대학교 양희재 교수님 수업 영상을 듣고 정리하였습니다.     

<hr>

## 프로세스 관리 (Process Management) = CPU 관리

### 프로그램 vs 프로세스

프로세스(process, task, job..): program in execution, 실행중에 있는 프로그램을 프로세스라고 한다.

`즉, 하드디스크 안에는 프로그램, 메모리 안에는 프로세스라고 부른다.`

cpu는 메인메모리 안의 process들을 관리하고 이를 관리한다는것은 cpu가 할당되어지는 시간을 관리하는 것을 의미한다.


### Multiprogramming

<center>
<figure>
<img src="/assets/post-img/OS/14.jpeg" alt="" width="50%">
</figure>
</center>

- new: 하드디스크로부터 메인메모리로 프로세스가 올라온 상태
- ready: 모든 초기화를 끝내고 진짜 실행할 모든 준비가 다 된 상태
- running: 실제로 cpu에 의해 실행상태가 됨, cpu가 실제 실행하는 상태 = 러닝 상태
- waiting: 프린트할때는 cpu가 더이상 해당 서비스가 아닌 다음 프로세스로 넘어가 다른 프로세서의 러닝상태를 만들어주고 cpu가 실행하고 있던 프로세스는 웨이팅 상태
  - 근데 이때 프린트를 다 하고 끝나면 다시 레디 상태로 온다. 즉 다시 cpu 서비스를 받을 준비를 하고있다는 상태.
- terminated: 프로그램이 끝난 상태

### TSS

<center>
<figure>
<img src="/assets/post-img/OS/15.jpeg" alt="" width="50%">
</figure>
</center>

IO를 만나지 않더라도 일정 시간이 지나면 강제로 스위칭!
- time expired: 자기에게 주어진 일정시간이 지나면 자동으로 레디로 감


## PCB(Process Control Block)

- 프로세스 제어 블록으로 TCB(Task Control Block)이라고도 부른다.
  - 이 블락안에는 프로세스에 대한 모든 정보들이 들어있음
  - 한 개의 프로세스에 대해 한 개의 PCB가 할당되며 PCB에는 프로세스에 대한 모든 정보가 들어있다.

- 프로세스의 상태(process state)- running, ready, waiting...
- 번지정보(PC, program counter)
- register: 다른 레지스터들의 정보
- MMU info(base, limit)
- CPU Time
- PID(Process ID, 프로세스마다의 번호)
- list of open files(어떤 파일들을 사용하고있는지)
....


프로세스는 사람과 비슷한데, 이 PCB는 OS안의 process management 안에 들어있다. 모든 정보를 가지고 있어야 관리가 가능하고 관리를 하려면 모든 정보가 필요하다. 프로세스에 대한 정보는 모두 os안의 프로세스 관리 안에서 다 가지고 있다.


## Queue

하드디스크에는 수많은 프로그램들이 있지만 이 모든 프로그램들을 메인메모리가 다 받아줄 수는 없다. 그렇기 때문에 하드디스크에서 메인메모리로 가는 과정에 대기하는 줄이 있고 그것을 Job Queue라고 한다. 그리고 메인메모리에 올라왔다고 해서 바로 서비스를 받지도 못한다. CPU는 하나뿐이기 때문에 결국 한번에 작업은 하나밖에 받지를 못한다.

즉, CPU의 작업을 받기위해서도 줄을 서서 기다려야 한다. 작업을 하고 있는 와중에 IO를 하기 위해서라도 줄을 서서 기다려야 한다.

- Job Queue: 하드디스크에서 메인메모리로 올라올때의 대기줄
- Ready Queue: 메인메모리에서 CPU 서비스를 받기 위한 대기줄
- Device Queue: 프린트나 하드디스크와 같은 디바이스를 사용하기 위한 대기줄

<center>
<figure>
<img src="/assets/post-img/OS/17.jpeg" alt="" width="50%">
</figure>
</center>


### 하드디스크에서 메인메모리로 가는 프로세스의 우선순위는 어떻게 정해지는가?

= Job scheduler(long-term scheduler): 메모리에 어느 프로세스를 올릴지를 결정하는 코드로 이미 메인메모리가 꽉 차여있다면 결정할 필요가 없다. <br>
어느 한 프로세스가 끝나 메모리가 비어지면 결정을 내리는 것으로 스케줄링은 자주 일어나지 않다.

= CPU scheduler(short-term scheduler): 프로세스간의 스위칭이 빠르게 돌아가야 한다. 1초에도 수십번씩 일어남. 컴퓨터에서는 CPU가 제일 중요!  

= Device scheduler

- OS안의 Process Management안에는 줄이 많이 서있다.(Job, Ready, Device Queue)
- 각 줄 안에는 프로세스들이 줄을 서서 기다리고 있고
- 각 줄에서 어느 프로세스를 먼저 꺼내서 할지를 결정하는 것은 scheduler 프로그램이 결정한다.


## 멀티 프로그래밍

현대의 운영체제는 대부분 다 멀티 프로그래밍이다. (하나의 메모리에 여러개의 프로세스를 올리는 것)

- Degree of multiprogramming: 멀티프로그래밍의 정도로 메인메모리에 프로세스가 몇개 올라가져있는가를 살펴본다.
- I/O-bound process vs CPU-bound process

: 프로세스를 크게 두개 종류로 나눔<br>
-> I/O-bound process: IO관련 작업만 하는 프로세스, 워드같은 프로그램<br>
-> CPU-bound process: CPU 사용하는 프로세스 (계산 많이 하는 프로세스), 슈퍼컴퓨터 사용하는 애플리케이션. 일기예보 등

골고루 사용되기 위해 job scheduler가 적절히 올려주는 것이 중요! 즉, os는 적당히 io와 cpu가 일하도록 해줘야한다!

- Medium-term scheduler

: 대화형시스템(TSS)에서...

-> 서버컴퓨터를 두고 여러 사용자가 컴퓨터를 사용하고 있다고 하자. 서버컴퓨터 내의 메모리에는 각각 사용자에 대한 프로세스들이 준비되어있을 것이다. 그런데 한 사용자가 잠시 작업을 중단했다고 하자. 그렇다는 것은 그 사용자를 위한 메모리는 현재 준비되어있지만 아무것도 하고 있지 않음을 의미한다. 실제 CPU 또한 그 사용자의 일을 하지 않을 것이다. CPU 입장에서는 해당 사용자의 메모리 사용이 아까울 것이다. os의 프로세스 관리부서(cpu사용시간을 관리하는 부서)에서 메모리를 사용하지 않음을 발견하고 이를 하드디스크에 메모리 전체를 쫓아낸다.`(swap out)` 비어진 메모리 공간에는 다른 프로세스를 올릴수도 있고 나머지 프로세스의 메모리 크기를 더 크게 해줄 수 있게 된다.

- `swapping`: 메인메모리에서 하드디스크로 쫓아내는 행위(swap out) 프로세스 이미지를 쫓아내는 행위로서 디스크를 사용하는 것(backing store-swap device), 사용자가 다시 돌아와서 해당 프로세스를 사용하고자 하여 디스크에 들어갔던 프로세스를 다시 메모리로 가져오는 행위(swap in) 이 모든 행위를 swapping이라고 한다.

즉, medium-term scheduler은 스케줄링이 중간 즈음에 일어나는 것을 의미한다. os가 지켜보고 있다가 어떤 애를 몰아낼 것인지를 결정하는것으로 os가 쭉 메모리를 뒤져가지고 현재 사용되지 않은 어느 놈을 하드디스크로 몰아낼 것인가를 결정하는 것을 의미한다.

<center>
<figure>
<img src="/assets/post-img/OS/16.jpeg" alt="" width="50%">
</figure>
</center>

- `Context switching(문맥전환)`

프로세스가 메인메모리에 여러개 있어도 CPU에서는 하나밖에 일 하지 못한다. 따라서 프로세스간의 이동을 말한다.

  - Scheduler: 다음에 무엇을 할지를 결정하는것. 지금 프로세스 다음 어느 프로세스를 할 것인지 결정하는 컴퓨터 프로그램
  - Dispatcher: 실제로 스케줄러가 선택한 프로세스를 실행하도록 상태, 값을 바꾸어주는 행위
  -> p1의 현재 상태를 os의 PCB에 다 저장해놔야 한다. 나중에 다시 돌아왔을때 그 상태로 돌아갈 수 있도록
  -> MMU의 base, limit 정보도 PCB에 저장해놓는다
  -> p1에서 p2를 넘어가려면 p1의 현재 상태를 PCB 에 저장해놓아야 한다.
  -> restore
  -> 이는 os안의 Dispatcher라는 프로그램이 해준다.
  -> context switching overhead: 저장하고 복원할때마다의 부담이 있기 때문에 이를 너무 자주하면 안된다. overhead의 숫자가 너무 많아진다. 이를 줄이는게 좋음!

  <center>
  <figure>
  <img src="/assets/post-img/OS/18.jpeg" alt="" width="50%">
  </figure>
  </center>
