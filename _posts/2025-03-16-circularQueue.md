---
title: "25.03.16 / 원형 큐(Circular Queue)"
excerpt: ""

categories:
  - Programming
  - Data Structure and Algorithm
tags:
  - []

toc: true
toc_sticky: true

date: 2025-03-16
last_modified_at: 2025-04-25
---

저번 글에서 알아본 큐는 선형 큐였다. 선형 큐에서는 배열의 처음과 끝이 고정되어 있어서, 원소를 계속 삽입하고 삭제하다 보면 비어 있는 공간이 있더라도 배열의 끝 부분에 더 이상 원소를 삽입할 수 없는 문제가 발생한다. 하지만 원형 큐를 사용하면 Front나 Rear가 배열의 끝에 도달했을 때 다시 배열의 처음으로 돌아와 빈 공간을 활용할 수 있게 된다

<div style="display: flex; gap: 1rem; margin-bottom: 1rem;">
  <img src="/assets/img/250316_circularQueue/01.png" alt="01" style="max-width: 100%" />
</div>
<div style="display: flex; gap: 1rem; margin-bottom: 1rem;">
  <img src="/assets/img/250316_circularQueue/02.png" alt="02" style="max-width: 100%;" />
</div>

선형 큐를 원형 큐로 바꾼 모양이다.

이번 글에선 원형 큐를 1차원 배열을 이용해 만들어보겠다.

```c
#define QUEUE_SIZE 5

typedef struct {
    int queue[QUEUE_SIZE];
    int front;
    int rear;
} CircularQueue;
```

queue 배열과 front, rear를 구조체로 묶어 관리한다. 우선 front와 rear를 초기화 해줘야 한다.

```c
void InitQueue(CircularQueue *_cq){
    _cq->front = 0;
    _cq->rear = 0;
}
```

그럼 이제 Enqueue를 하거나 Dequeue를 하기 위해 배열이 가득 찼는지, 텅 비어있는지 확인해야한다. 가득 차 있으면 Enqueue를 못할거고, 텅 비어있으면 Dequeue를 못할거다.

```c
#define TRUE 1
#define FALSE 0
#define QUEUE_SIZE 5

int CqEmpty(CircularQueue* _cq) {
    if (_cq->front == _cq->rear) return TRUE;
    else return FALSE;
}

int CqFull(CircularQueue* _cq) {
    if (((_cq->rear + 1) % QUEUE_SIZE) == _cq->front) return TRUE;
    else return FALSE;
}
```

front와 rear가 같다면, 즉 enqueue가 한번도 일어나지 않았다면 텅 비어있다고 가정한다. 반대로 rear에 1을 더해서 QUEUE_SIZE로 나눈 나머지가 front가 된다면, 즉 이미 배열의 크기만큼 enqueue가 반복됐다면 가득 찼다고 가정한다.

이제 데이터를 삽입(Enqueue)하고 삭제(Dequeue)해보자.

```c
void Enqueue(CircularQueue* _cq, int _data) {
    if (CqFull(_cq)) {
        printf("Queue is Full!\n");
        return;
    }
    _cq->rear = (_cq->rear + 1) % QUEUE_SIZE;
    _cq->queue[_cq->rear] = _data;
}

int Dequeue(CircularQueue* _cq) {
    if (CqEmpty(_cq)) {
        printf("Queue is Empty!\n");
        return -1;
    }

    _cq->front = (_cq->front + 1) % QUEUE_SIZE;
    return _cq->queue[_cq->front];
}
```

데이터를 삽입할 땐 우선 큐가 가득 차있는지 확인하고, 가득 찼으면 삽입 불가 메세지를 출력한다. 가득 차지 않았다면 rear를 한 칸 옮긴 뒤 그 자리에 값을 삽입한다.

데이터를 삭제할 땐 우선 큐가 텅 비어있는지 확인하고, 비어있으면 삭제 불가 메세지를 출력하고 오류코드 -1을 반환한다. 비어있지 않다면 front를 한 칸 옮긴 뒤 그 자리에 값을 반환한다.

이제 코드가 잘 작동하는지 확인하기 위해 출력하는 함수도 만들어보겠다.

```c
void PrintQueue(CircularQueue* _cq) {
    printf("Queue : ");
    int i = (_cq->front + 1) % QUEUE_SIZE;
    while (i != (_cq->rear + 1) % QUEUE_SIZE) {
        printf("%d ", _cq->queue[i]);
        i = (i + 1) % QUEUE_SIZE;
    }
    printf("\n");
}
```

front 다음 칸부터 rear까지 배열의 원소를 순회하며 출력한다.

전체적인 실행 결과를 알아보자.

```c
#include <stdio.h>
#include <stdlib.h>

#define QUEUE_SIZE 5
#define TRUE 1
#define FALSE 0

typedef struct {
    int queue[QUEUE_SIZE];
    int front;
    int rear;
} CircularQueue;

void InitQueue(CircularQueue* _cq) {
    _cq->front = 0;
    _cq->rear = 0;
}
int CqEmpty(CircularQueue* _cq) {
    if (_cq->front == _cq->rear) return TRUE;
    else return FALSE;
}

int CqFull(CircularQueue* _cq) {
    if (((_cq->rear + 1) % QUEUE_SIZE) == _cq->front) return TRUE;
    else return FALSE;
}

void Enqueue(CircularQueue* _cq, int _data) {
    if (CqFull(_cq)) {
        printf("Queue is Full!\n");
        return;
    }
    _cq->rear = (_cq->rear + 1) % QUEUE_SIZE;
    _cq->queue[_cq->rear] = _data;
}

int Dequeue(CircularQueue* _cq) {
    if (CqEmpty(_cq)) {
        printf("Queue is Empty!\n");
        return -1;
    }

    _cq->front = (_cq->front + 1) % QUEUE_SIZE;
    return _cq->queue[_cq->front];
}

void PrintQueue(CircularQueue* _cq) {
    printf("Queue : ");
    int i = (_cq->front + 1) % QUEUE_SIZE;
    while (i != (_cq->rear + 1) % QUEUE_SIZE) {
        printf("%d ", _cq->queue[i]);
        i = (i + 1) % QUEUE_SIZE;
    }
    printf("\n");
}

int main(void) {
    CircularQueue cq;
    InitQueue(&cq);

    Enqueue(&cq, 10);
    Enqueue(&cq, 20);
    Enqueue(&cq, 30);
    PrintQueue(&cq);

    printf("Dequeue: %d\n", Dequeue(&cq));
    PrintQueue(&cq);

    Enqueue(&cq, 40);
    Enqueue(&cq, 50);
    PrintQueue(&cq);

    Enqueue(&cq, 60);
    PrintQueue(&cq);

    printf("Dequeue: %d\n", Dequeue(&cq));
    printf("Dequeue: %d\n", Dequeue(&cq));
    PrintQueue(&cq);

    return 0;
}
```

```text
실행 결과

Queue : 10 20 30
Dequeue: 10
Queue : 20 30
Queue : 20 30 40 50
Queue is Full!
Queue : 20 30 40 50
Dequeue: 20
Dequeue: 30
Queue : 40 50
```
