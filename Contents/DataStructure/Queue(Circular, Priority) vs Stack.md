# Queue는 어떤 자료구조인가?

## 핵심 개념
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

## 💡면접 대비 핵심 포인트💡
1. **Queue vs Stack**의 비교되는 특징(활용 예시 포함)
2. Circular queue 자료구조의 특징


## 💡꼬리 질문
1. Array-base, List-base 의 차이점
2. Queue 2개로 Stack 구현하기, Stack 2개로 Queue 구현하기
     

### 활용 예시
 -> 
