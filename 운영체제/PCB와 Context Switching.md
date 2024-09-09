# PCB와 Context Switching
## 목차

1. [PCB (Process Control Block)](#pcb-process-control-block)
    - [PCB 상세구조](#pcb-상세구조)
2. [Context Switching](#context-switching)
    - [Context Switching 과정](#context-switching-과정)
    - [Context Switching Overhead](#context-switching-overhead)

---
## PCB (Process Control Block)

**PCB는 운영체제에서 프로세스를 관리하기 위해 해당 프로세스의 상태 정보를 담고 있는 자료구조를 말한다.** <br>

프로세스를 컨텍스트 스위칭 할 때 기존 프로세스의 상태 및 작업 상태를 어딘가에 저장해둬야 
어디서부터 다시 작업을 시작할 지, 어떤 작업을 이어가야 할 지 결정할 수 있을 것이다. <br>
PCB는 프로세스 스케줄링을 위해 프로세스에 관한 이러한 모든 정보들인 메타데이터(Metadata)를 저장하는 임시 저장소 역할을 한다.

![1](https://github.com/user-attachments/assets/3ca01634-1922-44d4-a326-2c3e62bc4c4d)


- PCB는 프로세스의 생성과 함께 만들어진다.
- 프로그램 실행 → 프로세스 생성 → 프로세스 주소 공간에 code, data, stack 생성 → PCB에 프로세스 메타데이터 저장

따라서 운영체제는 PCB에 담긴 프로세스 고유 정보를 통해 프로세스를 관리하며, 프로세스의 실행 상태를 파악하고, 우선순위를 조정하며, 스케줄링을 수행하고, 다른 프로세스와의 동기화를 제어한다.
<br></br>

### PCB 상세구조

![2](https://github.com/user-attachments/assets/545824af-ecd8-44a6-bf84-8d890228ee1f)

- 포인터(Pointer): 프로세스의 현재 위치를 저장하는 포인터 정보
- 프로세스 상태(Process State): 프로세스의 각 상태 - 생성(New), 준비(Ready), 실행(Running), 대기(Waiting), 종료(Terminated) 를 저장
- 프로세스 아이디(Process Number): 프로세스 식별자를 지정하는 고유한 ID
- 프로그램 카운터(Program Counter):  프로세스를 위해 실행될 다음 명령어의 주소를 포함하는 카운터를 저장
- 레지스터(Registers): 누산기, 베이스, 레지스터 및 범용 레지스터를 포함하는 CPU 레지스터에 있는 정보
- 메모리 제한(Memory limits): 운영 체제에서 사용하는 메모리 관리 시스템에 대한 정보
- 열린 파일 목록(List of open files): 프로세스를 위해 열린 파일 목록
<br></br>

## Context Switching

**컨텍스트 스위칭(Context Switching)은 CPU가 한 프로세스에서 다른 프로세스로 전환할 때 발생하는 일련의 과정으로,** 
동작 중인 프로세스가 대기를 하면서 해당 프로세스의 상태(Context)를 보관하고, 
대기하고 있던 다음 순서의 프로세스가 동작하면서 이전에 보관했던 프로세스의 상태를 복구하는 작업을 말한다. <br>
CPU는 한 번에 하나의 프로세스만 실행할 수 있으므로, 여러 개의 프로세스를 번갈아가며 실행하여 CPU 활용률을 높이기 위해 컨텍스트 스위칭이 필요하다.

_(이러한 컨텍스트 스위칭이 일어날 때 다음 프로세스는 스케줄러가 결정하게 된다.)_
<br></br>

### Context Switching 과정

![3](https://github.com/user-attachments/assets/6c0c5ea5-2b26-47d9-8a76-f94b8476cc2e)

1. CPU는 Process P1을 실행한다. (Executing)
2. 일정 시간이 지나 Interrupt 또는 system call이 발생한다. (CPU는 idle 상태)
3. 현재 실행 중인 Process P1의 상태를 PCB1에 저장한다. (Save state into PCB1)
4. 다음으로 실행할 Process P2를 선택한다. (CPU 스케줄링)
5. Process P2의 상태를 PCB2에서 불러온다. (Reload state from PCB2)
6. CPU는 Process P2를 실행한다. (Executing)
7. 일정 시간이 지나  Interrupt 또는 system call이 발생한다. (CPU는 idle 상태)
8. 현재 실행 중인 Process P2의 상태를 PCB2에 저장한다. (Save state into PCB2)
9. 다시 Process P1을 실행할 차례가 된다. (CPU 스케줄링)
10. Process P1의 상태를 PCB1에서 불러온다. (Reload state from PCB1)
11. CPU는 Process P1을 중간 시점부터 실행한다. (Executing)
<br></br>

### Context Switching Overhead

컨텍스트 스위칭 과정은 사용자에게 빠른 반응성과 동시성을 제공하지만, 
실행되는 프로세스를 변경하는 과정에서 프로세스의 상태, 레지스터 값을 저장하고 불러오는 등의 작업을 수행하기 때문에 시스템에 부담을 주게 된다. <br>

위의 컨텍스트 스위칭 과정 그림을 보면 P1이 execute에서 idle로 될 때 P2가 바로 execute 되지 않고 idle 상태에 조금 있다가 execute 되는걸 볼 수 있다. 
**이 간극이 바로 컨텍스트 스위칭 오버헤드(overhead)이다.**
<br></br>

> 참고 1

컨텍스트 스위칭 오버헤드는 대표적으로 다음과 같은 행위에 의해서 발생된다.

- PCB 저장 및 복원 비용
- CPU 캐시 메모리 무효화에 따른 비용
- 프로세스 스케줄링 비용

컨텍스트 스위칭 과정에서 PCB를 저장하고 복원하는데 비용이 발생하며, 프로세스 자체가 교체되는 것이니 CPU의 캐시 메모리에 저장된 데이터가 무효화가 된다.
이 과정에서는 메모리 접근 시간이 늘어나고, 성능 저하가 발생할 수 있다. 
또한 CPU 스케줄링 알고리즘에 따른 프로세스 선택에도 비용이 발생한다.
<br></br>

> 참고 2

컨텍스트 스위칭은 프로세스 뿐만 아니라 여러 개의 스레드 사이에서도 일어난다.
보통 멀티 스레드라고 하면 여러 개의 스레드가 동시에 돌아가니 프로그램 성능이 상승할 것이라 예상할 수 있다.
그러나 컨텍스트 스위칭 오버헤드라는 변수 때문에 스레드 교체 과정에서 과하게 오버헤드가 발생하면 오히려 멀티 스레드가 싱글 스레드보다 성능이 떨어지는 현상이 나타날 수 있다.

---
### ❓ 질문

1. PCB에 저장되는 주요 정보는 무엇인가요?
2. 컨텍스트 스위칭은 무엇이며, 필요한 이유는 무엇인가요?
3. 컨텍스트 스위칭 중 발생하는 오버헤드의 원인은 무엇인가요?
