---
title: "25.03.11 / 동적 배열(Dynamic Array)"
excerpt: ""

categories:
  - Programming
tags:
  - []

toc: true
toc_sticky: true

date: 2025-03-11
last_modified_at: 2025-04-25
---

지난 글에 이어 동적 배열(Dynamic Array)를 알아보자.

```c
int arrLen = 4;
int* pArr = (int*)malloc(sizeof(int) * arrLen);

printf("pArr : %p\n", pArr);
printf("pArr + 1 : %p\n", pArr + 1);	// 메모리 주소 연산, 1을 더하면 1byte가 아닌 자료형 크기만큼 건너뛴다

if (pArr != NULL) {
    pArr[0] = 1;	// *(pArr + 0) = 1;
    pArr[1] = 2;	// *(pArr + 1) = 2;
    pArr[2] = 3;	// *(pArr + 2) = 3;
    pArr[3] = 4;	// *(pArr + 3) = 4;

    for (int i = 0; i < 4; ++i) {
        printf("pArr[%d] : %d\n", i, pArr[i]);
        printf("*(pArr + %d) : %d\n", i, *(pArr + i));
    }
}

// 출력값
// pArr : 00000217D8C1ED20
// pArr + 1 : 00000217D8C1ED24

// pArr[0] : 1
// *(pArr + 0) : 1

// pArr[1] : 2
// *(pArr + 1) : 2

// pArr[2] : 3
// *(pArr + 2) : 3

// pArr[3] : 4
// *(pArr + 3) : 4
```

배열의 크기 arrLen을 변수로 선언해 그 크기를 가지는 배열 pArr을 동적 할당으로 선언하는 코드이다. sizeof(int) \* arrLen의 크기를 가지는 int포인터형 포인터 pArr을 선언했다. 배열명은 결국 포인터 변수라 했기 때문에 pArr은 int 자료형 \* arrLen만큼의 크기를 가지는 배열이 된다.

printf를 사용해 pArr의 주소와 pArr + 1의 주소를 알아보면 int형의 크기인 4바이트만큼 차이가 나는 걸 알 수 있다.

pArr\[0\]은 pArr + 0이 가리키는 주소와 동일하기 때문에 pArr\[0\] = 1을 \*(pArr + 0) = 1로 선언할 수 있다. 동적 할당 한 경우 유효성 검사를 해야하는 걸 잊지말자.

동적 할당을 했으면 반환도 해야한다. 반환하는 코드를 살펴보자.

```c
#include <stdio.h>
#include <malloc.h>

// Macro Function
#define SAFE_FREE(p) { if (p) { free(p); p = NULL; } }
#define SAFE_FREE2(p)\
do {\
    if (p) {\
        free(p);\
        p = NULL;\
    }\
} while(0)

void main() {
    // 동적 할당
    int arrLen = 4;
    int* pArr = (int*)malloc(sizeof(int) * arrLen);

    if (pArr != NULL) {
        pArr[0] = 1;
        pArr[1] = 2;
        pArr[2] = 3;
        pArr[3] = 4;
    }

    // 동적 할당 해제
    // if (pArr != NULL) {
    //    free(pArr);
    //    pArr = NULL;
    // }

    SAFE_FREE(pArr);
```

if (pArr != NULL)의 코드를 항상 다 적기엔 불편하다. 그래서 define을 이용해 매크로 함수(Macro Function) SAFE_FREE를 구현했다. 매크로 함수가 아닌 그냥 함수로 만들면 안될까? 안되는건 아니지만, 이런 함수는 매크로 함수로 선언하는 걸 추천한다. 함수는 호출할 때마다 비용이 든다 (Function Call Overhead). 이렇게 계속 쓰는 코드를 함수로 처리하면 비용이 계속 늘어나는 것이다. 매크로 함수는 함수를 호출하는 게 아니라 코드를 치환해서 작동하는 방식이기 때문에 비용이 비교적 덜 든다는 장점이 있어 이럴 때 사용하는 것이다.
