# Data Structure

- Array vs Linked List
- Stack & Queue

## Array vs Linked List

> Array는 논리적 저장순서와 물리적 저장순서가 일치하는 자료형. index를 알면 O(1)로 탐색(random access)이 가능하다. 크기가 고정된 자료구조
> Linked List는 논리적 저장순서와 물리적 저장순서가 일치하지 않는 자료형, 각 노드는 다음 노드의 주소만 가지고 있다.
> Array는 탐색에 유리하고 Linked List는 데이터의 삽입과 삭제에 유리하다

1. Array는 탐색과정은 O(1)로 처리가 가능하지만 해당 데이터를 삭제하거나 삽입을 할 경우 다른 데이터들을 조정해주는 작업이 필요하기에 최대 O(n)의 시간복잡도를 가진다.
2. Linked는 자기 다음 노드에 대한 주소만 기억하고 있기에 데이터를 삽입하거나 삭제할때 그냥 해당 노드에 관련된 부분만 처리하면 되니 O(1)이 가능하지만 그 데이터를 찾기 위해서 첫번째 원소부터 확인하기에 최대 O(n)의 시간복잡도를 가진다.
3. Array list는 Array 구조에 list이 특성을 추가한 것이다.
4. Linked List는 tree구조의 근간이 된다.

## Stack & Queue

> 선형 자료구조는 데이터를 순차적으로 나열시킨 자료구조, 비선형 자료구조는 하나의 데이터 뒤에 여러개의 데이터가 존재가능한 자료구조.
> 스택과 큐는 선형 자료구조 일종이다. Stack은 LIFO, Queue는 FIFO이 그 특징
> Stack은 pop, push, stackTop, Queue는 Enqueue(push), Dequeue(shift), front

1. 스택에 데이터가 넘치는 스택오버플로우, 데이터가 없는데 꺼내오는 스택언더플로우
2. 큐에서 자주쓰이는 순환 큐, 힙, 우선 순위 큐(Priority queue), 큐의 끝은 rear, 시작은(front) head