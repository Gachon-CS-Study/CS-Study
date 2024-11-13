## 목차

- [Linked List](#linked-list)
   - [Linked List의 구조 및 요소](#linked-list의-구조-및-요소)
   - [Linked List 종류](#linked-list-종류)
   - [Linked List 장단점](#linked-list-장단점)
- [구현 by Python](#구현-by-python)
   - [삽입, 삭제](#삽입-삭제)

---
## Linked List

Linked List란, 떨어진 곳에 존재하는 데이터를 화살표로 연결해서 관리하는 데이터 구조로, 연결 리스트라고 불리기도 한다.
<br></br>

> **❓ Linked List 자료구조의 필요성**
> 

컴퓨터에 데이터를 저장할 때 변수를 사용해야 한다. 여러 개의 데이터를 다룰 때는 필요한 수 만큼 변수를 선언해야 하는데, 이때 배열을 쓰면 원하는 수 만큼의 변수를 한번에 만들 수 있다. 그러나 다음과 같은 상황에서는 메모리 효율이 떨어진다.

(1) 배열로 100개의 변수를 선언했지만, 실제로 사용되는 데이터는 10개 미만일 경우 → 메모리 낭비
(2) 배열로 100개의 변수를 선언했는데, 실제로 사용되는 데이터는 100개 초과인 경우 → 메모리 부족

이처럼 데이터의 개수는 가변적일 수 있는데, 고정된 개수의 변수를 선언해야 하는 배열은 효율적이지 못하다. <br></br>
따라서 데이터 수에 맞춰 효율적으로 메모리를 활용할 수 있는 Linked List를 사용할 수 있다.
<br></br>

### Linked List의 구조 및 요소

![image](https://github.com/user-attachments/assets/5831649f-2944-4769-a277-85bbe87cf872)

1. **노드(Node)**: Linked List의 기본 단위로서, 데이터를 저장하는 *데이터 필드* 와 다음 노드를 가리키는 *링크 필드* 로 구성된다.
2. **포인터**: 각 노드 안에서, 다음이나 이전의 노드와의 연결 정보를 가지고 있는 공간
3. **헤드(Head)**: Linked List에서 가장 처음에 위치하는 노드를 가리킨다. 리스트 전체를 참조하는데 사용된다.
4. **테일(Tail)**: Linked List에서 가장 마지막에 위치하는 노드를 가리킨다. 이 노드의 링크 필드는 Null을 가리킨다.
<br></br>

### Linked List 종류

1. **단방향 연결 리스트 (Singly Linked List)**

위의 그림처럼, 다음 노드를 가리키는 링크 필드 하나만을 가지고 있는 경우, 이를 단방향 연결 리스트라고 한다. 

이러한 단방향 연결 리스트는 현재 요소에서 이전 요소로 접근해야 할 때 매우 부적합하다. 각 노드가 단방향(뒤의 노드)에 대한 정보를 가지고 있어서 특정 노드를 탐색할 때 단방향으로 가야하기 때문이다.
<br></br>

2. **양방향 연결 리스트 (Doubly Linked List)**

![image](https://github.com/user-attachments/assets/0fad47e2-09ca-4051-9763-a4ec465bb4c6)

단방향 연결 리스트의 단점을 보완하고자, 양방향에 대한 정보를 가지는 양방향 연결 리스트가 존재한다.

양방향 연결 리스트의 노드는 다음 노드의 포인터뿐만 아니라 이전 노드의 포인터 정보도 함께 가진다. 그래서 현재 노드에서 앞/뒤 방향 모두에 대해 노드를 탐색할 수 있기 때문에 각 요소에 대한 접근과 이동이 용이하다.
<br></br>

3. **양방향 원형 연결 리스트 (Circular Linked List)**

<img src="https://github.com/user-attachments/assets/b85e814e-3cc6-4cce-9512-9626479ed07a" width="580px" />

원형 연결 리스트는 양방향 연결 리스트에서 헤드와 테일을 연결한 구조이다.

첫번째 노드와 마지막 노드를 연결 시켜, 마지막 노드가 처음 노드를 참조해야 하는 경우 사용될 수 있다. 
<br></br>

### Linked List 장단점

> **장점**

연결 리스트는 동적으로 데이터를 관리하는 데 있어서 여러 가지 장점을 가진다.

- **동적 크기**: 미리 데이터의 크기를 지정할 필요가 없다. 배열과는 달리, 런타임 중에도 데이터를 추가하거나 삭제하는 것이 가능하므로 유연성이 뛰어나다.
- **효율적인 메모리 사용**: 노드가 메모리의 어디에나 위치할 수 있다. 따라서 메모리를 효율적으로 활용할 수 있다. 반면에 배열은 연속적인 메모리 공간을 필요로 하기 때문에, 크기를 변경하려면 메모리를 재배치해야 할 수도 있다.
- **데이터 삽입 및 삭제의 용이성**: 노드를 삽입하거나 삭제하는 과정이 간단하다. 특정 노드의 참조만 변경하면 되므로, 이러한 연산은 상수 시간에 이루어질 수 있다. 반면에 배열에서는 삽입 또는 삭제 후에 데이터를 재배치해야 하므로 더 많은 시간이 소요될 수 있다.

**BUT**, 이러한 장점들은 일부 상황에서만 적용되며, Linked List가 모든 상황에서 배열보다 뛰어나다는 것은 아니다. 

예를 들어, Linked List에서 특정 인덱스의 데이터에 접근하려면 리스트를 처음부터 순회해야 하므로, 이러한 접근은 배열에 비해 비효율적일 수 있다. 또한, Linked List의 각 노드는 데이터와 함께 다음 노드를 가리키는 링크 정보를 저장해야 하므로, 이로 인한 추가 메모리 사용이 필요하다.
<br></br>

> **단점**
- **직접 접근 불가능**: 배열과는 달리 Linked List는 인덱스를 이용한 직접적인 데이터 접근이 불가능하다. 즉, 특정 인덱스의 요소에 접근하려면 Linked List의 처음부터 해당 인덱스까지 순차적으로 이동해야 한다. 이로 인해 데이터 검색에 있어서 비효율적일 수 있다.
- **메모리 오버헤드**: Linked List의 각 노드는 데이터 필드와 함께 링크(참조) 필드를 가지고 있어야 한다. 이 링크 필드는 추가적인 메모리를 사용하므로, 링크드 리스트는 배열에 비해 메모리 오버헤드가 크다고 볼 수 있다.
- **역방향 탐색의 어려움**: 일반적인 단방향 연결 리스트에서는 노드들이 다음 노드만을 참조하므로, 리스트를 역방향으로 탐색하는 것이 어렵다. 이 문제는 양방향 연결 리스트를 사용하면 해결할 수 있지만, 그러면 또 다른 메모리 오버헤드가 발생하게 된다.
- **복잡한 구현**: 배열에 비해 Linked List의 구현은 복잡하다. 특히, 노드의 삽입, 삭제 시에 링크를 정확히 관리해야 하므로 버그가 발생할 가능성이 더 높다.
<br></br>

---
## 구현 by Python

```python
# 노드 클래스 정의
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

# 링크드 리스트 클래스 정의
class LinkedList:
    def __init__(self):
        self.head = None

    def append(self, data):
        if not self.head:
            self.head = Node(data)
        else:
            current = self.head
            while current.next:
                current = current.next # current.next가 존재하지 않을 때 까지 next를 한다.
            current.next = Node(data)

    def print_list(self):
        current = self.head
        while current:
            print(current.data)
            current = current.next

# 링크드 리스트 객체 생성 및 데이터 추가
linked_list = LinkedList()
linked_list.append('A')
linked_list.append('B')
linked_list.append('C')

# 링크드 리스트 데이터 출력
linked_list.print_list()  # 출력: A, B, C
```


1. **Node 클래스**: 각 노드를 나타내는 클래스이다. 각 노드는 data와 next라는 두 가지 속성을 가진다. data는 노드에 저장된 데이터를 나타내며, next는 다음 노드를 가리키는 링크다. 초기에는 next는 None으로 설정된다.
2. **LinkedList 클래스**: 이 클래스는 링크드 리스트 자체를 표현한다. head 속성은 리스트의 첫 번째 노드를 가리킨다. 초기에는 head가 None으로 설정된다.
3. **LinkedList의 append 메소드**: 이 메소드는 링크드 리스트의 끝에 새로운 노드를 추가한다. 만약 리스트가 비어 있다면(head가 None이라면), head를 새 노드로 설정한다. 그렇지 않다면, 리스트의 끝을 찾아서(current.next가 None이 될 때까지) 그곳에 새 노드를 추가한다.
4. **LinkedList의 print_list 메소드**: 이 메소드는 리스트의 모든 노드의 데이터를 출력한다. head부터 시작해서 next를 이용해 리스트의 끝까지 이동하면서 각 노드의 data를 출력한다.
5. **링크드 리스트 사용 예제**: LinkedList 객체를 생성하고, append 메소드를 이용해 여러 데이터('A', 'B', 'C')를 리스트에 추가한다. 그리고 print_list 메소드를 이용해 리스트의 모든 데이터를 출력한다. 출력 결과는 'A', 'B', 'C' 순서대로 나타낸다.
<br></br>

### **삽입, 삭제**

```python
class Node:
    def __init__(self, data):
        self.data = data  # 노드가 저장하는 데이터
        self.next = None  # 다음 노드를 가리키는 포인터

class LinkedList:
    def __init__(self):
        self.head = None  # 링크드 리스트의 첫 번째 노드를 가리키는 포인터

    def append(self, data):  # 새로운 노드를 리스트의 끝에 추가
        if not self.head:  # 만약 리스트가 비어있으면
            self.head = Node(data)  # 새 노드를 head로 지정
        else:  # 리스트가 비어있지 않으면
            current = self.head
            while current.next:  # 리스트의 끝을 찾아가는 루프
                current = current.next
            current.next = Node(data)  # 리스트의 끝에 새 노드 추가

    def print_list(self):  # 리스트의 모든 노드를 출력
        current = self.head
        while current:
            print(current.data)
            current = current.next
            
    def insert(self, data, position):  # 새로운 노드를 리스트의 특정 위치에 삽입
        new_node = Node(data)  # 새 노드 생성
        if position == 0:  # 만약 맨 앞에 삽입하는 경우
            new_node.next = self.head  # 새 노드의 다음 노드를 현재의 head 노드로 지정
            self.head = new_node  # 새 노드를 head 노드로 지정
        else:
            current = self.head
            for _ in range(position - 1):  # 삽입할 위치의 앞 노드를 찾는 루프
                if current:
                    current = current.next
                else:
                    raise IndexError('Position out of range')  # 삽입 위치가 리스트의 길이를 초과하면 예외 발생
            if current is None:
                raise IndexError('Position out of range')  # 삽입 위치가 리스트의 길이를 초과하면 예외 발생
            new_node.next = current.next  # 새 노드의 다음 노드를 삽입 위치의 노드로 지정
            current.next = new_node  # 삽입 위치의 앞 노드의 다음 노드를 새 노드로 지정

    def delete(self, data):  # 주어진 값을 가진 노드를 리스트에서 삭제
        if self.head and self.head.data == data:  # 삭제할 노드가 head 노드인 경우
            self.head = self.head.next  # head 노드를 다음 노드로 지정
        else:
            current = self.head
            while current and current.next and current.next.data != data:  # 삭제할 노드를 찾는 루프
                current = current.next
            if current and current.next:  # 삭제할 노드를 찾은 경우
                current.next = current.next.next  # 삭제할 노드 앞의 노드의 다음 노드를 삭제할 노드의 다음 노드로 지정
            else:
                raise ValueError('Value not found in the list')  # 삭제할 노드를 찾지 못한 경우 예외 발생
```

- **고려해야 하는 case**
1. 헤드 부분에 추가, 삭제
2. 중간 부분에 추가, 삭제
3. 꼬리 부분에 추가, 삭제
<br></br>

- **insert method**
    
    : 새 노드를 주어진 위치에 삽입한다. 
    
    - 위치가 0이라면 새 노드를 리스트의 앞에 삽입
    - 그렇지 않으면 주어진 위치의 앞에 삽입
    - 주어진 위치가 리스트의 길이를 초과하면 IndexError를 발생

- **delete method**
    
    : 주어진 값을 가진 노드를 삭제한다. 
    
    - 삭제할 노드가 리스트의 앞에 있다면 head를 다음 노드로 이동
    - 그렇지 않으면 삭제할 노드의 앞 노드의 next를 삭제할 노드의 다음 노드로 변경
    - 주어진 값이 리스트에 없으면 ValueError를 발생
<br></br>

> *사용 예시*
> 

```python
linked_list = LinkedList()

linked_list.append('A')  # 노드 'A' 추가
linked_list.append('B')  # 노드 'B' 추가
linked_list.append('C')  # 노드 'C' 추가
linked_list.append('D')  # 노드 'D' 추가

linked_list.print_list()
# Output: A B C D

linked_list.insert('E', 0)  # 'E' 노드를 맨 앞에 삽입
linked_list.print_list()
# Output: E A B C D

linked_list.insert('F', 2)  # 'F' 노드를 index 2 (세 번째 위치)에 삽입
linked_list.print_list()
# Output: E A F B C D

linked_list.delete('B')  # 'B' 노드를 삭제
linked_list.print_list()
# Output: E A F C D

linked_list.delete('E')  # 'E' 노드를 삭제
linked_list.print_list()
# Output: A F C D
```
<br></br>

---
> **➕ Java에서의 ArrayList와 LinkedList**
> 

Java에서 LinkedList는 ArrayList와 같이 인덱스로 접근하여 조회나 삽입이 가능하지만, 내부 구조는 전혀 다르다. ArrayList는 내부적으로 배열을 이용하여 메서드로 조작을 할 수 있도록 만든 컬렉션이라면, LinkedList는 노드(객체)끼리의 주소 포인터를 서로 가리키며 링크(참조)함으로써 이어지는 구조이다.

![image](https://github.com/user-attachments/assets/6a1dae7f-a74e-44de-862a-62ca627b71d0)

위의 그림에서, LinkedList는 각각의 노드가 화살표로 연결되어 리스트 형태로 나열되어 있다. 여기서 노드를 하나의 객체로 보면 된다. 즉, 객체를 만들면 객체의 주소가 생기게 되는데, 이 주소를 각 노드가 서로 참조하여 연결 형태를 구성하게 되는 것이다.

[🧱 자바 LinkedList 구조 & 사용법 - 정복하기](https://inpa.tistory.com/entry/JAVA-%E2%98%95-LinkedList-%EA%B5%AC%EC%A1%B0-%EC%82%AC%EC%9A%A9%EB%B2%95-%EC%99%84%EB%B2%BD-%EC%A0%95%EB%B3%B5%ED%95%98%EA%B8%B0)

---
### 질문
1. Linked List 의 장단점에 대해 설명해주세요.
2. Linked List 와 Array(배열)을 비교해주세요.
