# Blocking/Non-blocking & Synchronous/Asynchronous

## Blocking / Non Blocking
 - 다른 함수가 작업 중에 자신이 제어권이 있는지 없는지로 판단

### Blocking
 - A 함수가 B 함수를 호출했을 때, B 함수의 작업이 종료되기 전까지 제어권을 돌려주지 않음

### Non-Blocking
 - A 함수가 B 함수를 호출했을 때, B 함수의 작업이 종료되기 전에도 A 함수가 다른 작업을 실행 가능

![alt text](/네트워크/images/Block_NonBlock_Sync_Async/image.png)

## Sync / Async
 - 호출된 함수의 종료를 호출한 함수가 처리 하는지, 호출 된 함수가 직접 처리하는지로 판단

### Sync
 - A 함수가 B 함수를 호출했을 때, B 함수의 종료를 A함수가 처리

### Async
 - A 함수가 B 함수를 호출했을 때, B 함수의 종료를 B함수가 직접 처리 (callback 함수)

![alt text](/네트워크/images/Block_NonBlock_Sync_Async/image-1.png)

## Ex) I/O 처리 상황
 - Sync / Async 차이: 요청한 순서가 지켜지는가 아닌가

 - Blocking / Non-Blocking 차이: 요청에 대해 받은 함수에서 처리가 끝나기 전에 리턴을 해주는가 아닌가

![alt text](/네트워크/images/Block_NonBlock_Sync_Async/image-2.png)

### Sync-Blocking 방식
 - 결과에 대한 순서 보장

 - 호출한 측에서 호출된 함수를 처리

 - 결과값이 반환되기 전까지 제어권 X

### Sync-Non Blocking 방식
 - 결과에 대한 순서 보장

 - 호출한 측에서 호출된 함수를 처리

 - 호출 후 다른 작업 수행 가능

 - 주기적으로 작업의 완료를 확인함 (Pooling, Interrupt 방식)

### Async-Blocking 방식
 - 결과의 순서를 보장하지 않음

 - 호출된 함수가 종료 후 callback 함수 호출

 - 제어권 X

 - 자주 사용하지 않음

### Async-Non Blocking 방식
 - 결과의 순서를 보장하지 않음

 - 호출된 함수가 종료 후 callback 함수 호출

 - 호출 후 다른 작업 수행 가능

![alt text](/네트워크/images/Block_NonBlock_Sync_Async/image-3.png)

---
Q1. Blocking과 Non-Blocking 방식의 차이점은?

Q2. Sync와 Async 방식의 차이점은?

Q3. Sync-Blocking 방식과 Async-Non Blocking 방식에 대해 설명하시오.

Reference
---
 - https://velog.io/@maketheworldwise/SyncAsync-BlockingNon-Blocking-%EB%AC%B4%EC%8A%A8-%EC%B0%A8%EC%9D%B4%EC%9D%BC%EA%B9%8C

 - https://jh-7.tistory.com/25

 - https://engineerinsight.tistory.com/295#%F0%9F%92%8B%C2%A0Synchronous%20non-Blocking%20I%2FO-1

 - https://steady-coding.tistory.com/531

