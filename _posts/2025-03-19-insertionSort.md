---
title: "25.03.19 / 삽입 정렬(Insertion Sort)"
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

삽입 정렬(Insertion Sort)은 책장에 꽂힌 책을 정리하는 과정과 비슷하다고 생각하면 된다. 예를 들어 스물 네 권의 슬램덩크 전집이 아무렇게나 꽂혀 있는 책장을 정리한다고 가정해보자. 일단 책장의 왼쪽부터 오른쪽 방향으로 각 권이 옳은 순서로 꽂혀 있는지 확인하고, 잘못된 위치에 꽂혀 있는 책은 뽑아서 올바른 위치에 삽입해나가면 된다.

삽입 정렬은 책을 정리하듯이 자료구조를 순차적으로 순회하면서 순서에 어긋나는 요소를 찾고, 그 요소를 올바른 위치에 다시 삽입해나가는 정렬 알고리즘이다. 삽입 정렬은 구현하는 방식이 여러 가지이기도 하고, 만들기 쉬운 구조라 자주 쓰인다.

아래 코드를 보며 삽입 정렬을 알아보자.

```c
void InsertionSort(int* _pArr, const int _len) {
    if (_pArr == NULL) return;

    for (int i = 1; i < _len; ++i) {
        // 현재 삽입하려는 원소(data)를 임시 저장
        int j = i;
        int data = _pArr[i];
        printf("Data : [%d] %d\n", i, data);

        // 왼쪽에 있는 값들 중 data보다 큰 값들을 한 칸씩 뒤로 옮긴다.
        while (j > 0) {
            // data보다 작거나 같은 값을 만나면 그 자리가 삽입 위치가 된다.
            if (data > _pArr[j - 1]) break;

            // 뒤로 옮기는 과정을 출력
            _pArr[j] = _pArr[j - 1];
            printf("[%d] >>\n", j - 1);
            --j;
        }

        // 찾은 위치 j에 data를 삽입
        _pArr[j] = data;
        printf("Insert : [%d] = %d\n", j, data);

        // 중간 결과 확인용
        PrintAll(_pArr, _len);
        printf("\n");
    }
}
```

현재 삽입하려는 원소를 data에 임시로 저장한다. 그리고 왼쪽에 있는 값들 중 data보다 큰 값들을 오른쪽으로 한 칸씩 옮기는 과정을 거친다. break문은 data가 왼쪽 원소(\_pArr\[j - 1\])보다 크면 그 자리가 적절한 삽입 위치이므로 더 이상 비교하지 않고 while문을 빠져나가기 위해 작성했다. data가 왼쪽 원소보다 작거나 같다면, 해당 원소를 한 칸 오른쪽으로 옮긴 뒤 j를 한 칸 왼쪽으로 줄인다. 이러한 과정을 거쳐 while문 내부에서 현재 삽입할 값(data)보다 큰 값들을 한 칸씩 뒤로 미루어 공간을 마련하게 된다.

while 반복문이 종료되면 data가 들어갈 정확한 위치를 알기 때문에 그 위치에 data를 삽입한다. 이제 i를 1부터 길이 - 1까지 증가시키면서 현재 위치의 값을 왼쪽 정렬 구간에 삽입하는 과정을 반복한다.

한 번 출력해보자.

```c
void main() {
	const int len = 5;
	int* pArr = (int*)malloc(sizeof(int) * len);

	srand((unsigned int)time(NULL));
	InitWithRandom(pArr, len);

	PrintAll(pArr, len);

	InsertionSort(pArr, len);
	PrintAll(pArr, len);

	if (pArr != NULL){
		free(pArr);
		pArr = NULL;
	}
}
```

```text
실행 결과

2 - 2 - 29 - 6 - 0 - (5)
Data : [1] 2
[0] >>
Insert : [0] = 2
2 - 2 - 29 - 6 - 0 - (5)

Data : [2] 29
Insert : [2] = 29
2 - 2 - 29 - 6 - 0 - (5)

Data : [3] 6
[2] >>
Insert : [2] = 6
2 - 2 - 6 - 29 - 0 - (5)

Data : [4] 0
[3] >>
[2] >>
[1] >>
[0] >>
Insert : [0] = 0
0 - 2 - 2 - 6 - 29 - (5)

0 - 2 - 2 - 6 - 29 - (5)
```

이런 식으로 삽입 정렬은 이미 정렬된 구간에 새로운 원소를 적절한 위치에 끼워 넣는 방식을 매 단계 수행하여 전체 배열을 정렬하는 방식이다.
