---
layout: post
title: 이중모드, 하드웨어 보호
category: OS
tags: [OS, operating system]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
경성대학교 양희재 교수님 수업 영상을 듣고 정리하였습니다.     

<hr>

## 이중모드(Dual Mode)

한 컴퓨터를 여러 사람이 동시에 사용하는데 한 사람이 고의/실수로 잘못된 명령을 내리면 그 영향이 전체에 미칠 수 있다.

cpu를 다운 시키는 명령어에는 `stop, reset, halt`가 있는데, 어떤 사용자가 프로그램을 하드디스크에 깔면서 stop이라는 명령을 넣어뒀다고 하자. 이후 프로그램을 실행시키면 해당 프로그램은 메인 메모리로 올라가게 될 것이고 stop명령을 cpu가 들고와 실행한다면 cpu는 정지하게 된다. 정지된 cpu가 다시 동작하는 방법은 컴퓨터를 껏다 키는 방법 뿐이다. 서버컴퓨터일수록 그 시간은 더 오래 걸리고 혼자 사용하는거면 그나마 났지만, 여러명이 하는 컴퓨터일수록 치명적인 상황을 초래할 것이다.

그래서 사용자 프로그램(일반 유저)들은 이런 명령어를 사용하면 안된다. 즉, 오로지 관리자만 내릴 수 있어야한다. 그래서 `이중모드`를 만들었다.

이중모드란 cpu가 동작하는 모드를 2가지로 두겠다는것을 의미한다.

- 사용자(user) 모드: 일반 사용자가 사용하는 모드
- 관리자(supervisor,시스템(system), 모니터(감시자, monitor), 특권(privileged mode) 모드: 컴퓨터에서 관리자는 os

cpu가 메모리에 있는 명령을 읽어와서 실행하는데, os에 있는 명령을 실행할 적에는 cpu가 관리자 모드에서 동작하도록 하고 일반 유저 영역의 메모리를 가져와 실행하면 사용자모드에 있도록 한다.

> 특권명령(privileged instruction): 관리자 모드에서만 내릴 수 있는 명령인 stop, halt, reset, set_timer, set_HW 등 은 일반 사용자가 아닌 반드시 os(운영체제, 관리자)만이 내릴 수 있도록 한다.


### 이중모드는 어떤식으로 만드는가?

cpu안에는 레지스터(비트들의 모음. 32bit라고 하면 비트가 32개 들어있음), alu, cu도 있는데, 이 비트들에는 어떤 프로세서(cpu)의 상태를 알려주는 비트를 둘 수 있다.

- 캐리(carry)
- negative: 연산의 결과가 음수가 나왔을때
- zero: 앞의 연산이 0이 나올때
- overflow: 앞의 연산이 그 연산 결과가 자리 범위를 넘었을때

`fleg` :깃발, 사건이 일어나면 깃발을 들고 사라지면 내리는 것과 같이 상태를 나타내는 것을 플래그라고 한다 즉, 1이면 발생한것이고 0이면 발생 안한 것!


### 이중모드에서 갑자기 플래그?

레지스터 안에 있는 1개의 비트를 모니터 비트로 할당해서 이중모드를 나타낸다.(이중모드를 나타내는 비트를 하나 더 추가한다. 그때 비트의 이름이 monitor, system 등이다.)

즉, `1이면 시스템 모드, 0이면 유저 모드!`

처음 파워를 키면 부팅을 통해 os가 메인메모리로 올라가고, 올라가는 도중에는 모두 모니터 비트가 다 1(시스템모드)이다. 즉, os가 동작할 적에는 시스템모드에서 동작한다(os가 모든 명령을 다 내릴 수 있다) 그래서 os 부팅이 끝나면 이제 메모리에 사용자 프로그램을 올릴 것이고, cpu는 그 프로그램을 실행하게된다. 이때는 유저모드가 동작한다. 즉, os가 돌적에는 시스템 모드지만 이제 os에서 사용자 프로그램으로 갈 적에는 os의 비트를 0으로 만들어줌으로써 유저 모드가 되도록 한다. 그래서 사용자 프로그램이 동작할 때는 유저모드에서 동작하는 것이다.

os가 새로운 프로그램을 실행하면 그 프로그램가 올라오고 (os가 해당 프로그램을 하드디스크에서 메모리로 올리는 행위) os가 프로그램을 메모리에 올리고나면 `cpu에서는 프로그램이 실행되기 직전에 해당 비트를 1에서 0으로 만들어준다.` 그래서 사용자프로그램이 사용될때는 사용자모드에서 동작이 된다. 따라서 사용자 프로그램에서는 stop과 같은 특권 명령어는 사용하지 못한다.


#### 만약 내가 사용자프로그램에서 햇던 행위를 하드디스크에 저장하고싶다면?

프로그램 자체가 하드디스크에 저장을 해도되지만 문제가 많다. 이는 곧 유저 프로그램이 하드디스크에 접근이 가능하다는 것인데, 그 의미는 하드디스크의 남의 파일애도 접근이 가능하다는 뜻이다. 즉 서버컴퓨터에는 여러 사용자들의 파일들이 저장되어있을텐데 다른 사람들의 파일에도 접근이 가능하다는 의미로 일반 사용자프로그램이 하드디스크를 직접 접근하는것은 보안에 심각한 문제를 끼친다.

즉, 일반 프로그램이 하드디스크에 접근하는 할 때에는 해당 프로그램이 os에 부탁을 해야한다. 이 부탁은 `swi`를 이용해서 부탁한다.

swi가 온다면 cpu는 지금 하는 일을 중지하고 os의 isr로 접근하여 하드디스크에 저장할 수 있도록 한다. os로부터 인터럽트를 걸어서 cpu의 모드가 1로 변경되면 이제 os에서는 무슨 명령이든 내릴 수가 있게되고 이때 유저 프로그램에서는 그 어떤 명령들도 내릴 수가 없게된다. 이제 저장을 하고나면 다시 유저영역으로 돌아오고 모니터비트를 다시 0으로 바꿔준다.

대부분의 모든 cpu는 이중모드를 지원한다. (모니터를 나타내는 비트가 따로있음) 그래서 절대 일반유저가 특별한 명령을 내리지 못한다.

만약 내리려면 어떻게 될까?

cpu가 그 명령을 읽어왔는데 모니터비트가 0임을 확인했는데 유저모드에서 stop명령이 왔다면 cpu는 이 명령이 잘못된 명령이라고 생각해서 내부적으로 인터럽트가 발생했다고 생각한다. 그래서 os의 isr(해당 프로그램이 잘못된 명령을 내렸을때 처리하는 루틴)을 통해 그 프로그램을 강제로 종료시킨다. 즉 바로 메모리에서 삭제시켜버린다.


이중모드는 이런면에서 보호와 관련이 있다.(protection)


### 컴퓨터에서 보호받아야하는 3가지

#### 1. 입출력장치 보호(IO, Input/Output device protection)

일반사용자가 하드웨어를 자기멋대로 사용할 수 있도록 하면 안된다.

- 서버컴퓨터는 동시에 여러사람이 사용하는데 거기에 프린트가 달려있다고 하자.
- 한 사용자가 프린트를 사용하고 있는데 다른 사용자가 그 프린트에 대해 리셋 혹은 자기도 찍겠다고 신호를 보낸다면 프린트의 한줄에는 다른 사람꺼도 찍히게 된다.
  - 프린트 혼선, 강제 리셋 발생
- 혹은 프린트 말고도 서버컴퓨터에는 대용량 하드디스크도 달려있다.
- 하드디스크에 여러사람들의 파일들이 들어있을텐데 뿐만 아니라, 자신의 중요정보를 하드디스크에 저장했는데 이를 마음대로 누군가 접근해서 보면 안된다.
  - priviliged instruction violation
  - 그러니까 프린트, 하드디스크 등은 아무나 볼 수 없도록 보호를 받아야 한다.

이를 보호하려면, 이 접근 자체를 못하도록 하려면 입출력장치를 제어해야한다. 입출력장치와 관련이 있는 명령어들은 아래와 같다.

-> in(키보드나 마우스 같은 입출력 장치로부터 정보를 받아들이는 것),
-> out(출력장치에 명령을 내리는것(출력을 내보내라고), 프린트, 디스크, 스피커, 램 등)

-> 이런 명령어들을 priviliged instruction 함으로써 os만 내릴 수 있도록 해야한다. 즉, 입출력 관련 명령어는 무조건 특권명령어이다.


#### 2. 메모리 보호 (Memory protection)

유저1 프로그램이 돌면서 해당 프로그램이 유저 2,3 메모리 영역을 읽으려고 하거나 혹은 os안의 값을 바꾸려고 한다면 이를 해킹이라고 하고 이는 보호받아야한다.

- cpu에서 메모리로 address bus(몇번지를 읽겠다는 명령)가 가면 data bus(몇번지에 해당하는 데이터)를 통해 cpu로 오게 된다.
- 일반 유저프로그램이 os나 다른영역에 가지 않게 하려면 address bus를 잘라버려야 하는데, 이는 자기 영역에도 못들어가게 함으로 해결책이 되지 않는다.

그러면 다른 방법은 address bus에 문지기를 세워두는 것!

어떤 주소를 보낼 적에 그 주소가 해당 영역에 해당되는 주소가 맞다면 문지기가 통과시켜주고 벗어나면 문지기가 거절을 내버림.

> 문지기(MMU: Memory management unit)는 실제로 레지스터를 둔다. base, limit 이런식으로 base에서 limit 사이에 들어오면 통과, 벗어나면 cpu로 다시 인터럽트 신호가 가도록한다. 인터럽트 신호를 보내주면 cpu는 하던일을 중지하고 os의 isr로 점프하고 해당 isr는 잘못된 번지를 읽으려고 시도하면 그 프로그램을 강제로 종료시킨다.

`segment violation : 영역에 대한 침범 > 해당 에러를 발생시키며 강제 종료시킴!`

이 mmu의 base, limit 값 설정은 os가 해준다. 이 값을 바꾸는 것은 특권명령으로 해서 올려둬야한다.


#### 3. CPU 보호 (CPU protection)

cpu도 어떤 침범 대상이 된다. 만약 유저 1가 cpu 시간을 독접하게 되어버리면 문제가 될테니까..

따라서 해당 프로그램마다 timer를 둔다. timer는 일정 주기로 cpu에 인터럽트를 걸도록 회로가 설계되어있다. 그러면 해당 시간마다 인터럽트가 걸리면 cpu는 os의 isr로 점프하고 해당 isr에는 해당 cpu가 골고루 돌아가고 있는지를 체크하고, 오랜 시간이 지났는데도 한 유저에게만 묶여있다면 isr이 cpu로 하여금 강제로 해당 프로그램이 아닌 다음 프로그램으로 넘어가도록 조정해준다.

즉, 어느 한 유저프로그램에만 묶여있지않도록 한다. 그래서 timer를 두면 일정시간 지나면 인터럽트 걸리고 해당 isr로 점프하게 되어 해당 루틴이 너무 묶여있는 것 같다면 다음 프로그램으로 넘어가도록 한다. 