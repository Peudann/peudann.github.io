---
title: "25.03.12 / 2중 포인터와 함수를 이용한 문자열의 동적 할당과 복사"
excerpt: ""

categories:
  - Programming
tags:
  - []

toc: true
toc_sticky: true

date: 2025-03-12
last_modified_at: 2025-04-25
---

문자열 글에서 만들었던 문자열을 복사하는 함수 StrCpy를 사용해 동적 할당 문자열을 알아보자.

```c
char* StrCpy(char* _pDst, const char* _pSrc) {
    if (_pDst != NULL) {
        free(_pDst);
        _pDst = NULL;
    }

    int strLen = strlen(_pSrc);
    _pDst = (char*)malloc(strLen + 1);
    if (_pDst != NULL) {
        strcpy(_pDst, _pSrc);
    }

    return _pDst;
}

int main() {
    char* pBuf = NULL;
    pBuf = StrCpy(pBuf, "hello");
    printf("%s\n", pBuf); // 예상 출력: "hello"
}
```

이런 코드를 사용해 pBuf를 동적 할당하고 hello를 복사하려 시도했다. 하지만 pBuf를 출력해도 hello가 출력되지 않는데 그 이유는 무엇일까?

우선 포인터의 기본 개념을 알아야한다. 포인터란 메모리의 주소를 저장하는 변수이다. 일반 변수는 값을 저장하지만, 포인터는 그 값이 저장된 메모리의 주소를 저장하는 것이다.

```c
int num = 10;
int* p = &num; // num의 주소를 p에 저장

printf("%d\n", num);  // 값 출력 → 10
printf("%p\n", &num); // num의 주소 출력
printf("%p\n", p);    // p에 저장된 주소 출력 → num의 주소
printf("%d\n", *p);   // p가 가리키는 값 출력 → 10
```

p에는 num의 메모리 주소가 저장되고, \*p를 통해 해당 주소에 저장된 값인 10에 접근 가능하다. 이를 인지하고 아래의 코드를 봐보자.

```c
void modify(char* p) {
    p = (char*)malloc(10); // 함수 내부에서 p 수정
}

int main() {
    char* str = NULL;
    modify(str);
    if (str == NULL) {
        printf("str is still NULL\n"); // str은 여전히 NULL
    }
    return 0;
}
```

포인터 변수 str을 NULL로 선언했다. 그 후에 modify 함수를 사용해 입력받은 str을 동적 할당하려 했지만 실패했다. 이유가 무엇일까?

이유는 C언어의 특징에 있다. C언어는 모든 함수 인자가 기본적으로 값에 의한 전달(Call by Value)로 전달된다. 즉, 함수를 호출할 때 인자의 복사본이 함수 내부로 전달되므로, 함수 내부에서 인자의 값을 변경해도 main에는 반영되지 않는다는 거다.

그래서 포인터 변수 str을 char\* p의 방식으로 전달하면 p의 값(메모리 주소)만 복사되어 함수 내에서 수정하더라도 원본 포인터 str에는 영향을 미치지 못한다는 것이다.

이를 해결하기 위해 2차원 포인터를 사용한다. 수정된 코드를 한번 보도록 하자.

```c
char* StrCpy(char** const _pDst, const char* const _pSrc) {
    if (_pDst == NULL || _pSrc == NULL) return NULL;	// 예외 처리

    if ((*_pDst) != NULL) {
        free(*_pDst);
        *_pDst = NULL;
    }
    int strLen = 0;
    // hello
    while (1) {
        if (*(_pSrc + strLen) == '\0') break;
        ++strLen;
    }

    *_pDst = (char*)malloc(sizeof(char) * (strLen + 1));

    if (*_pDst != NULL) {
        for (int i = 0; i < strLen; ++i) *((*_pDst) + i) = *(_pSrc + i);
        *((*_pDst) + strLen) = '\0';
    }

    return *_pDst;
}

void main() {
    char* pBuf = NULL;
    char* pSource = "hello";

    StrCpy(&pBuf, pSource);
    printf("StrCpy : %s", pBuf);

    printf("free(*_pDst)\n");
    if (pBuf != NULL) {
        free(pBuf);
        pBuf = NULL;
    }
}
```

StrCpy 함수에선 동적 할당을 위해 새로운 메모리 공간을 할당하고, 이 주소값을 main에 있는 포인터 변수에 저장하게 된다. 이를 위해 호출자 포인터의 주소(&pBuf)를 전달받는데, 이 때 함수의 매개변수를 char\*\* const \_pDst로 선언해 포인터의 포인터를 받게 된다.

main 함수 내에서 pBuf는 char\* 포인터 변수인데, &pBuf는 char\*\* 포인터 변수가 되는거다. 이렇게 함으로써 \*\_pDst에 할당된 메모리 주소를 변경하면, 그 변경이 main에 반영된다.

아직 잘 이해가 안되는 것 같은가? 조금 더 자세히 알아보자.

```c
void asdf(int x) {
    x = 10;
}

void main() {
    int a = 3;
    asdf(a);
    printf("%d", a); // 3 출력
}
```

위에서 설명했듯 C언어에서 함수의 매개변수는 모두 값에 의한 전달이다. 즉 함수를 호출 할 때 원본 변수의 값이 복사되어 함수의 매개변수에 전달된다. 그래서 asdf(a)를 한다해서 main 함수에 존재하는 a의 값이 10으로 바뀌지 않는다. a의 값 3이 복사되어 x에 전달된 거기 때문에 원본에 영향을 미치지 못하는 것이다.

이 원리는 포인터를 함수 인자로 받을 때도 마찬가지이다.

```c
void func(char* ptr) {
    // ptr을 다른 주소로 바꿔도
    // 원래 main에 있는 그 포인터 변수는 바뀌지 않음
    ptr = (char*)malloc(10);
}

void main() {
    char* pBuf = NULL;
    func(pBuf);
    // main의 pBuf 값은 여전히 NULL
}
```

func가 받는 매개변수 ptr 또한 pBuf의 값(NULL)의 복사본이다. 함수 내에서 ptr을 아무리 지지고 볶아도 밖에 있는 main의 pBuf는 변하지 않는다는 것이다.

즉 함수 안에서 포인터의 값 자체를 수정해 원본 포인터(pBuf)를 수정하고 싶다면, 우리가 새로 할당한 주소를 pBuf가 갖게 해주고 싶다면, 포인터를 가리키는 포인터, 즉 이중 포인터를 사용해야 한다는 것이다.

```c
char* StrCpy(char** const _pDst, const char* const _pSrc) {
    if (_pDst == NULL || _pSrc == NULL) return NULL;	// 예외 처리

    if ((*_pDst) != NULL) {
        free(*_pDst);
        *_pDst = NULL;
    }
    int strLen = 0;
    // hello
    while (1) {
        if (*(_pSrc + strLen) == '\0') break;
        ++strLen;
    }

    *_pDst = (char*)malloc(sizeof(char) * (strLen + 1));

    if (*_pDst != NULL) {
        for (int i = 0; i < strLen; ++i) *((*_pDst) + i) = *(_pSrc + i);
        *((*_pDst) + strLen) = '\0';
    }

    return *_pDst;
}
```

자 그래서 왜 StrCpy는 char\*\* const \_pDst를 매개변수로 받는가? 이 또한 새로 동적 할당 된 주소를 main의 원본 포인터(pBuf)가 갖도록 하기 위해 함수 내부에서 pBuf라는 포인터를 직접 바꾸기 위해서다.

만약 char\* const \_pDst를 매개변수로 받게 되면 그건 포인터 값(NULL)의 복사본일 뿐이고, 복사된 포인터 값에 무엇을 하든 함수를 빠져나갈 때까지의 변경 사항이 원본에는 저장되지 않는다.

원본 포인터를 직접 수정해야하기 때문에 이중 포인터 char\*\* \_pDst로 받게 되는 것이다. \_pDst는 pBuf의 주소를 보관하므로 \*\_pDst(pBuf의 주소에 저장된 값)에 새로운 메모리 주소를 대입하면 이 대입이 실제로 pBuf에 영향을 미치게 된다.

함수 내부에서 \*\_pDst를 사용하는 이유가 바로 이것이다. \_pDst는 pBuf의 주소를 받고, 따라서 \*\_pDst는 실제 pBuf의 값을 가리키게 된다.

```c
void main() {
    char* pBuf = NULL;
    char* pSource = "hello";

    StrCpy(&pBuf, pSource);
    printf("StrCpy : %s", pBuf);

    printf("free(*_pDst)\n");
    if (pBuf != NULL) {
        free(pBuf);
        pBuf = NULL;
    }
}
```

그래서 main 함수에서 StrCpy를 호출할 때 &pBuf(pBuf의 주소)를 인자로 던져야 하는 것이다. \_pDst가 pBuf의 주소값을 가져야 \*\_pDst를 통해 pBuf를 수정할 수 있다(pBuf의 주소가 가지는 값이 우리가 바꾸려하는 내용이기 때문).

예외 처리에 대한 것까지 생각하면 더 어려우니 일단 이번 글에선 설명을 생략했다. 또한 나도 완벽하게 이해한 내용이 아니라 설명이 부족할 수도 있다. 어려울 수 있는 내용이니 천천히 생각해보고 많은 예제를 써보며 이해하도록 노력해보자.

챗지피티의 2중 포인터에 대한 설명이다. 읽어보는 게 도움이 될 것이다.

2중 포인터(Double Pointer)는 “포인터를 가리키는 포인터”를 의미합니다. 즉, 2중 포인터 변수는 1중 포인터(일반적인 포인터 변수)의 주소값을 저장합니다. 이 개념을 이해하려면 먼저 아래 단계로 나누어 생각해봅시다.

---

**1\. 1중 포인터(일반 포인터)**

• 예를 들어 int \*p 라고 했을 때, p는 정수형 변수를 가리키는(주소를 저장하는) 포인터입니다.

```c
int x = 10;
int *p = &x; // p는 x의 주소를 저장하고, *p는 x의 값(10)을 참조
```

• 이때 p는 ‘어떤 정수형 변수가 있는 메모리 공간의 주소’를 저장하고 있고, \*p는 실질적인 그 변수의 값을 가져옵니다.

---

**2\. 2중 포인터(Double Pointer)**

• int \*\*pp 라고 선언하면, pp는 “정수형 포인터의 주소”를 저장하는 포인터가 됩니다. 즉, “포인터를 가리키는 포인터”입니다.

```c
int x = 10;
int *p = &x;    // p: x의 주소를 저장
int **pp = &p;  // pp: p의 주소를 저장

printf("x   = %d\n", x);      // x의 값 (10)
printf("*p  = %d\n", *p);     // p가 가리키는 곳의 값 -> x의 값 (10)
printf("**pp = %d\n", **pp);  // pp가 가리키는 곳(p)이 가리키는 곳(x)의 값 (10)
```

• 시각적으로 표현해 보면,

```text
  x  (정수값 10 저장)
  ^
  | (포인터 p가 x의 주소를 가리킴)
  p  (p 자체는 주소 변수, 값: &x)
  ^
  | (포인터 pp가 p의 주소를 가리킴)
  pp (pp 자체는 주소 변수, 값: &p)
```

---

**3\. 예시 코드로 확인해보기**

아래는 2중 포인터를 선언하고 사용하는 간단한 예시입니다.

```c
#include <stdio.h>

int main(void) {
    int a = 100;
    int *p = &a;    // 1중 포인터: a의 주소를 저장
    int **pp = &p;  // 2중 포인터: p의 주소를 저장

    printf("a    의 값: %d\n", a);
    printf("*p   의 값: %d\n", *p);
    printf("**pp 의 값: %d\n", **pp);

    // 값을 변경해보기
    **pp = 200;  // pp를 통해 a의 값을 바꿀 수 있음

    printf("\n값 변경 후:\n");
    printf("a    의 값: %d\n", a);
    printf("*p   의 값: %d\n", *p);
    printf("**pp 의 값: %d\n", **pp);

    return 0;
}
```

**결과 예시**

```text
a    의 값: 100
*p   의 값: 100
**pp 의 값: 100

값 변경 후:
a    의 값: 200
*p   의 값: 200
**pp 의 값: 200
```

• \*\*pp = 200; 과 같이 2중 포인터를 사용하면 결국 a의 값이 변경됩니다.

• 즉, pp → p → a 형태로 연결되어, pp를 통해 최종 변수 a에 접근할 수 있다는 의미입니다.

---

**4\. 2중 포인터를 어디에 쓰는가?**

1. **동적 메모리에서 2차원 배열처럼 활용**

• 예: int \*\*arr = malloc(sizeof(int\*) \* N); 이런 식으로 “포인터 배열”에 대한 메모리를 잡고, 각 원소를 다시 malloc해서 2차원 배열처럼 사용합니다.

2. **함수에서 포인터의 값을 수정하려고 할 때**

• 함수 인자로 2중 포인터를 넘겨주면, 함수 내부에서 1중 포인터(주소) 자체를 변경할 수 있습니다. 예를 들어, 동적 할당 결과를 함수 밖에서도 유지하려고 할 때 유용합니다.

```c
void allocateMemory(int **pp) {
    *pp = malloc(sizeof(int));
    **pp = 999;
}

// main 함수
int main(void) {
    int *p = NULL;
    allocateMemory(&p);  // &p -> pp
    printf("%d\n", *p);  // 999
    free(p);
    return 0;
}
```

---

**5\. 핵심 정리**

• **1중 포인터**: 변수를 가리킨다(주소를 저장한다).

• **2중 포인터**: 포인터 자체를 가리킨다(포인터의 주소를 저장한다).

이렇게 2중 포인터는 “포인터를 간접적으로 한 단계 더 추적할 수 있는” 도구이며, 동적 메모리 관리나 “포인터의 주소값 변경”을 위한 목적으로 자주 활용됩니다. 한 번에 많은 것을 이해하려 하기보다는, 단계를 나누어 (변수 → 포인터 → 포인터의 포인터) 순으로 연결 관계를 시각적으로 그려보며 익히는 것이 좋습니다.
