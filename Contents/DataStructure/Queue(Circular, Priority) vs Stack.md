# Queue는 어떤 자료구조인가?

## ✅ 핵심 개념
Queue는 **선입선출(FIFO: First In First Out)**하는 자료구조입니다.  

---

## Queue의 특징(FIFO)
-핵심 특징 : **선입선출 FIFO(First In First Out)** 형식으로 데이터를 저장

![image](https://github.com/user-attachments/assets/9e66cab1-d882-4fa8-98cf-e1c9f1d02094)

1. 구조 및 함수
   - `front`   : 맨 앞의 위치(인덱스)
   - `rear`    : 맨 뒤의 위치(인덱스)
   - `Enqueue` : 데이터를 추가
   - `Dequeue` : 데이터를 추출
   - `Peek`    : front에 위치한 데이터를 읽기

---

## Queue종류(Array-base, List-base, Circular) 및 시간복잡도

### ✅ 1. Array-base Queue vs List-base Queue
|     | Array-base | List-base |
| --- | -----------|-----------|
| 메모리 | 크기가 고정(가득 차면 크기를 조정해야 함)| 크기가 동적(재할당과 메모리 낭비에 대한 걱정이 없음)|
| 시간 복잡도 | **`O(1)`** (큐의 모든 요소에 대한 상수 시간 액세스) | **`O(1)`** (단, 큐의 중간 요소에는 보다 액세스가 느림) |

 => Array-base Queue는 **메모리 낭비를 줄이기 위해서 Circular Queue형식으로 구현**한다.

### 2. Circular Queue
✅ 핵심 특징 : **양쪽에서 enqueue, dequeue가 가능**
- 활용 예시 : 프린터, CPU task scheduling,Cache 구현, 너비 우선 탐색(BFS) 
![image](https://github.com/user-attachments/assets/e1442728-ae1e-44fd-8bb0-1cc5b9463f01)


---
<br><br><br>


# Stack은 어떤 자료구조인가?
`stack`은 **후입선출(LIFO: Last In First Out)**하는 자료구조입니다. 

---

## 💡 면접 TIP
1. `stack`은 **후입선출(LIFO: Last In First Out)**하는 자료구조입니다. 

---

## Stack의 특징(LIFO) 및 시간복잡도
-핵심 특징 : **후입선출 LIFO(Lirst In First Out)** 형식으로 데이터를 저장

![image](https://github.com/user-attachments/assets/2d83f90d-d986-42dc-86d2-d1717271a441)

1. 구조 및 함수
   - **top**  : 마지막 데이터의 위치(인덱스)
   - **Push** : 데이터를 추가(**`O(1)`**)
   - **Pop**  : 데이터를 추출(**`O(1)`**)
   - **Peek** : top의 데이터 읽기

2. 특징
   - 재귀적인 특징이 있다.

3. 활용 예시
   - call stack, 후위 표기법 연산, 웹 브라우저 방문기록(뒤로 가기), DFS
  
---
<br><br><br>
# Priority Queue는 어떤 자료구조인가?

## ✅ 핵심 개념
-들어간 순서에 상관없이 **우선순위가 _높은_ 데이터가 먼저 나옵니다.**
- Priority queue의 시간복잡도는 **`enqueue O(log n), dequeue O(log n)`**입니다.
  
---

## 구현 방법(Heap)
✅ **Heap으로 구현한다.**
> 각 구현 방식에 따른 시간 복잡도

| 구현 방법                   | enqueue() | dequeue() |
|-----------------------------|-----------|-----------|
| 배열 (unsorted array)       | O(1)      | O(N)      |
| 연결 리스트 (unsorted linked list) | O(1)      | O(N)      |
| 정렬된 배열 (sorted array)  | O(N)      | O(1)      |
| 정렬된 연결 리스트 (sorted linked list) | O(N)  | O(1)      |
| 힙 (heap)                   |**O(logN)** |**O(logN)**  |


0. 가정
   - 배열은 우선순위만을 갖는다.
   - 값이 클 수록 우선순위가 높다. (최대 힙)
   - 루트 노드는 인덱스 1에 위치한다.
```
struct node {
  int data;
  int priority;
}
```

1. Peek()
  - 최대 우선순위 값을 반환한다.
     -> 이는 항상 루트에 있으므로 추트 값을 반환한다.
```
int peek(int arr[]) {
  return arr[1];
}
```

2. Enqueue()
<img width="802" alt="image" src="https://github.com/user-attachments/assets/9e7883cf-a330-433c-86ab-0ad8f4b3ebba" />

- 새로운 데이터를 추가한다.
- 추가 방법
   1. 힙 끝에 새 요소를 삽입한다.
   2. if) 부모 노드와 비교하여 힙 속성을 위배하는 경우 -> 부모 노드와 값을 바꾼다.
   3. for) 힙 속성이 유지될 때까지 2번 작업을 반복한다.
      
```
void enqueue(int arr[], int val)
{
  int i = 0; 
  if (size == MAX_LEN) { // 배열에 공간이 없으면 실패
    printf("Overflow: Could not insertKey\n");
  }
  
  // 힙 끝에 요소 삽입.
  size ++;
  i = size;
  arr[i]= val;

  // 우선순위가 부모 노드가 더 작다면 교환하고 부모 노드부터 다시 비교, 힙속성을 유지할 때까지 반복함.
  while(i > 1 && arr[i/2] < arr[i]) {
    swap(arr[i/2], arr[i]);
    i = i/2;
  }
}
```

3. Dequeue()
<img width="869" alt="image" src="https://github.com/user-attachments/assets/851f15b8-b5da-47fa-a81b-959dbe63eaa6" />

- 루트 노드(우선 순위가 가장 높은 노드)의 값을 추출한다.
- 삭제 방법
   1. **heap 마지막 요소를 루트 노드에 배치**한다.
   2. **마지막 요소는 제거**(기존 루트 노드)한다.
   3. 루트 노드 위치부터 Heapify()를 수행한다.
      
```
int dequeue (int arr[]) {
  if(size == 0) {
    printf("empty\n");
  }
  
  // 루트 노드 값을 리턴값에 저장한 뒤, 마지막 요소를 루트노드에 배치함
  Node max = arr[1];
  arr[1] = arr[size];
  size --;

  // 루트 노드부터 heapify 수행
  max_heapify(arr, 1);
  
  return max;
}
```
 
4. Heapify()
 <img width="823" alt="image" src="https://github.com/user-attachments/assets/5e7d2245-63e3-44ac-bfeb-b800f01950b3" />
 
- **힙 속성을 유지**한다. **위에서 아래로**내려가면서 작업한다.
- 방법
  1. 자식 노드와 우선순위를 비교한다.
  2. if) 자식 노드 우선순위가 높다면 -> 왼쪽 오른쪽 자식 중 가장 우선순위가 높은 노드와 교환한다.
  3. for) 힙 속성이 유지 될 때까지 1,2번 과정을 반복한다.
      
```
void max_heapify (int arr[], int i)
{
  int largest = i;  
  int left = 2*i              //left child
  int right = 2*i +1          //right child
  
  // 현재 요소 i와 자식 노드의 값을 비교
  if(left <= size && arr[left] > arr[i] )
    largest = left;  
  if(right <= size && arr[right] > arr[largest] )
    largest = right;
  
  // 자식 노드의 값이 더 크다면 교환하고 교환된 자식 노드부터 heapify 진행
  if(largest != i ) {
    swap (arr[i] , arr[largest]);
    max_heapify (h, largest);
  } 
}
```

---
### 활용 예시
- CPU 스케줄링, Dijkstra's shortest path algorithm, Prim's Mininum Spanning tree, 우선순위를 포함한 모든 queue 응용

---

### 💡면접 대비 핵심 포인트💡
## 1. **Queue vs Stack**의 비교되는 특징(활용 예시 포함)
## 2. Circular queue 자료구조의 특징
## 3. Priority queue의 **구현 방법**(Heap의 이진 완전 트리를 활용)과 operation의 **시간복잡도( `push O(log n), pop O(log n)`)** (및 삽입 삭제 과정)


### 💡꼬리 질문
## 1. Array-base, List-base 의 차이점
## 2. Queue 2개로 Stack 구현하기, Stack 2개로 Queue 구현하기
     

