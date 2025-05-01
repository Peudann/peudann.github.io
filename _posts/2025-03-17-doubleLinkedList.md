---
title: "25.03.17 / 더블 링크드 리스트(Double Linked List)"
excerpt: ""

categories:
  - Programming
  - Data Structure and Algorithm
tags:
  - []

toc: true
toc_sticky: true

date: 2025-03-17
last_modified_at: 2025-04-29
---

더블 링크드 리스트는 링크드 리스트의 탐색 기능을 개선한 자료구조이다. 링크드 리스트에서 어떤 노드를 찾으려면 헤드에서 테일 방향으로만 탐색할 수 있는 데 비해, 더블 링크드 리스트에서는 양방향으로 탐색이 가능하다.

링크드 리스트의 노드는 다음 노드를 가리키는 포인터만 가지는 데 비해, 더블 링크드 리스트의 노드는 자신의 이전에 있는 노드를 가리키는 포인터도 가지고 있다. 그래서 더블 링크드 리스트의 노드는 뒤로는 물론 앞으로도 이동할 수 있다.

<div style="display: flex; gap: 1rem; margin-bottom: 1rem;">
  <img src="/assets/img/250317_doubleLinkedList/01.png" alt="01" style="max-width: 100%;" />
</div>

이를 구조체로 나타내면 다음과 같다.

```c
typedef struct _SNode {
    int data;
    struct _SNode* pPrev;
    struct _SNode* pNext;
} SNode;
```

그리고 이 노드를 계속 엮으면 다음과 같은 더블 링크드 리스트가 되는 것이다.

<div style="display: flex; gap: 1rem; margin-bottom: 1rem;">
  <img src="/assets/img/250317_doubleLinkedList/02.png" alt="02" style="max-width: 100%;" />
</div>

더블 링크드 리스트의 주요 연산은 링크드 리스트와 다른 것이 하나도 없다. 이전 노드를 처리하기 위한 구현이 더 추가될 뿐이다.

이번 글에선 가장 앞의 Head 노드와 가장 끝의 Tail 노드를 의미가 없는 값을 가진 전역변수로 선언하고 더블 링크드 리스트를 구현해보겠다.

```c
typedef struct _SNode {
    int data;
    struct _SNode* pPrev;
    struct _SNode* pNext;
} SNode;

// 값이 의미가 없는 헤드와 테일을 미리 만들고 구현해보겠다
SNode g_head;
SNode g_tail;

void main() {
	// 더블 링크드 리스트(Double Linked-List)
	// 다음 노드와 이전 노드를 가리키는 포인터 2개가 들어간다

	// 맨 처음에 헤드와 테일을 연결해주고 시작한다
	g_head.data = -1;
	g_head.pNext = &g_tail;
	g_head.pPrev = NULL;

	g_tail.data = -1;
	g_tail.pPrev = &g_head;
	g_tail.pNext = NULL;
}
```

헤드와 테일이 양방향으로 연결된 더블 링크드 리스트를 만들었으니 노드를 만들어 여러 연산을 해보자.

아래는 새로운 노드를 만드는 CreateNode 함수다.

```
SNode* CreateNode(const int _data, SNode* _pPrev, SNode* _pNext) {
	SNode* pNewNode = (SNode*)malloc(sizeof(SNode));
	pNewNode->data = _data;
	pNewNode->pPrev = _pPrev;
	pNewNode->pNext = _pNext;

	return pNewNode;
}
```

데이터와 이전 노드, 다음 노드를 입력받아 새로 동적 할당한 pNewNode에 data, pPrev, pNext 멤버에 각각 대입한다. 그리고 pNewNode를 반환한다.

아래는 데이터를 매개변수로 받아 Head 다음에 새로운 노드를 생성하는 PushFront 함수다.

```c
// Head 다음에 새로운 노드 생성
void PushFront(const int _data) {
	SNode* pNewNode = CreateNode(_data, NULL, NULL);

	pNewNode->pNext = g_head.pNext;
	g_head.pNext = pNewNode;

	pNewNode->pNext->pPrev = pNewNode;
	pNewNode->pPrev = &g_head;
}
```

\_data를 입력받아 CreateNode를 이용해 새로운 노드를 만든다. 그 이후

pNewNode->pNext가 기존 g_head의 다음 노드를 가리키도록 한다. 그리고 g_head의 다음 노드를 pNewNode로 수정한다. 이 과정에서 g_head의 다음 노드를 pNewNode로 먼저 바꿔버리면 기존에 g_head가 가리키던 포인터가 없어져버려 pNewNode->pNext를 g_head.pNext와 연결할 수 없어지니 순서를 헷갈리지 말자.

그리고 pNewNode 다음 노드가 가리키는 이전 노드를 pNewNode로 수정하고 pNewNode의 이전 노드를 g_head의 주소로 수정한다. 이 과정은 순서가 바뀌어도 상관이 없다.

아래는 노드를 매개변수로 받아 Tail 앞에 새로운 노드를 생성하는 PushBack 함수다.

```c
// Tail 앞에 새로운 노드 생성
void PushBack(SNode* _pNewNode) {
	if (_pNewNode == NULL) return;

	_pNewNode->pPrev = g_tail.pPrev;
	g_tail.pPrev = _pNewNode;

	_pNewNode->pPrev->pNext = _pNewNode;
	_pNewNode->pNext = &g_tail;
}
```

입력받은 노드가 존재하지 않는다면 삽입도 불가능하기 때문에 함수를 반환한다. 위와 마찬가지이지만 순서를 반대로

\_pNewNode->pPrev가 기존 g_tail의 이전 노드를 가리키도록 한다. 그리고 g_tail의 이전 노드를 \_pNewNode로 수정한다. 마찬가지로 이 과정에서 순서를 바꿔버리면 기존에 g_tail이 가리키던 포인터가 없어져버려 \_pNewNode->pPrev를 g_tail.pPrev와 연결할 수 없어지니 순서에 유의하자.

그리고 \_pNewNode의 이전 노드가 가리키는 다음 노드를 \_pNewNode로 수정하고 \_pNewNode의 다음 노드를 g_tail의 주소로 수정한다. 이 과정도 순서가 바뀌어도 상관 없다.

아래는 Head부터 Tail까지의 순서로 연결된 노드를 출력하는 함수다.

```c
void PrintForward() {
	if (g_head.pNext == &g_tail) {
		printf("[PrintForward] Empty!\n");
		return;
	}
	int cnt = 0;
	SNode* pIter = g_head.pNext;
	while (pIter != &g_tail) {
		printf("%d - ", pIter->data);
		pIter = pIter->pNext;
		++cnt;
	}
	printf("(%d)\n", cnt);
}
```

g_head의 다음 노드가 g_tail이면 삽입된 노드가 없다는 뜻이므로 리스트가 비어있다고 출력하고 함수를 반환한다.

임시로 값을 저장할 pIter 노드를 만들어 g_head.pNext를 대입한다. pIter가 g_tail이 아닌 동안(다음 노드가 g_tail이 아닌 동안) pIter->data를 출력하고 pIter를 연결된 다음 노드로 이동한다. 이 과정에서 cnt를 증가시켰는데, 이건 노드가 몇 개인지 세기 위한 부분이라 없어도 작동에는 문제가 없다.

아래는 Tail부터 Head까지의 순서로 연결된 노드를 출력하는 함수다.

```c
void PrintBackward() {
	if (g_tail.pPrev == &g_head) {
		printf("[PrintBackward] Empty!\n");
		return;
	}
	int cnt = 0;
	SNode* pIter = g_tail.pPrev;
	while (pIter != &g_head) {
		printf("%d - ", pIter->data);
		pIter = pIter->pPrev;
		++cnt;
	}
	printf("(%d)\n", cnt);
}
```

그냥 위의 PrintForward 함수를 단순히 다음 노드 부분을 이전 노드로 바꾸고 테일부터 헤드 방향으로 출력하는 코드다. 위와 진행 과정은 똑같다.

이번엔 중간에 노드를 삽입해보겠다. 넣을 위치의 인덱스와 데이터를 입력받아 노드를 만들어 삽입하는 함수다.

```c
void InsertWithData(int _idx, int _data) {
	SNode* pNewNode = CreateNode(_data, NULL, NULL);
	SNode* pIter = g_head.pNext;
	int cnt = 0;
	if (pIter == &g_tail) {
		printf("[InsertWithData] List Not Exist!\n");
		return;
	}
	while (pIter != &g_tail) {
		pIter = pIter->pNext;
		++cnt;
		if (cnt == _idx) {
			pNewNode->pNext = pIter;
			pNewNode->pPrev = pIter->pPrev;
			pNewNode->pPrev->pNext = pNewNode;
			pNewNode->pNext->pPrev = pNewNode;
		}
	}
	printf("[InsertWithData] [%d] : %d\n", _idx, _data);
}
```

마지막에 이전 노드, 다음 노드와 연결될 pNewNode와 반복문에서 임시로 사용할 pIter를 SNode 구조체 포인터로 선언했다. pIter가 g_tail가 동일하다면 리스트가 존재하지 않는다고 출력하고 함수를 종료한다. pIter가 g_tail가 다르다면 cnt를 1 증가시키고 pIter를 다음 노드로 이동시킨다. cnt가 입력받은 인덱스와 동일할 때 그 위치의 pIter를 pNewNode의 양 옆 노드에 연결해 pNewNode->pPrev, pNewNode, pNewNode->pNext 순으로 연결되도록 한다. 마지막으로 \_idx번째 인덱스에 \_data가 잘 들어갔음을 출력한다.

이번엔 중간에 미리 만들어진 노드를 삽입하는 함수다.

```c
void InsertWithNode(int _idx, SNode* _pNewNode) {
	SNode* pIter = g_head.pNext;
	int cnt = 0;
	if (pIter == &g_tail) {
		printf("[InsertWithNode] List Not Exist!\n");
		return;
	}
	while (pIter != &g_tail) {
		pIter = pIter->pNext;
		++cnt;
		if (cnt == _idx) {
			_pNewNode->pNext = pIter;
			_pNewNode->pPrev = pIter->pPrev;
			_pNewNode->pPrev->pNext = _pNewNode;
			_pNewNode->pNext->pPrev = _pNewNode;
		}
	}
	printf("[InsertWithNode] [%d] : %d\n", _idx, _pNewNode->data);

}
```

전반적인 구성은 위의 InsertWithData와 비슷하다. 안에서 만들던 pNewNode를 밖에서 입력받아 \_pNewNode의 다음 노드 이전 노드를 직접 수정하는 것만 다른 부분이다.

이번엔 노드를 삭제해보자. 먼저 인덱스 번호를 입력받아 그 번호의 노드를 삭제하는 함수다.

```c
void RemoveAt(const int _idx) {
	if (g_head.pNext == &g_tail) return;

	SNode* pIter = g_head.pNext;
	SNode* pDel = NULL;
	int cnt = 0;
	// 1. pIter가 &g_tail이 아닌동안 반복
	// 2. pDel에 pIter->pNext를 복사
	// 3. cnt를 증가
	// 4. cnt가 _idx일때 pInter의 이전 노드와 pIter의 다음노드를 연결하고 pIter을 동적해제

	while (pIter != &g_tail) {
		pDel = pIter->pNext;
		if (_idx == cnt) {
			pIter->pPrev->pNext = pIter->pNext;
			pIter->pNext->pPrev = pIter->pPrev;
			SAFE_FREE(pIter);
			printf("[RemoveAt] [%d] Index Removed\n", _idx);
		}
		pIter = pDel;
		++cnt;
	}
}
```

간단하게 설명하자면 cnt가 \_idx일때 그 순번의 pIter의 양 옆 노드가 pIter를 건너뛰고 서로 연결되게 하는 코드다. pIter의 이전 노드가 가리키는 다음 노드가 pIter의 다음 노드가 되도록, 즉 pIter의 이전 노드가 pIter를 건너뛰어 그 다음 노드와 연결되도록 하는 과정을 거친다. 머리 속으로 그림을 그려보며 천천히 생각해보면 어떤 식으로 구현되는지 알 수 있을 것이다.

아래는 데이터를 입력받아 해당 데이터를 가지는 노드를 모두 삭제하는 함수다.

```c
void Remove(int _data) {
	if (g_head.pNext == &g_tail) return;

	SNode* pIter = g_head.pNext;
	SNode* pDel = NULL;

	while (pIter != &g_tail) {
		pDel = pIter->pNext;
		if (_data == pIter->data) {
			pIter->pPrev->pNext = pIter->pNext;
			pIter->pNext->pPrev = pIter->pPrev;
			SAFE_FREE(pIter);
			printf("[Remove] Data %d Removed\n", _data);
		}
		pIter = pDel;
	}
}
```

g_head의 다음부터 g_tail이 되기 전까지(g_tail의 이전까지) 반복하며 그 사이에 노드가 가진 데이터가 입력받은 데이터랑 같으면 위의 RemoveAt 함수처럼 그 순번의 pIter의 양 옆 노드가 pIter를 건너뛰어서 서로 연결되도록 하는 코드다. RemoveAt이 이해가 된다면 조건만 다를 때 똑같은 과정을 거치는 함수라 이해가 쉬울 것이다.

마지막으로 전체 노드를 삭제하는 RemoveAll 함수다.

```c
void RemoveAll() {
	if (g_head.pNext == &g_tail) return;

	SNode* pIter = g_head.pNext;
	SNode* pNext = NULL;
	while (pIter != &g_tail) {
		pNext = pIter->pNext;
		printf("[RemoveAll] : %d\n", pIter->data);
		SAFE_FREE(pIter);
		pIter = pNext;
	}

	g_head.pNext = &g_tail;
	g_tail.pPrev = &g_head;
}
```

헤드 다음 노드부터 테일 이전 노드를 모두 삭제한다(연결을 해제한다). 중간에 pIter를 pNext에 저장하는 이유는 pNext가 없는 상태로 pIter를 할당 해제해버리면 다음 노드로 갈 방법이 없어지기 때문에 일단 다음으로 넘어갈 방법을 저장해두는 것이다. 그리고 마지막으로 g_head와 g_tail을 연결해주면 중간에 존재하던 모든 노드가 사라지게 된다.

마지막으로 메인 함수의 결과를 보며 코드의 진행 과정을 다시 한 번 생각해보자.

```c
void main() {
	g_head.data = -1;
	g_head.pNext = &g_tail;
	g_head.pPrev = NULL;

	g_tail.data = -1;
	g_tail.pPrev = &g_head;
	g_tail.pNext = NULL;

	PushFront(1);
	PushFront(2);

	PushBack(CreateNode(10, NULL, NULL));
	PushBack(CreateNode(20, NULL, NULL));

	PrintForward();
	PrintBackward();
	RemoveAll();
	printf("\n");

	PushFront(1);
	PrintForward();
	printf("\n");

	PushFront(1);
	PushFront(2);
	PrintForward();
	printf("\n");

	InsertWithData(2, 3);
	PrintForward();
	printf("\n");

	InsertWithNode(3, CreateNode(5, NULL, NULL));
	PrintForward();
	printf("\n");

	RemoveAt(0);
	PrintForward();

	RemoveAt(2);
	PrintForward();

	RemoveAt(2);
	PrintForward();

	PushFront(1);
	PushFront(2);
	PushFront(3);
	PrintForward();

	Remove(2);
	PrintForward();

	Remove(1);
	PrintForward();

	RemoveAll(); // 메모리 정리
}
```

```text
실행 결과

2 - 1 - 10 - 20 - (4)
20 - 10 - 1 - 2 - (4)
[RemoveAll] : 2
[RemoveAll] : 1
[RemoveAll] : 10
[RemoveAll] : 20

1 - (1)

2 - 1 - 1 - (3)

[InsertWithData] [2] : 3
2 - 1 - 3 - 1 - (4)

[InsertWithNode] [3] : 5
2 - 1 - 3 - 5 - 1 - (5)

[RemoveAt] [0] Index Removed
1 - 3 - 5 - 1 - (4)
[RemoveAt] [2] Index Removed
1 - 3 - 1 - (3)
[RemoveAt] [2] Index Removed
1 - 3 - (2)
3 - 2 - 1 - 1 - 3 - (5)
[Remove] Data 2 Removed
3 - 1 - 1 - 3 - (4)
[Remove] Data 1 Removed
[Remove] Data 1 Removed
3 - 3 - (2)
[RemoveAll] : 3
[RemoveAll] : 3
```
