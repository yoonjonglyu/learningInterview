# Data Structure

- Array vs Linked List
- Stack & Queue
- Tree

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
