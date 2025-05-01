---
title: "25.03.19 / 퀵 소트(Quick Sort)"
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

드디어 정렬계의 스테디셀러 퀵 정렬(Quick Sort)에 대해 알아볼 때가 됐다.

퀵 정렬은 전쟁 전략 중 하나인 분할 정복(Divide and Conquer)에 바탕을 둔 알고리즘이다. 분할 정복 전략은 적군 전체를 공략하는 대신, 적군 전체를 잘게 나누어 공략하는 전법이다. 퀵 정렬의 동작 과정을 한마디로 요약하면 기준 요소 선정 및 분할의 반복이다. 구체적으로는 다음과 같은 과정으로 정렬이 진행된다.

- 1\. 기준 요소 선정 및 정렬대상 분류
  - 자료구조에서 기준이 될 요소를 임의로 선정하고 기준 요소(Pivot)의 값보다 작은 값을 가진 요소는 기준 요소의 왼쪽으로, 큰 값을 가진 요소는 오른쪽으로 옮긴다.
- 2\. 정렬대상 분할
  - 기준 요소 왼쪽에는 기준 요소보다 작은 요소의 그룹, 오른쪽에는 큰 요소의 그룹이 생긴다. 여기에서 왼쪽 그룹과 오른쪽 그룹을 분할하여 각 그룹에 대해 1의 과정을 수행한다.
- 3\. 반복
  - 그룹의 크기가 1 이하여서 더 이상 분할할 수 없을 때까지 1과 2의 과정을 반복한다.

백문이불여일견이라 했다. 코드를 보며 과정을 알아보자.

```c
void QuickSort(int _arr[], int _len, int _startIdx, int _endIdx) {
    if (_arr == NULL) return;                // (1) 배열이 NULL이면 종료
    if (_startIdx >= _endIdx) return;        // (2) 구간의 원소가 1개 이하이면 정렬 불필요

    int pivotIdx = Partition(_arr, _len, _startIdx, _endIdx);
    if (pivotIdx == -1) return;              // (3) Partition 실패 시(에러), 함수 종료

    // (4) 왼쪽 파티션 정렬: 피봇의 왼쪽 구간(_startIdx부터 pivotIdx-1)
    QuickSort(_arr, _len, _startIdx, pivotIdx - 1);

    // (5) 오른쪽 파티션 정렬: 피봇의 오른쪽 구간(pivotIdx+1부터 _endIdx)
    QuickSort(_arr, _len, pivotIdx + 1, _endIdx);
}
```

QuickSort 함수부터 알아보자.

일단 배열이 NULL인지 검사한다. 그리고 \_startIdx가 \_endIdx이면, 이미 구간 내에 원소가 1개 이하이므로 재귀 호출 없이 그대로 종료한다. 그 다음 Partition 함수를 이용해 기준이 될 피봇 인덱스를 구한다. Partition이 -1을 리턴하면 에러로 가정하고 종료한다.

그 다음 구해진 pivotIdx의 왼쪽 구간을 재귀 함수를 이용해 다시 QuickSort한다. 그리고 pivotIdx의 오른쪽 구간 또한 재귀 함수를 이용해 다시 QuickSort한다. 이 과정을 반복 호출하면 배열 전체가 정렬되게 된다.

그럼 안에 들어있는 Partition 함수는 무엇일까.

```c
int Partition(int _arr[], int _len, int _startIdx, int _endIdx) {
    if (_arr == NULL) return -1;     // (1) 배열이 NULL이면 에러 표시

    int pivotIdx = _startIdx;        // (2) 피봇을 구간의 첫 원소로 설정
    int leftIdx = pivotIdx + 1;      // (3) 피봇 바로 오른쪽에서 시작
    int rightIdx = _endIdx;          // (4) 구간의 맨 끝에서 시작
    int finalPivotIdx = pivotIdx;    //     최종 피봇 인덱스를 저장할 변수

    // (5) 피봇을 기준으로 왼쪽/오른쪽에서 서로 교차할 때까지 반복
    while (leftIdx < rightIdx) {
        // (5-1) 피봇보다 큰 원소를 찾기 위해 leftIdx를 오른쪽으로 이동
        while (leftIdx < _endIdx && _arr[leftIdx] <= _arr[pivotIdx]) {
            ++leftIdx;
        }

        // (5-2) 피봇보다 작은 원소를 찾기 위해 rightIdx를 왼쪽으로 이동
        while (rightIdx > _startIdx + 1 && _arr[rightIdx] >= _arr[pivotIdx]) {
            --rightIdx;
        }

        // (5-3) 만약 leftIdx >= rightIdx가 되었다면 교환할 구간이 교차했음을 의미
        if (leftIdx >= rightIdx) break;

        // (5-4) 아직 교차하지 않았다면, leftIdx에는 피봇보다 큰 값, rightIdx에는 피봇보다 작은 값이 위치하므로 두 값을 교환
        int tmp = _arr[leftIdx];
        _arr[leftIdx] = _arr[rightIdx];
        _arr[rightIdx] = tmp;
    }

    // (6) 반복문을 빠져나온 뒤, 피봇보다 작은 원소들을 찾아서 피봇과 교환해야 할 수도 있음
    //     rightIdx가 피봇보다 작은 원소가 위치해야 최종 피봇 위치가 된다.
    if (_arr[pivotIdx] > _arr[rightIdx]) {
        // (6-1) 피봇이 rightIdx 위치의 값보다 크다면, 피봇과 rightIdx 위치 값을 교환
        int tmp = _arr[pivotIdx];
        _arr[pivotIdx] = _arr[rightIdx];
        _arr[rightIdx] = tmp;
        finalPivotIdx = rightIdx;  // 최종 피봇 인덱스를 rightIdx로 갱신

        // (6-2) 디버그 용도로 현재 배열 상태 출력
        for (int i = _startIdx; i < _endIdx; ++i) {
            if (i == finalPivotIdx) printf("<%d> - ", _arr[i]);
            else printf("%d - ", _arr[i]);
        }
        printf("(%d)\n", _endIdx - _startIdx);

        return finalPivotIdx;
    }

    // (7) 피봇이 rightIdx 위치의 값보다 작다면 교환 없이 그대로
    //     finalPivotIdx를 pivotIdx(초기값)로 유지
    for (int i = _startIdx; i < _endIdx; ++i) {
        if (i == finalPivotIdx) printf("<%d> - ", _arr[i]);
        else printf("%d - ", _arr[i]);
    }
    printf("(%d)\n", _endIdx - _startIdx);

    return finalPivotIdx;
}
```

우선 배열이 NULL이면 -1을 반환해 에러가 발생했음을 알린다. 이번 글에선 구간의 시작 원소(\_startIdx 위치)에 있는 값을 피봇으로 삼는다. 그 후 값을 비교해줄 leftIdx와 rightIdx를 선언한다.

좌우 인덱스를 각각 피봇보다 큰 값, 피봇보다 작은 값을 만날 때까지 오른쪽 왼쪽으로 이동한다. 만약 leftIdx >= rightIdx가 되어버리면 교차했다는 뜻이고, 더 이상 교환할 요소가 없다는 뜻이므로 반복을 종료한다. 교차하지 않았다면, 피봇 기준으로 위치가 어긋나 있는 값 두개를 서로 맞교환한다.

피봇이 rightIdx보다 큰 값을 가진다면 둘을 교환해 피봇보다 작은 값이 왼쪽에, 피봇보다 큰 값이 오른쪽에 오도록 만든다. 최종적으로 마지막 피봇의 위치를 반환해 다음 재귀의 QuickSort 함수의 매개변수로 사용한다.

실행 결과를 보며 마무리 하겠다.

```text
실행 결과

35 - 9 - 19 - 16 - 31 - 13 - 16 - 35 - 25 - 5 - (10)

5 - 9 - 19 - 16 - 31 - 13 - 16 - 35 - 25 - (9)
<5> - 9 - 19 - 16 - 31 - 13 - 16 - 35 - (8)
<9> - 19 - 16 - 31 - 13 - 16 - 35 - (7)
13 - 16 - 16 - <19> - 31 - 35 - (6)
<13> - 16 - (2)
<16> - (1)
25 - <31> - (2)

5 - 9 - 13 - 16 - 16 - 19 - 25 - 31 - 35 - 35 - (10)
```

정리하자면 QuickSort는 분할 정복 방법론으로 구현되고, 각 재귀 단계에서 Partition 함수를 통해 피봇을 기준으로 "피봇보다 작은 집합"과 "피봇보다 큰 집합"을 분리한 뒤, 각 부분 배열에 대해 다시 QuickSort를 수행한다. Partition 핵심은 왼쪽 인덱스, 오른쪽 인덱스를 움직여서 피봇보다 큰 값과 피봇보다 작은 값을 교환해주고, 최종적으로 올바른 위치에 오도록 조정하는 것이다.

결과적으로, 이러한 분할 -> 정복 -> 합치는 과정 없이도 분할과 정복만 사용해 최종적으로 배열이 정렬되게 된다. 퀵 소트는 정말 자주 쓰이는 정렬 알고리즘이니 원리를 이해하고 자주 사용해보는 습관을 들이면 좋겠다.
