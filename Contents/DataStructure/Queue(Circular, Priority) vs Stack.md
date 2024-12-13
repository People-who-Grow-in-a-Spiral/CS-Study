# Queue는 어떤 자료구조인가?

## 핵심 개념
Queue는 **선입선출(FIFO: First In First Out)**하는 자료구조입니다.  

---

## 💡면접 대비 핵심 포인트💡
1. **Queue vs Stack**의 비교되는 특징
2. Circular queue 자료구조의 특징


## 꼬리 질문
1. Array-base, List-base 의 차이점

---

## Queue의 특징(FIFO)
-핵심 특징 : **선입선출 FIFO(First In First Out)** 형식으로 데이터를 저장한다.

![image](https://github.com/user-attachments/assets/9e66cab1-d882-4fa8-98cf-e1c9f1d02094)

1. `front`   : 큐의 맨 앞의 위치(인덱스)
2. `rear`    : 큐의 맨 뒤의 위치(인덱스
3. `enqueue` : 큐에서 데이터를 추가하는 것
4. `dequeue` : 큐에서 데이터를 추출하는 것
5. `peek`    : front에 위치한 데이터를 읽는 것

---

## Queue종류(Array-base, List-base, Circular)

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

# Queue의 기본 개념

`queue`는 선입선출(FIFO: First In First Out)의 자료구조입니다.  
시간복잡도는 `enqueue O(1)`, `dequeue O(1)` 입니다.  
활용 예시는 Cache 구현, 프로세스 관리, 너비우선탐색(BFS) 등이 있습니다.

---

## 💡 면접 TIP
> 기초적인 자료구조로 면접에서 간단한 질문으로 종종 나오는 문제입니다.  
> LIFO인 stack과 다르게 FIFO 자료구조임을 잘 기억하시고 계시면 됩니다.  
> 또한 활용 예시들과 Circular Queue 자료구조를 잘 알고 계시면 면접에서 충분한 답을 하실 수 있습니다.

---

## FIFO

`queue`는 시간 순서상 먼저 집어 넣은 데이터가 먼저 나오는  
선입선출 FIFO(First In First Out) 형식으로 데이터를 저장하는 자료구조입니다.

```plaintext
dequeue O(1)                         enqueue O(1)
        [1][2][3][4][5][6]
         First In           First Out
