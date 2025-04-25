---
title: "25.03.11 / 다중 차원 배열(Multi Dimesion Array)"
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

우선 코드 전문을 먼저 살펴보자.

```c
#include <stdio.h>
#include <malloc.h>

void PrintArray2D(int _arr[][2], int _rowCnt, int _colCnt) {
    for (int i = 0; i < _rowCnt; ++i) {
        for (int j = 0; j < _colCnt; ++j) {
            printf("_arr[%d][%d] : %d\n", i, j, _arr[i][j]);
        }
    }
}

void PrintPointer2D(int** _pArr, int _rowCnt, int _colCnt) {
    for (int row = 0; row < _rowCnt; ++row) {
        for (int col = 0; col < _colCnt; ++col) {
            // printf("_pArr[%d][%d] : %d\n", row, col, _pArr[row][col]);
            printf("*(*(_pArr + %d) + %d) : %d\n", row, col, *(*(_pArr + row) + col));
        }
    }
}

void main() {
    // 다차원 배열 (Multi-Dimension Array)
    int arr2D[2][3] =
    {
        { 11, 12, 13 },
        { 21, 22, 23 },
    };

    // row : 행
    // col : 열(Column)
    for (int row = 0; row < 2; ++row) {
        for (int col = 0; col < 3; ++col) {
            printf("arr2D[%d][%d] : %d (%p)\n", row, col, arr2D[row][col], &arr2D[row][col]);
        }
    }
    printf("\n");

    printf("arr2D[0][5] : %d\n", arr2D[0][5]);
    printf("arr2D[5] : %d\n", arr2D[5]);
    printf("\n");

    printf("arr2D : %p\n", arr2D);
    printf("arr2D[0] : %p\n", arr2D[0]);
    printf("&arr2D[0][0] : %p\n", &arr2D[0][0]);
    printf("arr2D + 1 : %p\n", arr2D + 1);
    printf("*(arr2D + 1) + 1 : %p\n", *(arr2D + 1) + 1);
    printf("\n");

    int arr1D[6] = { 11, 12, 13, 21, 22, 23 };
    int* pArr1D[2] = { &arr1D[0], &arr1D[3] };

    printf("&arr1D[0] : %p\n", &arr1D[0]);
    printf("pArr10[0] : %p\n", pArr1D[0]);
    printf("pArr1D[1][0] : %d\n", pArr1D[1][0]);
    printf("\n");

    int array2D[3][2] = { 11, 12, 21, 22, 31, 32 };
    PrintArray2D(array2D, 3, 2);
    // PrintPointer2D(array2D, 3, 2);	// 실행 실패
    printf("\n");

    // 2차원 배열의 동적할당
    int* pArray = (int*)malloc(sizeof(int) * 6);
    for (int i = 0; i < 6; ++i) {
        *(pArray + i) = i + 1;
    }

    int** pArray2D = (int**)malloc(sizeof(int*) * 3);
    *(pArray2D + 0) = pArray + 0;
    *(pArray2D + 1) = pArray + 2;
    *(pArray2D + 2) = pArray + 4;

    // PrintArray2D(pArray2D, 3, 2);	// 실행 실패
    printf("\n");
    PrintPointer2D(pArray2D, 3, 2);

    // 동적 할당 해제, 생성의 역순으로
    if (pArray2D != NULL) {
        free(pArray2D);
        pArray2D = NULL;
    }
    if (pArray != NULL) {
        free(pArray);
        pArray = NULL;
    }
}
```

```c
void PrintArray2D(int _arr[][2], int _rowCnt, int _colCnt) {
    for (int i = 0; i < _rowCnt; ++i) {
        for (int j = 0; j < _colCnt; ++j) {
            printf("_arr[%d][%d] : %d\n", i, j, _arr[i][j]);
        }
    }
}
```

고정 크기 2차원 배열을 출력하는 함수이다. 매개변수 \_arr\[\]\[2\]를 보면 열의 크기를 지정해줬는데, 이렇게 열의 크기를 지정하는 이유는 컴파일러가 메모리 레이아웃(Memory Layout)을 알아야 하기 때문이다. 만약 \_arr\[\]\[\]처럼 열 크기를 지정하지 않으면 컴파일러는 메모리에서 다음 행이 어디서 시작하는지 주소를 계산할 수 없다.

```c
void PrintPointer2D(int** _pArr, int _rowCnt, int _colCnt) {
    for (int row = 0; row < _rowCnt; ++row) {
        for (int col = 0; col < _colCnt; ++col) {
            printf("*(*(_pArr + %d) + %d) : %d\n", row, col, *(*(_pArr + row) + col));
        }
    }
}
```

포인터 기반 2차원 배열을 출력하는 함수이다. int\*\* \_pArr은 포인터이다. 즉, 각 행이 포인터로 이루어져 있다는 말이다. \*(\*(\_pArr + row) + col)은 포인터 연산으로 해당하는 행과 열의 배열 값을 가져온다. int\*\* 타입으로 포인터를 받았으므로 행마다 포인터가 가리키는 메모리가 존재해야 한다. 그리고 포인터로 넘겨받는 배열은 반드시 동적 할당 되어 있는 배열이어야 한다.

```c
int arr2D[2][3] =
{
    { 11, 12, 13 },
    { 21, 22, 23 },
};

for (int row = 0; row < 2; ++row) {
    for (int col = 0; col < 3; ++col) {
        printf("arr2D[%d][%d] : %d (%p)\n", row, col, arr2D[row][col], &arr2D[row][col]);
    }
}

printf("arr2D[0][5] : %d\n", arr2D[0][5]);  // 경고 발생 가능 (범위를 벗어남)
printf("arr2D[5] : %d\n", arr2D[5]);         // 오류 발생 가능 (메모리 침범)
```

main 함수 내부의 내용이다. 2행 3열의 2차원 배열 arr2D를 선언하고 초기화한다. 그리고 for문을 이용해 배열의 값과 메모리 주소를 출력한다.

아래 두가지 printf는 주석에 적어놨듯 범위 이탈, 메모리 침범의 이유로 실행되지 못하는 코드이다.

```c
int arr1D[6] = { 11, 12, 13, 21, 22, 23 };
int* pArr1D[2] = { &arr1D[0], &arr1D[3] };

printf("pArr1D[1][0] : %d\n", pArr1D[1][0]);
```

포인터 배열을 선언하고 1차원 배열의 시작 주소를 저장하는 내용이다.

```c
int* pArray = (int*)malloc(sizeof(int) * 6);
for (int i = 0; i < 6; ++i) {
    *(pArray + i) = i + 1;
}

int** pArray2D = (int**)malloc(sizeof(int*) * 3);
*(pArray2D + 0) = pArray + 0;
*(pArray2D + 1) = pArray + 2;
*(pArray2D + 2) = pArray + 4;
```

동적 할당을 이용해 1차원 배열 pArray, 2차원 배열 pArray2D를 선언한 내용이다. malloc을 이용해 메모리 동적 할당 후 값을 초기화했다. pArray2D는 포인터의 배열로, 각 포인터에 1차원 배열의 특정 위치를 연결한다.

```c
if (pArray2D != NULL) {
    free(pArray2D);
    pArray2D = NULL;
}
if (pArray != NULL) {
    free(pArray);
    pArray = NULL;
}
```

생성한 배열의 역순으로 메모리를 해제한다. 항상 동적 할당된 메모리를 해제하는 걸 잊지말자.

main 함수 내부에 보면 실행 실패하는 코드들이 존재한다.

```c
void PrintPointer2D(int** _pArr, int _rowCnt, int _colCnt) {
    for (int row = 0; row < _rowCnt; ++row) {
        for (int col = 0; col < _colCnt; ++col) {
            // printf("_pArr[%d][%d] : %d\n", row, col, _pArr[row][col]);
            printf("*(*(_pArr + %d) + %d) : %d\n", row, col, *(*(_pArr + row) + col));
        }
    }
}

// 고정 크기 배열
int array2D[3][2] = { 11, 12, 21, 22, 31, 32 };
PrintArray2D(array2D, 3, 2);
// PrintPointer2D(array2D, 3, 2);	// 실행 실패
```

int array2D는 고정 크기로 선언한 배열이다. PrintPointer2D는 포인터 배열을 받아와 출력하는 함수이기 때문에 실행에 실패한다.

```c
void PrintArray2D(int _arr[][2], int _rowCnt, int _colCnt) {
    for (int i = 0; i < _rowCnt; ++i) {
        for (int j = 0; j < _colCnt; ++j) {
            printf("_arr[%d][%d] : %d\n", i, j, _arr[i][j]);
        }
    }
}

// 동적 할당 배열
int* pArray = (int*)malloc(sizeof(int) * 6);
for (int i = 0; i < 6; ++i) {
    *(pArray + i) = i + 1;
}

int** pArray2D = (int**)malloc(sizeof(int*) * 3);
*(pArray2D + 0) = pArray + 0;
*(pArray2D + 1) = pArray + 2;
*(pArray2D + 2) = pArray + 4;

// PrintArray2D(pArray2D, 3, 2);	// 실행 실패
PrintPointer2D(pArray2D, 3, 2);
```

반대로 int\* pArray는 포인터를 이용한 동적 할당 배열이다. PrintArray2D는 고정 크기 배열을 받아와 출력하는 함수이기 때문에 실행에 실패한다.

이번엔 고정 크기 배열과 동적 포인터 배열의 차이점을 알아보자. 고정 크기 배열과 동적 포인터 배열은 메모리 할당 방법, 메모리 관리, 유연성 등에서 차이를 보인다.

고정 크기 배열은 컴파일 타임에 크기가 결정되며, Stack 메모리에 할당된다.

```c
int arr[2][3] = {
    {1, 2, 3},
    {4, 5, 6}
};
```

이 배열의 크기는 \[2\]\[3\]으로 컴파일 타임에 크기가 정해진다.

| 인덱스        | 값  | 주소 |
| ------------- | --- | ---- |
| arr\[0\]\[0\] | 1   | 0x00 |
| arr\[0\]\[1\] | 2   | 0x04 |
| arr\[0\]\[2\] | 3   | 0x08 |
| arr\[1\]\[0\] | 4   | 0x0c |
| arr\[1\]\[1\] | 5   | 0x10 |
| arr\[1\]\[2\] | 6   | 0x14 |

고정 크기 배열은 배열의 크기를 컴파일 시점에 알아야 하고, 크기 변경이 불가능해 유연성이 부족하다는 단점이 있다. 그러나 동적 포인터 배열에 비교해 속도가 빨라 고정된 크기의 데이터 구조를 가진 배열, 예를 들어 행렬 등은 고정 크기 배열로 구현하면 좋다.

동적 포인터 배열은 런 타임에 크기 할당이 가능하고, Heap 메모리에 할당된다.

```c
int row = 2, col = 3;

// 행에 대한 포인터 배열 할당
int** arr = (int**)malloc(sizeof(int*) * row);

// 열에 대한 메모리 할당
for (int i = 0; i < row; ++i) {
    arr[i] = (int*)malloc(sizeof(int) * col);
}

// 값 할당
arr[0][0] = 1;
arr[0][1] = 2;
arr[0][2] = 3;
arr[1][0] = 4;
arr[1][1] = 5;
arr[1][2] = 6;
```

| 행            | 값  | 주소 |
| ------------- | --- | ---- |
| arr\[0\]\[0\] | 1   | 0x00 |
| arr\[0\]\[1\] | 2   | 0x04 |
| arr\[0\]\[2\] | 3   | 0x08 |
| arr\[1\]\[0\] | 4   | 0x0c |
| arr\[1\]\[1\] | 5   | 0x10 |
| arr\[1\]\[2\] | 6   | 0x14 |

따라서 메모리의 크기 변경이 가능하고, 배열의 유연성이 증가한다. 동적 포인터 배열은 만약 메모리 공간이 부족하다면 메모리 할당에 실패할 수 있어 유효성 검사가 필요하고, 반드시 free()를 통해 해제해야 한다는 특징이 있다. 크기가 변동될 가능성이 있는 배열의 경우 동적 포인터 배열을 사용하는 것이 유리하다.

| 구분             | 고정 크기 배열           | 동적 포인터 배열                 |
| ---------------- | ------------------------ | -------------------------------- |
| 메모리 할당 위치 | Stack                    | Heap                             |
| 크기 결정 시점   | 컴파일 타임              | 런타임                           |
| 런타임 크기 변경 | 불가능                   | 가능                             |
| 초기화 방식      | 선언 시 자동 초기화 가능 | malloc() 필요                    |
| 메모리 접근 속도 | 빠름                     | 느림                             |
| 메모리 해제      | 자동 해제                | free() 로 직접 해제 필요         |
| 사용 용도        | 크기가 고정된 경우       | 크기가 변동될 가능성이 있는 경우 |

대부분 속도가 중요한 경우 고정 크기 배열을 사용하고, 메모리 유연성이 중요한 경우 동적 포인터 배열을 사용한다. 두 방법의 장단점을 잘 따져 골라 사용하도록 하자.
