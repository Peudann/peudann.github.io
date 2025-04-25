---
title: "25.03.16 / 단일 연결 리스트(Singly Linked List)"
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

리스트는 그 이름에서 유추할 수 있듯 목록 형태로 이뤄진 데이터 형식이다. 리스트의 목록을 이루는 개별 요소를 노드(Node)라고 부른다. 리스트의 첫 번째 노드와 마지막 노드를 부르는 이름이 따로 있는데, 데이터를 담는 노드 목록에서 첫 번째 노트를 헤드(Head)라고 부르고 마지막 노드를 테일(Tail)이라고 부른다. 리스트의 길이는 헤드부터 테일까지를 이르는 노드의 개수와 같다.

리스트와 배열을 비교해보자. 배열은 리스트처럼 데이터 목록을 다루며, C언어에서 기본적으로 제공하기 때문에 프로그래머가 따로 구현하지 않아도 된다는 장점이 있다. 그런데 왜 배열이 아닌 리스트를 사용하는 걸까? 배열은 생성하는 시점에 반드시 배열의 크기를 지정해줘야 하고 생성한 후에는 그 크기를 변경할 수 없다는 단점이 있다.

하지만 소프트웨어는 종종 프로그래머에게 배열의 한계를 넘어설 것을 요구한다. 예를 들어 소프트웨어가 임의의 디렉토리 내에 있는 파일 목록을 필요로 한다면 어떻게 해야할까? 그 디렉토리에는 파일이 전혀 없을 수도 있고, 10개도 채 안되는 파일만 있을 수도 있다. 아니면 수천, 수만 개의 파일이 있을지도 모른다. 분명한건 디렉토리 내에 있는 파일의 수는 프로그래머의 바람과는 무관하게 정해진다는 것이다.

파일이 항상 10개 미만으로 있으면 좋겠다고 바라면서 다음처럼 배열을 선언할 순 없다.

```c
char* files[10];
```

그렇다고 또 무작정 큰 배열을 선언할 수도 없다. 게다가 문제를 완전히 해결하지도 못한다. 선언한 수보다 더 많은 파일이 존재할 수도 있다.

```c
char* files[65535];
```

너무 작게 선언하자니 일을 제대로 할 수가 없고 무작정 크게  선언하자니 메모리가 부족하다. 이 문제를 해결하기 위해 필요한 것이 배열처럼 데이터 집합 보관 기능을 가지면서도 배열과 달리 유연하게 크기를 바꿀 수 있는 자료구조다. 이 자료구조가 바로 리스트이다. 이번 글에선 리스트를 구현하는 여러 기법 중 가장 간단한 방법으로 꼽히는 링크드 리스트(Linked List)를 알아보겠다.

링크드 리스트는 노드를 연결해서 만든 리스트라고 해서 붙여진 이름이다. 링크드 리스트의 노드는 다음과 같이 데이터를 보관하는 필드, 다음 노드와 연결 고리 역할을 하는 포인터로 이루어진다.

<div style="display: flex; gap: 1rem; margin-bottom: 1rem;">
  <img src="/assets/img/250316_singlyLinkedList/01.png" alt="01" style="max-width: 100%;" />
</div>

데이터와 포인터로 이루어진 노드들을 다음 그림처럼 주렁주렁 모두 엮으면 링크드 리스트가 되는 것이다.

<div style="display: flex; gap: 1rem; margin-bottom: 1rem;">
  <img src="/assets/img/250316_singlyLinkedList/02.png" alt="02" style="max-width: 100%;" />
</div>

리스트의 첫 번째 노드를 헤드라고 하고 마지막 노드를 테일이라고 부른다고 했다. 이 구조는 여러 장점을 가지는데, 다뤄야 하는 데이터 집합의 크기를 미리 알지 못해도 걱정할 필요없이 데이터가 늘어날 때마다 노드를 만들어 테일에 붙이기만 하면 그만이다. 이렇게 붙인 노드는 새로운 테일이 되고 기존 테일은 평범한 노드가 된다.

리스트 사이에 새로운 노드를 끼워 넣거나 제거하기도 아주 쉽다. 해당 노드를 가리키는 포인터만 교환하면 된다.

링크드 리스트의 기본 연산을 배우기 전, 노드를 표현하는 방법을 먼저 알아보겠다. 링크드 리스트의 노드를 C언어로 표현하면 다음과 같은 구조체로 나타낼 수 있다.

```c
typedef struct Node {
    int data;              // 저장할 데이터(정수 예시)
    struct Node* next;     // 다음 노드를 가리키는 포인터
} Node;
```

이제 링크드 리스트가 어떤 연산을 가져야 할지 생각해보자. 링크드 리스트에는 두 종류의 연산이 필요하다. 첫 번째는 자료구조를 구축하기 위한 연산이고, 두 번쨰는 자료구조에 저장된 데이터를 활용하기 위한 연산이다. 다음 여섯 가지 주요 연산을 링크드 리스트 자료구조에 구현해보겠다.

- 노드 생성(CreateNode) / 소멸(DestroyNode)
- 노드 추가(AppendNode)
- 노드 탐색(GetNodeAt)
- 노드 삭제(RemoveNode)
- 노드 삽입(InsertNode)

연산을 만들기 전, C언어로 작성된 프로그램의 메모리 영역을 기억하는가? 전역 변수와 정적 변수가 저장되는 정적 메모리(Data), 자동으로 메모리를 해제하는 자동 메모리(Stack), 자유롭게 데이터를 할당해서 사용하는 자유 저장소(Heap)가 있었다. 링크드 리스트의 노드는 자동 메모리와 자유 저장소 둘 중 어느 곳에 생성하는 게 좋을까?

먼저 자동 메모리(Stack)을 사용하는 방안을 생각해보자.

```c
Node* CreateNode(int _data) {
    Node newNode;
    newNode.data = _data;
    newNode.next = NULL;

    return &newNode;
}

Node* myNode = CreateNode(117);
```

이 코드를 같이 살펴보자. CreateNode() 함수가 가장 먼저 지역 변수 newNode를 자동으로 자동 메모리(Stack)에 생성하고 newNode의 data와 nex 필드를 초기화한다. 그리고 함수 끝에서 만든 newNode의 주소를 반환한다.

문제는 return문이 실행된 이후에 벌어지는데 myNode 포인터는 newNode가 존재하는 메모리의 주소를 갖고 있는 것이 아니게 된다. 자동 메모리에 의해 제거된 newNode가 존재했던 과거의 메모리 주소를 담고 있는 것이다. 이미 사라져버린 newNode의 메모리 주소를 담고 있는 myNode 때문에 프로그램이 뻗거나 예기치 않은 동작을 야기하게 된다.

그럼 이제 노드 생성에 자동 메모리가 적합하지 않다는 걸 알았으니 남은 선택지는 자유 저장소 뿐이다. 자유 저장소를 활요하려면 메모리를 동적 할당하는 malloc() 함수가 필요하다. malloc을 사용하여 노드를 자유 저장소에 생성하는 예제는 다음과 같다.

```c
Node* newNode = (Node*)malloc(sizeof(Node));
```

malloc을 이용해 CreateNode를 고쳐보자.

```c
Node* CreateNode(int _data) {
    Node* newNode = (Node*)malloc(sizeof(Node));

    newNode->data = _data;
    newNode->next = NULL;

    return newNode;
}
```

이제 노드를 제대로 생성할 수 있게 되었으니 노드를 소멸(Destroy)시키는 예제도 만들어보자. 노드를 소멸시키는 코드는 단순하다. 동적 할당받은 메모리를 해제하기만 하면 알아서 노드가 소멸한다.

```c
void DestroyNode(Node* Node) {
    free(Node);
}
```

이번엔 노드를 추가하는 연산을 구현해보겠다. 노드 추가는 링크드 리스트의 테일 노드 뒤에 새로운 노드를 만들어 연결한다.

<div style="display: flex; gap: 1rem; margin-bottom: 1rem;">
  <img src="/assets/img/250316_singlyLinkedList/03.png" alt="03" style="max-width: 100%;" />
</div>

CreateNode() 함수를 이용해 자유 저장소에 노드를 생성한 다음, 새로 생성한 노드의 주소를 테일의 next 포인터에 저장하면 된다.

```c
void AppendNode(Node** Head, Node* NewNode) {
    if ((*Head) == NULL) *Head = NewNode;
    else {
        Node* Tail = (*Head);
        while (Tail->next != NULL) {
            Tail = Tail->NextNode;
        }
        Tail->NextNode = NewNode;
    }
}
```

```c
Node* List = NULL;
Node* NewNode = NULL;

NewNode = CreateNode(117);
AppendNode(&list, NewNode);

AppendNode(&list, CreateNode(119));
```

이번엔 노드 탐색 연산을 만들어보자. 링크드 리스트의 노드 탐색 연산은 조금 비효율적인 면이 있다. 배열에서는 어떤 위치에 있는 요소를 취하고 싶을 때 해당 요소의 인덱스를 입력하면 바로 해당 요소로 접근할 수 있다. 이에 반해, 링크드 리스트는 헤드부터 시작해 다음 노드에 대한 포인터를 다리 삼아 차근차근 노드의 개수만큼 거쳐야만 원하는 요소에 접근할 수 있다. 찾고자 하는 요소가 n번째라면 n-1개의 노드를 거쳐야만 찾을 수 있는 것이다.

링크드 리스트 내에 임의의 위치에 있는 노드를 찾아 반환하는 GetNodeAt() 함수를 만들어보자.

```c
Node* GetNodeAt(Node* _pHead, int _location) {
    Node* Current = _pHead;

    while (Current != NULL && (--_location) >= 0) {
        Current = Current->next;
    }

    return Current;
}
```

헤드 노드와 위치를 입력받아 입력받은 위치의 노드를 찾아 반환하는 코드다. Current는 입력받은 헤드에서 시작해 --\_location이 0보다 큰 동안 계속 다음 노드로 옮겨가다가 \_location이 0 이하가 되면(입력받은 위치에 도착하면) 그 값을 반환한다.

사용은 간단하지만 내부 구현은 비효율적이다. 이러한 노드 탐색의 비효율성은 링크드 리스트의 숙명과도 같다.

이번엔 노드를 삭제하는 연산을 만들어보자. 노드 삭제 연산은 링크드 리스트 내 임의의 노드를 제거하는 연산이다. 삭제하고자 하는 노드를 찾은 뒤 해당 노드의 다음 노드를 이전 노드의 next에 연결하면 그 노드를 삭제할 수 있다.

<div style="display: flex; gap: 1rem; margin-bottom: 1rem;">
  <img src="/assets/img/250316_singlyLinkedList/04.png" alt="04" style="max-width: 100%;" />
</div>

```c
void RemoveNode(Node** _pHead, Node* _remove){
    if (*_pHead == _remove) {
        *_pHead = _remove->next;
    }
    else {
        Node* Current = *_pHead;
        while (Current != NULL && Current->next != _remove) {
            Current = Current->next;
        }
        if (Current != NULL) Current->next = _remove->next;
    }
}
```

노드를 삭제하고 더 이상 쓰지 않는 경우, 소멸까지 진행해서 동적 할당된 메모리를 반환하는 걸 잊지말자.

```c
Node* List = NULL;
Node* MyNode = NULL;

AppendNode(&List, CreateNode(117));
AppendNode(&List, CreateNode(119));
AppendNode(&List, CreateNode(121));

MyNode = GetNodeAt(List, 1)	// 두 번째 노드의 주소를 MyNode에 저장
printf("%d\n", MyNode->data);

RemoveNode(&List, MyNode) // 두 번째 노드를 제거
DestoryNode(MyNode) // 제거한 노드를 메모리에서 소멸
```

이번엔 노드 삽입 연산이다. 노드 삽입은 다음 그림과 같이 노드 사이에 새로운 노드를 끼워 넣는 연산이다.

<div style="display: flex; gap: 1rem; margin-bottom: 1rem;">
  <img src="/assets/img/250316_singlyLinkedList/05.png" alt="05" style="max-width: 100%;" />
</div>

노드 삽입(InsertNode) 함수는 다음과 같이 구현한다.

```c
void InsertNode(Node* _current, Node* _newNode) {
    _newNode->next = _current->next;
    _current->next = _newNode;
}
```

아래 코드는 조금 다른 방식으로 위의 연산을 구현한 코드이다. 여러 코드를 읽어보고 분석해보며 이해하도록 노력해보자.

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct _SNode
{
	int data;

	struct _SNode* pNext;
} SNode;


SNode* CreateNode(int _data);	// 노드 생성하기
void AddNode(SNode** const _pHead, const SNode* const _pNewNode);	// 노드 추가하기
void InsertNode(int _idx, SNode** const _pHead, SNode* const _pNewNode);	// 중간에 노드 삽입하기
void RemoveAt(int _idx, SNode** const _pHead);	// 해당 인덱스 삭제하기
void RemoveAll(SNode** const _pHead);	// 노드 전부 지우기
void RemoveRecursive(SNode* _pHead);	// 재귀(Recursive)를 이용한 노드 전부 지우기
void PrintAll(const SNode* const _pHead);

// 길이 구하기 만들어보기



void main()
{
	// 자료구조(Data Structure)
	// 메모리 단편화
	// 메모리 풀(Pool)

	// 링크드 리스트(Linked List)
	// 싱글 링크드 리스트(Single Linked List)
	SNode* pHead = NULL;

	PrintAll(pHead);

	AddNode(&pHead, CreateNode(10));
	AddNode(&pHead, CreateNode(55));
	AddNode(&pHead, CreateNode(222));

	PrintAll(pHead);

	InsertNode(0, &pHead, CreateNode(7));
	PrintAll(pHead);

	InsertNode(1, &pHead, CreateNode(11));
	PrintAll(pHead);

	InsertNode(20, &pHead, CreateNode(555));

	InsertNode(4, &pHead, CreateNode(44));
	PrintAll(pHead);

	RemoveAt(0, &pHead);
	PrintAll(pHead);

	RemoveAt(4, &pHead);
	PrintAll(pHead);

	RemoveAt(2, &pHead);
	PrintAll(pHead);

	RemoveAll(&pHead);
	PrintAll(pHead);
	AddNode(&pHead, CreateNode(10));
	PrintAll(pHead);
}


SNode* CreateNode(int _data)
{
	SNode* pNewNode = (SNode*)malloc(sizeof(SNode));
	if (pNewNode == NULL) return NULL;

	pNewNode->data = _data;
	pNewNode->pNext = NULL;

	return pNewNode;
}

void AddNode(SNode** const _pHead,
	const SNode* const _pNewNode)
{
	if (_pHead == NULL || _pNewNode == NULL) return;

	if ((*_pHead) == NULL)
	{
		(*_pHead) = _pNewNode;
		printf("Set head: %d\n", _pNewNode->data);
		return;
	}

	// 반복자(Iterator)
	SNode* pCurNode = (*_pHead);
	while (pCurNode->pNext != NULL)
		pCurNode = pCurNode->pNext;

	pCurNode->pNext = _pNewNode;
	printf("AddNode: %d\n", _pNewNode->data);
}

void InsertNode(int _idx, SNode** const _pHead, SNode* const _pNewNode) {
	if (_idx < 0 || _pHead == NULL || _pNewNode == NULL) return;
	if (_idx == 0) {
		_pNewNode->pNext = (*_pHead);
		(*_pHead) = _pNewNode;
		printf("InsertNode: [%d] <- %d\n", 0, _pNewNode->data);
		return;
	}
	SNode* pCurNode = (*_pHead);
	int curIdx = 0;
	while (pCurNode->pNext != NULL) {
		if (curIdx == _idx - 1) {
			_pNewNode->pNext = pCurNode->pNext;
			pCurNode->pNext = _pNewNode;
			printf("InsertNode : [%d] <- %d\n", _idx, _pNewNode->data);
			return;
		}
		pCurNode = pCurNode->pNext;
		++curIdx;
	}

	printf("InsertNode: Out of Index!\n");
}

void RemoveAt(int _idx, SNode** const _pHead) {
	if (_idx < 0 || _pHead == NULL) return;
	if (_idx == 0) {
		printf("RemoveAt : [%d]\n", _idx);
		SNode* pDelNode = (*_pHead);
		(*_pHead) = pDelNode->pNext;
		if (pDelNode != NULL) {
			free(pDelNode);
			pDelNode = NULL;
		}

		return;
	}

	SNode* pCurNode = (*_pHead);
	int curIdx = 0;
	while (pCurNode->pNext != NULL) {
		if (curIdx == _idx - 1) {
			printf("RemoveAt : [%d]\n", _idx);
			SNode* pDelNode = pCurNode->pNext;
			pCurNode->pNext = pDelNode->pNext;

			if (pDelNode != NULL) {
				free(pDelNode);
				pDelNode = NULL;
			}
			return;
		}
		pCurNode = pCurNode->pNext;
		++curIdx;
	}

}

void RemoveAll(SNode** const _pHead) {
	if (_pHead == NULL) return;
	/*while ((*_pHead) != NULL) {
		RemoveAt(0, _pHead);
	}*/
	RemoveRecursive((*_pHead));
	(*_pHead) = NULL;
}

void RemoveRecursive(SNode* _pNode) {
	if (_pNode == NULL) return;
	RemoveRecursive(_pNode->pNext);

	printf("RemoveRecursive : %d\n", _pNode->data);
	if (_pNode != NULL) {
		free(_pNode);
		_pNode = NULL;
	}
}


void PrintAll(const SNode* const _pHead)
{
	if (_pHead == NULL)
	{
		printf("Empty!\n");
		return;
	}

	// 상수성 보장
	const SNode* pCurNode = _pHead;
	int cnt = 0;
	while (pCurNode != NULL)
	{
		printf("%d - ", pCurNode->data);
		pCurNode = pCurNode->pNext;
		++cnt;
	}
	printf("(%d)\n", cnt);
}
```

본 포스팅의 일부 내용과 그림은 한빛미디어의 '이것이 자료구조+알고리즘이다 with C언어, 저자 박상현'에서 발췌했다. 본 포스팅은 어떤 방식으로도 블로그 소유자의 금전적 이익을 생성하지 않으며, 교육을 목적으로 써진 글임을 알린다.
