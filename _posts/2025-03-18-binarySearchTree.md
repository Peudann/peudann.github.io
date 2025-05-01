---
title: "25.03.18 / 이진 탐색 트리(Binary Search Tree)"
excerpt: ""

categories:
  - Programming
  - Data Structure and Algorithm
tags:
  - []

toc: true
toc_sticky: true

date: 2025-03-18
last_modified_at: 2025-04-29
---

트리(Tree)는 그 이름에서 알 수 있듯 나무를 닮은 자료 구조다. 나무에는 뿌리가 있고 뿌리에서는 가지가 뻗어 나오며 가지 끝에는 잎이 달린다. 우리 주변에도 이러한 나무의 구조를 응용한 여러 트리가 있다.

예를 들어 게임의 캐릭터 성장 시스템에 사용되는 스킬 트리나 기업에서 전략을 수립할 때 사용하는 의사 결정 트리를 보면 트리 자료 구조의 모양을 하고 있는 걸 볼 수 있다(애초에 이름이 트리기도 하고).

이러한 트리 구조는 컴퓨터 과학에서도 활용도가 매우 높다. 운영체제의 파일 시스템이 트리 구조로 이루어져 있고, HTML이나 XML 문서를 다룰 때 사용하는 DOM(Document Object Model)도 트리 구조로 이루어져있다. 또한 검색 엔진이나 데이터베이스도 트리 자료 구조에 기반해서 구현된다.

이번 글에선 트리 구조 중 이진 트리(Binary Tree)에 대해 알아보자. 이진 트리는 하나의 노드가 자식 노드를 2개까지만 가질 수 있는 트리이다. 이진이라는 이름도 자식을 둘만 가진다는 의미에서 붙여졌다(그래서 자식을 4개까지 가지면 Quad Tree, 8개까지 가지면 Octree라고 부른다). 아래 그림은 이진 트리의 구조를 개념적으로 표현한 그림이다.

<div style="display: flex; gap: 1rem; margin-bottom: 1rem;">
  <img src="/assets/img/250318_binarySearchTree/01.png" alt="01" style="max-width: 100%;" />
</div>

당연한 이야기지만 이진 트리를 이진 트리로 만드는 가장 중요한 특징은 노드의 최대 차수가 2라는 사실이다. 다시 말해, 모든 이진 트리 노드의 자식 노드 수는 0, 1, 2 중 하나이다.

다음 그림처럼 가장 끝 단의 노드를 제외한 모든 노드가 자식 노드를 둘씩 가진 이진 트리를 포화 이진 트리(Full Binary Tree)라고 한다. 포화 이진 트리는 끝 단 노드들이 모두 같은 깊이에 위치한다는 특징을 가진다.

<div style="display: flex; gap: 1rem; margin-bottom: 1rem;">
  <img src="/assets/img/250318_binarySearchTree/02.png" alt="02" style="max-width: 100%;" />
</div>

포화 이진 트리와 비슷하지만 포화 이진 트리로 진화하기 전 단계의 트리도 있다. 이 트리를 완전 이진 트리(Complete Binary Tree)라고 하는데, 끝 단 노드들이 트리 왼쪽부터 차곡차곡 채워진 것이 특징이다. 다음 그림의 이진 트리들은 모두 완전 이진 트리이다.

<div style="display: flex; gap: 1rem; margin-bottom: 1rem;">
  <img src="/assets/img/250318_binarySearchTree/03.png" alt="03" style="max-width: 100%;" />
</div>

포화 이진 트리와 완전 이진 트리는 왜 특별한 이름을 붙여 따로 분류를 했을까?

이진 트리는 일반 트리처럼 나무 모양의 자료(예를 들어 조직도)를 담기 위한 자료구조가 아니라 컴파일러나 검색과 같은 알고리즘의 뼈대가 되는 특별한 자료구조다. 특히 이진 트리를 이용한 검색에서는 트리의 노드를 가능한 한 완전한 모습으로 유지해야 높은 성능을 낼 수 있다. 그래서 우리는 완전한 모습의 트리가 무엇인지 알고 있어야 하는 것이다.

그 외에도 여러 트리가 있지만, 우리는 짧게 높이 균형 트리, 완전 높이 균형 트리까지만 알아보자.

높이 균형 트리는 다음 그림과 같이 뿌리 노드를 기준으로 왼쪽 하위 트리와 오른쪽 하위 트리의 높이가 2 이상 나지 않는 이진 트리를 일컫는다.

<div style="display: flex; gap: 1rem; margin-bottom: 1rem;">
  <img src="/assets/img/250318_binarySearchTree/04.png" alt="04" style="max-width: 100%;" />
</div>
   
완전 높이 균형 트리는 다음 그림과 같이 뿌리 노드를 기준으로 왼쪽 하위 트리와 오른쪽 하위 트리의 높이가 같은 이진 트리를 가리킨다.

<div style="display: flex; gap: 1rem; margin-bottom: 1rem;">
  <img src="/assets/img/250318_binarySearchTree/05.png" alt="05" style="max-width: 100%;" />
</div>

다음 내용을 설명하기 전 잠깐 이진 탐색에 대해 알아보자. 이진 탐색(Binary Search)은 정렬된 데이터에서 사용할 수 있는 고속 탐색 알고리즘이다. 이 알고리즘의 핵심이 탐색 범위는 1/2씩 줄여나가는 방식이 있기 때문에 붙여진 이름이다. 다음에 기회가 있다면 이진 탐색도 포스팅으로 다뤄보겠지만, 지금은 아무튼 빠른 탐색 방법이라는 것만 기억해둔 다음 우리는 이진 탐색 트리에 대해 알아보자.

이진 탐색 트리(Binary Search Tree)는 이진 탐색을 위한 트리이자 탐색을 위한 이진 트리다. 다시 말해, 이진 탐색 트리는 이진 탐색을 위한 이진 트리인 것이다. 위에서 설명했듯 이진 트리는 자식 노드가 최대 2개뿐인 노드로만 구성된 트리라는 사실을 알고 있을 것이다. 이 독특한 모습의 트리가 컴파일러의 표현식 트리를 비롯해 다양한 용도로 활용되고 있다는 사실도 함께 말이다. 그런데 이진 탐색 알고리즘이 존재하는데 왜 이진 탐색 트리가 필요한 걸까?

이는 이진 탐색이 배열에만 사용 가능한 알고리즘이기 때문이다. 이진 탐색을 사용하려면 데이터의 처음과 끝을 알아야하고 순식간에 데이터의 중앙 요소를 계산할 수 있어야 하며 계산된 중앙 요소에 즉시 접근할 수 있어야 한다. 저번 글에 설명한 링크드 리스트는 위와 같은 작업들이 불가능한 자료구조다. 헤드와 테일의 위치는 알 수 있어도 헤드와 테일 사이의 중앙 요소를 알 수 없기 때문이다. 링크드 리스트처럼 동적으로 노드를 추가하거나 제거할 수 있으면서 이진 탐색 알고리즘도 사용할 수 있는 방법이 이진 탐색 트리이다.

이진 탐색 트리가 다른 이진 트리와 다른 특별한 점은 각 노드가 '왼쪽 자식 노드는 나보다 작고 오른쪽 자식 노드는 나보다 크다'라는 규칙을 따른다는 것이다. 그래서 이진 탐색 트리는 다음과 같은 모습으로 구성된다.

<div style="display: flex; gap: 1rem; margin-bottom: 1rem;">
  <img src="/assets/img/250318_binarySearchTree/06.png" alt="06" style="max-width: 100%;" />
</div>

또한 다음 그림의 트리들이 위의 이진 탐색 트리처럼 균형 잡히진 않았어도 모두 이진 탐색 트리이다. 트리의 각 노드가 이진 탐색 트리 노드의 조건을 갖추고 있기 때문이다.

<div style="display: flex; gap: 1rem; margin-bottom: 1rem;">
  <img src="/assets/img/250318_binarySearchTree/07.png" alt="07" style="max-width: 100%;" />
</div>

이제 코드를 보며 이진 탐색 트리의 연산에 대해 알아보자.

```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define SAFE_FREE(p) if(p) { free(p); p = NULL; }
#define MAX_LEN 5

typedef struct _SNode {
	int data;
	struct _SNode* pLeft;
	struct _SNode* pRight;
} SNode;

// 난수 생성 함수
int RandomRange(int _min, int _max) {
	return (rand() % (_max - _min + 1)) + _min;
}

SNode* g_pRoot = NULL;	// 헤드 전역변수 선언

SNode* CreateNode(int _data);
void BuildTree(const int _arr[]);
void AddNode(SNode* _pNewNode);
void AddNodeRecursive(SNode* _pCurNode, SNode* _pNewNode);
void PrintAll();
void PrintAllRecursive(SNode* _pCurNode);
void RemoveAll();
void RemoveAllRecursive(SNode* _pCurNode);

// 2진 트리(Binary Tree)
int main() {
	// Binary Search Tree

	// Random(난수)
	// Random Seed
	int dataset[MAX_LEN] = { 0 };

	srand(time(NULL));
	for (int i = 0; i < MAX_LEN; ++i) {
		dataset[i] = RandomRange(0, 20);
		printf("%d\n", dataset[i]);
	}

	BuildTree(dataset);
	PrintAll();

	RemoveAll();

	return 0;
}

SNode* CreateNode(int _data) {
	SNode* pNewNode = (SNode*)malloc(sizeof(SNode));
	pNewNode->data = _data;
	pNewNode->pLeft = NULL;
	pNewNode->pRight = NULL;

	return pNewNode;
}

void BuildTree(const int _arr[]) {
	if (_arr == NULL) return;
	/*for (int i = 0; i < MAX_LEN; ++i) {
		AddNode(CreateNode(_arr[i]));
	}*/
	g_pRoot = CreateNode(_arr[0]);
	for (int i = 1; i < MAX_LEN; ++i) {
		AddNodeRecursive(g_pRoot, CreateNode(_arr[i]));
	}
}

void AddNode(SNode* _pNewNode) {
	if (_pNewNode == NULL) return;

	printf("[AddNode] Data : %d\n", _pNewNode->data);

	if (g_pRoot == NULL) {
		g_pRoot = _pNewNode;
		printf("[AddNode] Set Root : %d\n", _pNewNode->data);
		return;
	}

	SNode* pCurNode = g_pRoot;
	while (1) {
		if (_pNewNode->data <= pCurNode->data){
			// 왼쪽
			if (pCurNode->pLeft == NULL) {
				pCurNode->pLeft = _pNewNode;
				printf("[AddNode] Set Left : %d\n", _pNewNode->data);
				return;
			}
			else {
				pCurNode = pCurNode->pLeft;
				printf("[AddNode] << Move Left\n");
			}
		}
		else if (_pNewNode->data > pCurNode->data) {
			// 오른쪽
			if (pCurNode->pRight == NULL) {
				pCurNode->pRight = _pNewNode;
				printf("[AddNode] Set Right : %d\n", _pNewNode->data);
				return;
			}
			else {
				pCurNode = pCurNode->pRight;
				printf("[AddNode] Move Right >>\n");
			}
		}
	}
}

void AddNodeRecursive(SNode* _pCurNode, SNode* _pNewNode) {
	if (_pCurNode == NULL || _pNewNode == NULL) return;

	if (_pNewNode->data <= _pCurNode->data) {
		// 왼쪽으로
		if (_pCurNode->pLeft == NULL) {
			_pCurNode->pLeft = _pNewNode;
			return;
		}
		else {
			AddNodeRecursive(_pCurNode->pLeft, _pNewNode);
		}
	}
	else {
		if (_pCurNode->pRight == NULL) {
			_pCurNode->pRight = _pNewNode;
			return;
		}
		else {
			AddNodeRecursive(_pCurNode->pRight, _pNewNode);
		}
	}
}

void PrintAll() {
	if (g_pRoot == NULL) {
		printf("[PrintAll] Empty!\n");
		return;
	}
	PrintAllRecursive(g_pRoot);
	printf("\n");
}

void PrintAllRecursive(SNode* _pCurNode) {
	if (_pCurNode == NULL) return;

	PrintAllRecursive(_pCurNode->pLeft);
	printf("[%d] - ", _pCurNode->data);
	PrintAllRecursive(_pCurNode->pRight);
}

void RemoveAll() {
	if (g_pRoot == NULL) return;

	RemoveAllRecursive(g_pRoot);
	g_pRoot = NULL;
}

void RemoveAllRecursive(SNode* _pCurNode) {
	if (_pCurNode == NULL) return;

	RemoveAllRecursive(_pCurNode->pLeft);
	RemoveAllRecursive(_pCurNode->pRight);

	printf("[RemoveAll] %d\n", _pCurNode->data);
	SAFE_FREE(_pCurNode);
}
```

본 코드는 뿌리 노드를 전역 변수로 선언하고, 메인 함수에서 MAX_LEN의 크기를 가지면서 난수를 요소로 가지는 배열 dataset을 미리 선언했다.

```c
SNode* CreateNode(int _data) {
	SNode* pNewNode = (SNode*)malloc(sizeof(SNode));
	pNewNode->data = _data;
	pNewNode->pLeft = NULL;
	pNewNode->pRight = NULL;

	return pNewNode;
}
```

먼저 노드를 생성하는 함수다. \_data를 매개변수로 입력 받아, 구조체 SNode만큼의 크기를 가지는 구조체 포인터 변수 pNewNode를 만들었다. 입력 받은 \_data를 새로운 노드의 data 멤버에 넣고, 아직 정해지지 않은 pLeft와 pRight는 NULL로 초기화 한 뒤에 pNewNode를 반환한다.

노드를 만들었으니 트리를 구성해보자.

```c
void BuildTree(const int _arr[]) {
	if (_arr == NULL) return;
	/*for (int i = 0; i < MAX_LEN; ++i) {
		AddNode(CreateNode(_arr[i]));
	}*/
	g_pRoot = CreateNode(_arr[0]);
	for (int i = 1; i < MAX_LEN; ++i) {
		AddNodeRecursive(g_pRoot, CreateNode(_arr[i]));
	}
}
```

전역 변수로 선언한 g_pRoot를 입력 받은 \_arr 배열의 0번 요소로 삼아 그 이후 반복문을 사용해 각 인덱스에 요소를 삽입하는 함수다. 입력 받은 배열이 NULL이면 트리를 구성할 수 없으니 함수를 종료한다.

안에 들어있는 AddNodeRecursive는 어떤 기능을 하는걸까?

```c
void AddNodeRecursive(SNode* _pCurNode, SNode* _pNewNode) {
	if (_pCurNode == NULL || _pNewNode == NULL) return;

	if (_pNewNode->data <= _pCurNode->data) {
		// 왼쪽으로
		if (_pCurNode->pLeft == NULL) {
			_pCurNode->pLeft = _pNewNode;
			return;
		}
		else {
			AddNodeRecursive(_pCurNode->pLeft, _pNewNode);
		}
	}
	else {
		if (_pCurNode->pRight == NULL) {
			_pCurNode->pRight = _pNewNode;
			return;
		}
		else {
			AddNodeRecursive(_pCurNode->pRight, _pNewNode);
		}
	}
}
```

현재 노드와 추가할 노드를 입력받아 함수를 진행한다. 현재 노드나 추가할 노드가 NULL이면 삽입도 불가능하므로 함수를 종료하고 반환한다. 위에서 설명했듯 이진 탐색 트리는 왼쪽 노드가 나보다 작고 오른쪽 노드가 나보다 커야 된다는 특징을 가진다. 그래서 조건문을 사용해 새로 넣을 노드의 데이터 값이 현재 노드의 데이터보다 작거나 같으면 왼쪽 노드로 이동하고, 그렇지 않다면 오른쪽 노드로 이동한다. 만약 이동한 노드가 비어있다면 거기에 노드를 삽입하고, 이미 값이 있는 노드라면 재귀함수를 이용해 다시 한 번 데이터의 크기를 비교해 값을 넣을 때까지 반복한다.

이제 값을 넣었으니 잘 들어갔는지 출력을 해보자.

```c
void PrintAll() {
	if (g_pRoot == NULL) {
		printf("[PrintAll] Empty!\n");
		return;
	}
	PrintAllRecursive(g_pRoot);
	printf("\n");
}

void PrintAllRecursive(SNode* _pCurNode) {
	if (_pCurNode == NULL) return;

	PrintAllRecursive(_pCurNode->pLeft);
	printf("[%d] - ", _pCurNode->data);
	PrintAllRecursive(_pCurNode->pRight);
}
```

똑같이 재귀함수를 만들어 만들어진 트리를 출력한다. 작은 것부터 출력하는 함수이므로 재귀를 통해 pCurNode->pLeft의 값이 NULL이 될 때까지 함수를 실행하고 거꾸로 돌아오며 현재 노드의 데이터를 출력한다. 그리고 오른쪽 노드 또한 재귀를 사용해 작은 것부터 출력을 반복하도록 한다. 왼쪽 노드, 현재 노드, 오른쪽 노드의 순서대로 출력하는 것이다.

출력도 해봤으니 이제 삭제를 해보도록 하자.

```c
void RemoveAll() {
	if (g_pRoot == NULL) return;

	RemoveAllRecursive(g_pRoot);
	g_pRoot = NULL;
}

void RemoveAllRecursive(SNode* _pCurNode) {
	if (_pCurNode == NULL) return;

	RemoveAllRecursive(_pCurNode->pLeft);
	RemoveAllRecursive(_pCurNode->pRight);

	printf("[RemoveAll] %d\n", _pCurNode->data);
	SAFE_FREE(_pCurNode);
}
```

똑같이 현재 노드가 NULL이 될 때까지 재귀 함수를 반복해 NULL이 되면 그 노드를 할당 해제하는 코드다. 이 때 위의 출력처럼 왼쪽 노드 다음에 현재 노드를 삭제해버리면 현재 노드의 오른쪽 노드로 갈 방법이 사라져 버리기 때문에 이번엔 왼쪽 노드, 오른쪽 노드, 현재 노드의 순서로 할당을 해제한다.

이제 이진 탐색 트리의 핵심인 탐색을 시작해보자.

```c
SNode* SearchNode(SNode* _pCurNode, int _target){
	if (_pCurNode == NULL) return NULL;

	if (_pCurNode->data == _target) {
		return _pCurNode;
	}
	else if (_pCurNode->data > _target) {
		return SearchNode(_pCurNode->pLeft, _target);
	}
	else {
		return SearchNode(_pCurNode->pRight, _target);
	}
}
```

현재 노드와 찾을 데이터를 입력받는 함수다. 당연히 현재 노드가 NULL이면 데이터를 찾고 자시고 할 것도 없기 때문에 NULL을 반환한다. 만약 현재 노드의 데이터가 찾을 데이터와 같다면 현재 노드를 반환하고, 작으면 재귀 함수를 이용해 왼쪽 노드를 다시 검사한다(데이터가 작으면 당연히 왼쪽 노드에 있을 거니까). 반대로 크다면 똑같이 재귀 함수를 이용해 오른쪽 노드를 다시 검사해서 같은 값이 나올 때까지 재귀를 반복한다.

오늘은 이진 탐색 트리만 알아봤지만 기회가 된다면 다른 트리들의 특징에 대해서도 포스팅 해보겠다.

본 포스팅의 일부 내용과 그림은 한빛미디어의 '이것이 자료구조+알고리즘이다 with C언어, 저자 박상현'에서 발췌했다. 본 포스팅은 어떤 방식으로도 블로그 소유자의 금전적 이익을 생성하지 않으며, 교육을 목적으로 써진 글임을 알린다.
