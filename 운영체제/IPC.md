# Inter-Process Communication

프로세스간 서로 데이터를 어떻게 주고 받을까? 이에 대해 알아보자.

- Process
    - 실행 중인 프로그램
- IPC란?
    - 다양한 프로세스가 **서로 데이터를 교환**하거나 **협력**할 수 있도록 하는 매커니즘
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/931b8051-99f5-4b90-8166-a68022df1900/782e00d5-df49-49ae-aa60-bee370c29ca3/image.png)
    
- 필요성
    - Information Sharing 데이터 공유
        - 여러 Process가 동일한 데이터에 대해 concurrent한 접근과 수정이 가능하다.
    - Computation Speed up 연산 속도 증가
        - 하나의 Task를 여러 subtask로 나누어 작업이 가능해 효율적으로 처리할 수 있다.
    - Modularity
        - 시스템 기능을 프로세스, 또는 쓰레드 단위로 나누어 모듈화할 수 있다.
    - Convenience
        - 동시에 여러 task를 처리할 수 있다.

> IPC의 대표 모델
> 
> - Message Passing (통신)
>     - 구현 방법으로 `Mesage Queue`, `Pipe`, `Socket` 등이 있다.
> - Shared Memory

## 파이프

하나의 프로세스가 다른 프로세스로 데이터를 직접 전달하는 방식이다.

- 익명 파이프
    - 통신할 프로세스를 명확히 알 수 있는 경우 사용한다.
- Named PIPE(FIFO)
    - 전혀 모르는 상태의 프로세스 간 통신에 사용한다.
- Half-Duplex
    - Pipe는 두 개의 프로세스를 연결하여, 하나의 프로세스는 데이터를 쓰기만, 다른 하나는 데이터를 읽기만 할 수 있다.
    - 한 방향으로만 데이터가 이동한다. 따라서 양방향 통신을 위해서는 두개의 파이프가 필요하다. 다만 구현하기 어렵다는 단점이 존재한다.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/931b8051-99f5-4b90-8166-a68022df1900/b24bd8d6-2e17-4241-9bb0-8a2a73c8c406/image.png)

## 메시지 큐

프로세스 간 메세지를 전송할 수 있는 큐를 제공한다.

메시지는 큐에 저장되고 수신 프로세스가 이를 읽어 처리한다. 

system call -  `send`, `receive`

모든 메세지 송수신에 system call이 요구된다.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/931b8051-99f5-4b90-8166-a68022df1900/a7bd180b-b84d-45e4-b0ba-2119d97e5369/image.png)

## 소켓

네트워크 통신을 통해 로컬 또는 원격 프로세스간 데이터 전송하는 방식

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/931b8051-99f5-4b90-8166-a68022df1900/8be68742-9414-4e68-8f58-25a0f26442ea/image.png)

## 공유 메모리

여러 프로세스가 동일한 메모리 공간에 접근하는 방식으로 중개자 없이 바로 메모리 접근이 가능해 IPC 중 가장 빠르게 작동한다.

`Read`, `Write`

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/931b8051-99f5-4b90-8166-a68022df1900/f9511c02-f250-4de3-aa12-b19e3e6ddb1d/image.png)

- 해결해야 할 문제점
    - 두 프로세스가 **동시**에 공유된 메모리에 접근하려 할때 어떻게 처리할 것인가?
    - **Muli-core** processor의 경우 core마다 독립적인 cache가 존재한다.  한 cache가 메모리 데이터를 업데이트 하면 다른 cache에도 해당 변화를 알려야 데이터 불일치가 발생하지 않도록 한다.

### 질문

1. IPC가 필요한 이유 4가지를 서술하시오.

### 참고 자료

https://minyakk.tistory.com/46

이미지 https://blog.naver.com/bycho211/220985701140

https://milktea24.github.io/posts/os-IPC/