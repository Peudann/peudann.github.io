---
title: "25.03.19 / 버블 정렬(Bubble Sort)"
excerpt: ""

categories:
  - Programming
  - Data Structure and Algorithm
tags:
  - []

toc: true
toc_sticky: true

date: 2025-03-19
last_modified_at: 2025-04-29
---

버블 정렬(Bubble Sort)라는 이름은 물 속 깊은 곳에서 수면을 향해 올라오는 거품의 모습처럼 데이터를 정렬하기에 붙여졌다고 한다. 이름의 유래는 해석하기 나름이겠지만, 참고하고 있는 책의 설명을 가져와봤다.

버블 정렬을 자료구조를 순회하면서 이웃한 요소들끼리 데이터를 교환하여 정렬을 수행한다. 우리는 오름차순으로 정렬하고 있는 중이므로 왼쪽에 있는 요소가 오른쪽에 있는 요소보다 작아야 한다. 제일 왼쪽에 있는 요소부터 오른쪽에 이웃하는 요소와 데이터를 비교해나가면 된다. 결국 배열에서 가장 큰 원소가 매 회전마다 한 칸씩 이동하여 결국 맨 끝 위치에 도달하게 되는 것이다.

버블 알고리즘은 이해하기도 쉽고 만들기도 쉬워서 정렬 알고리즘을 만들기 시작할 때 처음 배우기 좋은 정렬 방식이다. 비효율적이라는 단점이 있어 자주 사용되진 않지만 알고 있으면 다른 정렬 알고리즘을 이해하는 데 도움을 줄 것이다.

아래 코드를 보며 버블 정렬을 알아보자.

```c
void BubbleSort(int _arr[], int _len) {
    // 안전을 위해 배열 포인터와 길이에 대한 예외 처리
    if (_arr == NULL || _len < 0) return;

    // 배열의 끝에서부터 시작하여, 왼쪽으로 진행
    for (int i = _len - 1; i > 0; --i) {
        // 0번 인덱스부터 i-1번 인덱스까지 검사
        for (int j = 0; j < i; ++j) {
            if (_arr[j] > _arr[j + 1]) {
                // 교환
                int tmp = _arr[j];
                _arr[j] = _arr[j + 1];
                _arr[j + 1] = tmp;
            }
        }
        // 매 회전이 끝날 때마다 중간 결과를 출력
        PrintAll(_arr, _len);
    }
}
```

우선 배열 포인터가 유효하지 않거나, 길이가 0보다 작다면 함수를 종료한다.

바깥쪽 for 반복문(i)에선 배열의 마지막 인덱스부터 1까지 감소해 이 루프가 전체 몇 바퀴를 돌건지 결정한다. 각 바퀴가 끝날 때마다, 오른쪽 끝에서부터 차례대로 정렬이 확정된 원소가 하나씩 늘어난다.

안쪽 for 반복문(j)에선 0부터 i - 1(i가 한 바퀴 돌면 가장 오른쪽은 이미 정렬된 값이니까 바깥 반복문에서 i가 1 감소한다)까지 순회해 매 루프마다 인접한 두 원소 \_arr\[j\]와 \_arr\[j + 1\]을 비교해 \_arr\[j\]가 \_arr\[j + 1\]이면 두 값을 교환한다. 이 과정을 통해 더 큰 값이 오른쪽으로 한 칸씩 이동하게 된다. 디버깅을 위해 매 회전이 끝날 때마다 중간 결과를 출력하는 PrintAll을 작성했다.

이제 결과를 알아보자.

```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

void BubbleSort(int _arr[], int _len);
void PrintAll(int _arr[], int _len);
void Swap(int* _pLhs, int* _pRhs) {
	int tmp = *_pLhs;
	*_pLhs = *_pRhs;
	*_pRhs = tmp;
}
void SwapWithArray(int _arr[], int _lIdx, int _rIdx) {
	int tmp = _arr[_lIdx];
	_arr[_lIdx] = _arr[_rIdx];
	_arr[_rIdx] = tmp;
}

int main() {
	// 버블 정렬(Bubble Sort)
	int arr[10] = { 0 };
	srand((unsigned int)time(NULL));
	for (int i = 0; i < 10; ++i) {
		arr[i] = rand() % 30;
	}

	PrintAll(arr, 10);
	printf("\n");

	BubbleSort(arr, 10);
	printf("\n");

	PrintAll(arr, 10);


	return 0;
}

void BubbleSort(int _arr[], int _len) {
	if (_arr == NULL || _len < 0) return;

	for (int i = _len - 1; i > 0; --i) {
		for (int j = 0; j < i; ++j) {
			if (_arr[j] > _arr[j + 1]) {

				int tmp = _arr[j];
				_arr[j] = _arr[j + 1];
				_arr[j + 1] = tmp;

                // 스왑 함수를 따로 만들어 사용할 수도 있다
				// Swap(&_arr[j], &_arr[j + 1]);
				// SwapWithArray(_arr, j, j + 1);
			}
		}
		PrintAll(_arr, _len);
	}
}

void PrintAll(int _arr[], int _len) {
	if (_arr == NULL || _len < 0) return;

	for (int i = 0; i < _len; ++i) {
		printf("%d - ", _arr[i]);
	}
	printf("(%d)\n", _len);
}
```

```text
실행 결과

0 - 15 - 1 - 16 - 16 - 19 - 11 - 25 - 4 - 0 - (10)

0 - 1 - 15 - 16 - 16 - 11 - 19 - 4 - 0 - 25 - (10)
0 - 1 - 15 - 16 - 11 - 16 - 4 - 0 - 19 - 25 - (10)
0 - 1 - 15 - 11 - 16 - 4 - 0 - 16 - 19 - 25 - (10)
0 - 1 - 11 - 15 - 4 - 0 - 16 - 16 - 19 - 25 - (10)
0 - 1 - 11 - 4 - 0 - 15 - 16 - 16 - 19 - 25 - (10)
0 - 1 - 4 - 0 - 11 - 15 - 16 - 16 - 19 - 25 - (10)
0 - 1 - 0 - 4 - 11 - 15 - 16 - 16 - 19 - 25 - (10)
0 - 0 - 1 - 4 - 11 - 15 - 16 - 16 - 19 - 25 - (10)
0 - 0 - 1 - 4 - 11 - 15 - 16 - 16 - 19 - 25 - (10)

0 - 0 - 1 - 4 - 11 - 15 - 16 - 16 - 19 - 25 - (10)
```

버블 정렬은 코드가 단순하고 직관적이지만, 평균-최악 시간 복잡도가 O(n^2)이기 때문에 배열의 길이가 길어지면 비효율적이다. 그래도 알고리즘 학습 초기엔 가장 이해하기 쉬운 정렬 중 하나이고, 구현도 간단하니 한 번쯤은 직접 만들어보는 시간을 가지자.
