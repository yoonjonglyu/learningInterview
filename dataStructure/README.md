# Data Structure

- Array vs Linked List
- Stack & Queue
- Tree
- Hash Table

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

## Tree

> 트리는 비선형 자료구조 중 하나이다.계층적 관계(Hierarchical Relationship)를 표현한다.
> 트리는 Node(노드), Edge(간선), Root Node(루트 노드), Terminal Node(left, 단말노드), Internal Node(내부 노드, 비단말) 로 구성 되어있다.

### Binary Tree(이진 트리)

1. 루트 노드 중심으로 두개의 서브 트리를 가지고 나머지 서브 트리들도 모두 이진 트리인 트리, 공집합도 포함하여 노드가 하나인 트리도 이진트리다.
2. 트리에서의 각 층은 Level이라고 부르고 0부터 시작한다. 루트가 0이며 해당 트리 가장 최고 레벨을 height라고 한다.
3. 이진 트리의 종류는 여러가지 있는데 Perfect Binary Tree (포화 이진 트리), Complete Binary Tree (완전 이진 트리), Full Binary Tree (정 이진 트리)등이 있다.
4. 포화 이진 트리는 모든 레벨이 가득찬 이진 트리를 말한다.
5. 완전 이진 트리는 위에서 아래로, 왼쪽에서 오른쪽으로 순서대로 차곡차곡 채워진 이진 트리를 말한다.
6. 정 이진 트리는 모든 노드가 0개 혹은 2개의 자식 노드를 갖는 이진트리를 말한다.
7. 이진 트리는 노드의 개수가 n 개이고 root가 0이 아닌 1에서 시작할 때, i 번째 노드에 대해서 parent(i) = i/2 , left_child(i) = 2i , right_child(i) = 2i + 1 의 index 값을 갖는다.

### BST(Binary Search Tree)

1. 효율적인 데이터 탐색을 위한 자료구조 중 하나이다. 이진 트리의 일종이고 특정한 규칙에 따라서 데이터를 저장한다.
2. 규칙 1 : 트리의 노드에 저장된 키는 유일하다.(unique key), 규칙 2 : 부모키가 왼쪽 자식 노드 키 보다 크다, 규칙 3 : 부모의 키가 오른쪽 자식 노드 키 보다 작다, 규칙 : 4 왼쪽과 오른쪽 서브 트리도 BST다.
3. 정리하면 이진 탐색트리는 unique key, L < P < R, 재귀 구조인 이진 트리이다.
4. BST의 탐색 연산은 보통O(log n)의 시간 복잡도(또는 O(h))를 갖는다.
5. 트리 h가 하나씩 늘어 갈 수록 추가 가능한 노드 수는 2배씩 증가한다.
6. 저장 순서에 따라 계속 한 쪽으로만 노드가 추가 되는 경우 Skewed Tree(편향 트리)라 부르며 탐색의 Worst Case는 O(n)
7. BST는 배열보다 많은 메모리를 사용하고, 편향 트리가 될 경우 시간복잡도가 동일해지기에 이걸 막기 위해서 Rebalancing이라는 트리 구조 조정 기법이 존재한다.

### Binary Heap

1. 배열에 기반한 완전 이진 트리(Complate Binary Tree) 이다. 배열의 루트는 0이 아닌 1에서 시작한다.(key === index 목적)
2. 힙에는 최대 힙(max heap), 최소 힙(min heap) 두 종류가 있다.
3. Max Heap은 각 노드의 값이 해당 자식 노드의 값보다  크거나 같은 완전 이진트리, Min Heap은 그 반대다.
4. Max Heap은 Root Node에 있는 값이 가장 크기에 최대값 탐색 연산의 시간 복잡도는 O(1)이다, 배열 기반이라 Random Acces가 가능하다.
5. Min Heap은 Max Heap과 반대된다.
6. 힙구조를 유지하기 위해서 Root Node를 제거 한다면 heap의 맨 마지막 노드로 Root를 대체 한 후 heapify를 거치며 시간복잡도는 O(log n)
7. 최대값, 최소값 접근 최종 시간 복잡도는 O(log n)

### RBT(Red Black Tree)

1. RBT는 BST 기반으로 한 자료구조, Search, Insert, Delete 시간복잡도는 O(log n) 동일한 노드 개수 일때 depth를 최소화 하는게 핵심
2. 1.RBT 완전 이진 트리는 각 노드가 Red or Black 이라는 색을 가진다. 2.루트 노드는 Black과 각 leaf node는 Black이다. 어떤 노드가 Red이면 그 자식은 모두 Black이다.
3. 각 노드에 대해서 노드로부터 descendant leaves 까지의 단순 경로는 모두 같은 수의 black nodes 들을 포함하고 있다. 이를 해당 노드의 Black-Height라고 한다. cf) Black-Height: 노드 x 로부터 노드 x 를 포함하지 않은 leaf node 까지의 simple path 상에 있는 black nodes 들의 개수
4. RBT BST의 특징을 모두 가지고 있고, Root 로 부터 left까지의 모든 경로 중 최소, 최대 경로의 크기 비율은 2보다 크지 않은 balanced 상태다.
5. RBT에서 자식이 없을시 자식을 가르키는 포인터는 NIL 값을 leaf로 삼는다.
6. Java Collection 에서 ArrayList 도 내부적으로 RBT 로 이루어져 있고, HashMap 에서의 Separate Chaining에서도 사용된다. 그만큼 효율이 좋고 중요한 자료구조이다.

#### 삽입

1. BST 특성을 유지하며 Red 노드를 삽입하며 Black-Height 변경을 최소화한다.
2. 삽입 결과 RBT 특성을 위배(violation)시 노드 색깔을 조정, B-H 위배시 rotation으로 조정하여 동일 H 상의 내부 노드 들의 B-H가 동일하게 유지한다.

### 삭제

1. BST 특성을 유지하며 노드를 삭제한다. node의 자식 갯수에 따라 rotation 방법이 상이하다.
2. Black node가 삭제 된 경우에만 RBT특성을 위배하기에 해당 노드 -1 경로에 black node를 하나 추가하도록 rotation한다.

## Hash Table

> 내부에서 배열을 사용하여서 저장하기에 빠른 탐색 속도를 가진다. average case(!collision)에 대한 시간복잡도는 O(1)
> Hash Table은 Hash Function을 사용하여 유니크 키(고유한 숫자)를 만들어낸 뒤 인덱스로 사용한다.

1. 삽입 연산과, 삭제시 추가적인 비용 없이 할 수 있는 구조이고, 데이터를 탐색할때 역시 추가적인 비용이 없는 것을 목표로 한다.
2. 자바스크립트의 경우 Object가 Hash Table과 같은 방식으로 만들어진다. 다만 hash Map이 가진 장점들이 없으니 서로 용도에 맞게 쓰자.

### Hash Function

1. Hash Function(Hash Method)에서 반환한 고유 숫자 값을 HashCode라고 지칭한다.
2. Hash Function은 저장 되는 key 값을 작은 범위의 값들로 바꿔준다.
3. 어설픈 Hash Function을 사용하여 서로 다른 키 값으로 같은 HashCode를 도출하는 경우를 Collision 이라고 한다. 동일한 키값에 복수 개의 데이터가 존재하는 것.
4. 좋은 Hash Function의 조건 : key 전부를 참조 해서 Hash, Collision 을 배제가 아닌 최소화 (메모리 문제)하는 것.

### Resolve Conflict

1. Open Address(개방 주소)방식 : 해시 충돌 발생시 다른 해시 버킷에 해당자료를 삽입하는 방법으로 선형 탐색(Linear Probing), 2차 탐색(Quadratic Probing), 더블 해싱 탐색(Double Hashing Probing)이 있다. 데이터를 저장할 버킷을 찾지 못하면 다시 탐색 시작 위치까지 돌아 올 수 있다.
2. 선형 조사 : 순차 탐색 비어 있는 버킷을 찾을때 까지 진행, 2차 탐색 : 2차 함수를 이용해 탐색할 위치를 찾기, 더블 해싱 탐색 : 2차 해쉬 함수를 이용해 새로운 주소를 할당 가장 연산량이 많음
3. Separate Chaining(분리 연결법) 방식 : 일반적으로 개방 주소 방식 보다 빠르다, 개방 주소 방식은 버킷 밀도가 높아질 수록 Worst Case 빈도가 높아지기 때문, 보조 해시 함수를 통해 조정이 가능하다면 Worst Case 빈도를 줄일 수 있는 방법으로 연결리스트, 트리 가 있다. 자바 7에서는 이 방식으로 Hash Map을 구현한다.
4. 연결리스트 : 각각의 버킷(bucket)을 연결리스트로 만들어 collision 발생시 해당 버킷 리스트에 추가한다. 리스트 방식의 장점인 삭제 및 삽입이 간단, 단점인 작은 데이터를 저장할때 리스트 자체의 오버헤드가 크다. 개방 주소 방식에 비해 테이블 확장을 늦출 수 있다.
5. 트리(RBT) : 분리연결법과 동일 연결리스트를 트리로 대체한다. 연결리스트와 트리의 분기점은 key-value 쌍의 개수이다. 적다면 연결리스트를 사용한다. 그 이유는 트리 구조의 메모리 사용량 데이터 갯수가 적을떄는 연결리스트 방식과 Worst Case 차이가 거의 없다.
6. 데이터가 적다는 기준은 하나의 버킷에 있는 key-value 쌍의 갯수가 6과 8일때 변경시 소요 되는 비용을 줄이는 목적이다.
7. 일단 두 방식 모두 Worst Case 에서 O(M)이다. 하지만 Open Address방식은 연속된 공간에 데이터를 저장하기 때문에 Separate Chaining에 비해 캐시 효율이 높다. 따라서 데이터의 개수가 충분히 적다면 Open Address방식이 Separate Chaining보다 더 성능이 좋다. 한 가지 차이점이 더 존재한다. Separate Chaining방식에 비해 Open Address방식은 버킷을 계속해서 사용한다. 따라서 Separate Chaining 방식은 테이블의 확장을 보다 늦출 수 있다.
8. 보조 해시 함수(Supplement Hash Function) : key의 HashCode를 변경하여 Collision 충돌 가능성을 줄이는 것. 분리 연결법에 보조로 사용.

### 해시 버킷 동적 확장(Resize)

1. 해시 버킷 갯수가 적으면 메모리 사용량을 줄일 수 있지만 해시 충돌로 성능 손실이 발생한다.
2. HashMap은 key-value 쌍 데이터가 일정 개수 이상이 되면 버킷 개수를 2배로 늘린다.
3. 리사이즈의 임계점은 해시버킷 갯수의 load factor다. load factor은 75% 즉 0.75.