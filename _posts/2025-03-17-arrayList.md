---
title: "25.03.17 / ArrayList"
excerpt: ""

categories:
  - Programming
  - Data Structure and Algorithm
tags:
  - []

toc: true
toc_sticky: true

date: 2025-03-17
last_modified_at: 2025-04-25
---

이번 글에선 ArrayList에 대해 알아보자. C++에선 STL의 Vector이고, C#에선 Collections의 ArrayList이다.

ArrayList는 배열은 배열인데 가변 크기의 배열을 나타낸다. 만약 4의 크기를 가지고 있는 배열이라면 4개의 요소를 넣고 5번째 요소를 넣을 때 자동으로 배열의 크기가 확장되고 요소가 들어가는 기능을 가진 배열이라는 말이다.

우선 ArrayList를 만드는 것부터 시작해보자.

```c
void CreateArrayList(int** const _pArrList, const int _capacity) {
    // 배열의 주소를 담을 공간이 NULL인지 검사
    if (_pArrList == NULL) return;
    // 공간 안에 있는 배열의 주소가 NULL이 아닌지 검사
    if ((*_pArrList) != NULL) SAFE_FREE((*_pArrList));

    (*_pArrList) = (int*)malloc(sizeof(int) * _capacity);
    if ((*_pArrList) != NULL) printf("[CreateArrayList] Capacity : %d\n", _capacity);
    else printf("[CreateArrayList] Memory Allocate failed!\n");
}
```

입력받은 \_capacity의 크기만큼을 동적 할당 받을 배열 \_pArrList를 입력 받는다. 만들어질 배열의 주소를 담을 공간이 NULL이면 당연히 값이고 뭐고 안들어가기 때문에 함수를 종료한다. 또 \_pArrList의 값(공간 안에 있는 배열의 주소)이 초기에 NULL이 아니면 쓰레기 값이 담겨 있는거기 때문에 free로 메모리를 해제했다가 다시 int \* \_capacity의 크기만큼 동적 할당 해준다. 그리고 동적 할당에 성공했다면 안의 값이 NULL이 아닐 것이기 때문에 할당 된 용량을 출력했다. 만약 모종의 사유로 동적 할당이 실패했다면 실패 사실을 알린다.

다음은 ArrayList를 출력하는 함수이다.

```c
void PrintArrayList(const int* const _pArrList, int _curIdx, const int _capacity) {
    if (_pArrList == NULL || _curIdx < 0 || _capacity < 1 || _curIdx >= _capacity) return;
    if (_curIdx == 0) {
        printf("[PrintArrayList] List is Empty!\n");
        return;
    }
    for (int i = 0; i < _curIdx; ++i) {
        printf("%d - ", *(_pArrList + i));
    }
    printf("(%d / %d)\n", _curIdx, _capacity);
}
```

입력받은 값들이 유효한지 검사해 유효하지 않으면 반환한다. \_curIdx가 0이라면 추가한 요소가 없는 것이므로 ArrayList가 비어있다고 알린다. 그리고 현재 인덱스까지 반복하며 \_pArrList 배열에 저장된 데이터를 출력한다. 마지막으로 총 용량 대비 몇 칸을 썼는지 출력한다.

다음은 가장 후단에 데이터를 추가하는 함수이다.

```c
void AddBack(int** const _pArrList, int* const _pCurIdx, int* const _pCapacity, int _data) {
    if (_pArrList == NULL || (*_pArrList) == NULL || _pCurIdx == NULL || (*_pCurIdx) < 0 || _pCapacity == NULL || (*_pCapacity) < 1) return;

    // Pseudo Code(의사 코드)
    // 1. 만약에 현재 인덱스가 배열 길이와 같은 경우
    if ((*_pCurIdx) == (*_pCapacity)) {
        // 배열 확장
        // 1. _pCapacity *= 2;
        (*_pCapacity) <<= 1;
        // 2. 현재 배열 길이의 2배인 배열 동적 할당
        int* arrNew = (int*)malloc(sizeof(int) * (*_pCapacity));
        // 3. 현재 배열의 값을 새 배열에 복사(_pCurIdx까지)
        for (int i = 0; i < (*_pCurIdx); ++i) {
            *(arrNew + i) = *((*_pArrList) + i);
        }
        // 4. 기존 배열 해제
        SAFE_FREE(*_pArrList);
        // 5. _pArrList에 새 배열 주소 담기
        (*_pArrList) = arrNew;
        printf("[AddBack] ArrayList extend : %d -> %d\n", (*_pCapacity) >> 1, (*_pCapacity));
    }
    // 2. 그게 아니면 현재 인덱스가 배열 길이보다 큰 경우
    else if ((*_pCurIdx) > (*_pCapacity)) {
        // 처리 안함
        printf("[AddBack] : Out of range!\n");
        return;
    }

    *((*_pArrList) + ((*_pCurIdx)++)) = _data;
}
```

배열명, 현재 인덱스, 최대 용량, 넣을 데이터를 입력받아 유효성을 검사한다. 그 이후는 주석에 달린 순서대로 진행하면 된다. 현재 인덱스가  최대 용량과 같으면, 다시 말해 이번에 넣을 데이터가 최대 용량을 넘어서는 인덱스 + 1번째 데이터면 시프트 연산으로 입력받은 최대 용량을 두 배로 늘리고, 늘린 최대 용량만큼의 크기를 가지는 새로운 배열을 생성한다. 그리고 반복문을 이용해 새로운 배열에 기존 배열의 데이터를 복사한다(깊은 복사를 사용한다). 그리고 기존의 배열이 가리키는 배열을 동적 해제하고 새로 만든 배열의 주소를 다시 담는다. 그리고 \_pArrList가 가리키는 배열에 외부의 curIdx를 증가시키며 그 curIdx++번째의 인덱스에 새로 넣을 \_data를 삽입한다.

만약 현재 인덱스가 배열 길이보다 긴 경우, 우리가 의도하는 바가 아니기 때문에 범위를 벗어남을 알리고 반환한다.

메인 코드를 실행한 결과이다.

```c
void main() {
    int* pArrList = NULL;
    int capacity = 3;
    int curIdx = 0;

    CreateArrayList(&pArrList, capacity);

    AddBack(&pArrList, &curIdx, &capacity, 11);
    AddBack(&pArrList, &curIdx, &capacity, 22);
    AddBack(&pArrList, &curIdx, &capacity, 33);
    AddBack(&pArrList, &curIdx, &capacity, 44);

    PrintArrayList(pArrList, curIdx, capacity);

    SAFE_FREE(pArrList);
}
```

```text
실행 결과

[CreateArrayList] Capacity : 3
[AddBack] ArrayList extend : 3 -> 6
11 - 22 - 33 - 44 - (4 / 6)
```

위에서 설명한 함수들이 공통적으로 배열명, 현재 인덱스, 총 용량을 매개변수로 받아 실행된다. 이럴 땐 구조체를 사용해 매개변수의 수를 줄일 수 있다. 그리고 자주 하는 유효성 검사를 함수로 만들어 여러번 반복 사용하기 쉽게 만들었다.

```c
#include <stdio.h>
#include <stdlib.h>
#include <memory.h>

#define SAFE_FREE(p) { if (p) { free(p); p = NULL; }}

typedef struct _SArrarList {
    int* pArr;
    int capacity;
    int curIdx;
} SArrayList;

void CreateArrayList(SArrayList* const _pArrList, int _arrLen);
void AddBack(SArrayList* const _pArrList, int _data);
void PrintArrayList(SArrayList _arrList);

int InValidWith(const SArrayList* const _pArrList) {
    return (_pArrList == NULL || _pArrList->pArr == NULL || _pArrList->curIdx < 0 || _pArrList->capacity < 1);
}

void main() {
    SArrayList* pArrList = (SArrayList*)malloc(sizeof(SArrayList));
    memset(pArrList, 0, sizeof(SArrayList));
    pArrList->pArr = NULL;
    pArrList->capacity = 0;
    pArrList->curIdx = 0;

    CreateArrayList(pArrList, 3);

    AddBack(pArrList, 11);
    AddBack(pArrList, 22);
    AddBack(pArrList, 33);
    AddBack(pArrList, 44);

    PrintArrayList(*pArrList);

    SAFE_FREE(pArrList);

}

void CreateArrayList(SArrayList* const _pArrList, int _arrLen) {
    if (_pArrList == NULL) return;
    if (_pArrList->pArr) SAFE_FREE(_pArrList->pArr);

    _pArrList->pArr = (int*)malloc(sizeof(int) * _arrLen);

    if (_pArrList->pArr != NULL) {
        _pArrList->capacity = _arrLen;
        _pArrList->curIdx = 0;
        printf("[CreateArrayList] Capacity : %d\n", _pArrList->capacity);
    }
    else printf("[CreateArrayList] Memory Allocate failed!\n");
}

void AddBack(SArrayList* const _pArrList, int _data) {
    if (InValidWith(_pArrList)) return;
    if (_pArrList->curIdx == _pArrList->capacity) {
        _pArrList->capacity <<= 1;

        int* arrNew = (int*)malloc(sizeof(int) * _pArrList->capacity);
        for (int i = 0; i < _pArrList->curIdx; ++i) {
            *(arrNew + i) = *(_pArrList->pArr + i);
        }
        SAFE_FREE(_pArrList->pArr);
        _pArrList->pArr = arrNew;
        printf("[AddBack] ArrayList extend : %d -> %d\n", _pArrList->capacity >> 1, _pArrList->capacity);
    }
    else if (_pArrList->curIdx > _pArrList->capacity) {
        printf("[AddBack] : Out of range!\n");
        return;
    }

    *(_pArrList->pArr + _pArrList->curIdx) = _data;
    ++_pArrList->curIdx;
}

void PrintArrayList(SArrayList _arrList) {
	if (InValidWith(&_arrList) || _arrList.curIdx >= _arrList.capacity) return;
	if (_arrList.curIdx == 0) {
		printf("[PrintArrayList] List is Empty!\n");
		return;
	}
	for (int i = 0; i < _arrList.curIdx; ++i) {
		printf("%d - ", *(_arrList.pArr + i));
	}
	printf("(%d / %d)\n", _arrList.curIdx, _arrList.capacity);
}
```
