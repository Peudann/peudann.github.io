---
title: "25.03.14 / 구조체(Structure)"
excerpt: ""

categories:
  - Programming
  - Data Structure and Algorithm
tags:
  - []

toc: true
toc_sticky: true

date: 2025-03-14
last_modified_at: 2025-04-25
---

구조체(Structure)는 서로 관련된 여러 개의 변수를 하나의 사용자 정의 자료형으로 묶는 기능을 제공한다. 예를 들어, 사람을 나타내는 정보가 이름, 나이, 키, 체중과 같이 여러 개일 때, 각각의 정보를 개별 변수로 관리하는 대신 구조체를 사용하여 하나의 묶음으로 다룰 수 있다. 이를 통해 관련된 정보를 더 직관적으로 관리할 수 있고, 복잡한 프로그램에서도 변수를 체계적으로 조직할 수 있다.

```c
struct SPlayer {
    int lv;
    int hp;
    int mp;
    int exp
}

typedef struct _SMonster {
    int lv;
    int hp;
} SMonster;
```

int형 멤버 여러 개를 가지는 구조체 SPlayer와 SMonster를 선언했다. 두 방법은 나중에 구조체 변수를 선언할 때에 차이가 있다.

```c
void main() {
    struct SPlayer player;
    SMonster monster;
}
```

이렇게 위의 방식을 사용해 선언하면 사용할 때마다 struct를 붙여야 해 귀찮음이 생긴다. 그래서 typedef로 struct \_SMonster를 SMonster로 사용하기로 정한 것이다.

```c
void main() {
    struct _SPlayer player;
    player.lv = 5;
    player.hp = 10;
    player.mp = 3;
    player.exp = 3;

    printf("SPlayer Size: %d bytes\n", (int)sizeof(struct _SPlayer));
    printf("player.lv : %d (%p)\n", player.lv, &player.lv);
    printf("player.hp : %d (%p)\n", player.hp, &player.hp);
    printf("player.mp : %d (%p)\n", player.mp, &player.mp);
    printf("player.exp : %d (%p)\n", player.exp, &player.exp);
}
```

구조체 변수 player를 선언해 player의 멤버 lv, hp, mp, exp를 수정했다. 그리고 구조체의 총 용량과 각 멤버의 값과 주소를 출력했다. 결과 값을 보자.

```text
SPlayer Size : 16 bytes
player.lv : 5 (0x16f0df2c0)
player.hp : 10 (0x16f0df2c4)
player.mp : 3 (0x16f0df2c8)
player.exp : 100 (0x16f0df2cc)
```

int형인 멤버가 4개이기 때문에 4바이트 \* 4 = 16바이트가 나왔다. 그리고 각 멤버들도 값이 잘 들어간걸 확인할 수 있다. 멤버들의 주소를 보면 배열처럼 int형의 크기인 4바이트씩 차이가 나면서 이어진다는 걸 볼 수 있다. 이게 구조체의 특징 중 하나다.

그런데 만약 자료형을 섞어서 쓰면 어떻게 될까?

```c
typedef struct _SPlayer {
    char lv;
    int hp;
    char mp;
    int exp;
} SPlayer;
```

구조체 SPlayer를 이렇게 선언했다고 치고 위의 main 함수를 실행해보자.

```text
실행 결과

SPlayer Size : 16 bytes
player.lv : 5 (0x16eed72c0)
player.hp : 10 (0x16eed72c4)
player.mp : 3 (0x16eed72c8)
player.exp : 100 (0x16eed72cc)
```

여전히 SPlayer의 크기는 16바이트를 차지한다. char 2개, int 2개를 사용했으니 10 바이트가 되어야 하는게 아닐까?

또 다른 예시를 보자.

```c
typedef struct _SPlayer {
    char lv;
    char hp;
    int mp;
    int exp;
} SPlayer;
```

```text
실행 결과

SPlayer Size : 12 bytes
player.lv : 5 (0x16d15b2ac)
player.hp : 10 (0x16d15b2ad)
player.mp : 3 (0x16d15b2b0)
player.exp : 100 (0x16d15b2b4)
```

이번엔 갑자기 12바이트로 줄었다. 이게 어떻게 된 일일까?

이는 구조체의 특징 중 하나인 구조체 패딩(Padding) 때문이다. 구조체 패딩이란 컴파일러가 구조체 멤버들을 메모리에 배치할 때 멤버의 정렬 조건을 만족하기 위해 사용하지 않는 공간(패딩 바이트)을 중간중간 삽입하는 걸 말한다. 구조체 멤버가 올바른 주소 경계에서 시작하도록 해 CPU가 해당 멤버를 접근할 때 성능상의 이점을 얻거나, CPU가 요구하는 정렬 규칙을 준수하기 위함이다.

int, char, int, char이라 가정하면 컴퓨터 입장에서 4바이트 갔다가 1바이트 가고 다시 4바이트 갔다가 1바이트 가고 이러는 것보다 그냥 4바이트마다 멤버를 배치하는게 더 낫다는 판단을 내린거다. 그래서 4바이트 + 1바이트 + 패딩 3바이트 + 4바이트 + 1바이트 + 패딩 3바이트로 총 16바이트가 나오게 된 것이다. 아래의 경우 1바이트를 두개 사용하더라도 4바이트보다 작기 때문에 1바이트 + 1바이트 + 패딩 2바이트 + 4바이트 + 4바이트로 총 12바이트가 나오게 되었다.

```c
void main() {
    SMonster* mob = (SMonster*)malloc(sizeof(SMonster));

    mob->lv = 5;
    mob->hp = 10;

    if (mob != NULL) {
        free(mob);
        mob = NULL;
    }
}
```

이런 식으로 구조체 변수를 포인터로 선언할 수도 있다. 이 경우 멤버에 접근할 때 사용되는 접근 지정자가 . 에서 -> 로 변경된다.

이번엔 구조체 변수를 복사해보도록 하자.

```c
void main() {
    SPlayer copyPlayer = player;
    copyPlayer.hp = 9999;

    printf("copyPlayer.lv : %d\n", copyPlayer.lv);
    printf("copyPlayer.hp : %d\n", copyPlayer.hp);
    printf("copyPlayer.mp : %d\n", copyPlayer.mp);
    printf("copyPlayer.exp : %d\n", copyPlayer.exp);

    printf("player.hp : %d\n", player.hp);
}
```

이렇게 copyPlayer에 player를 대입해 복사를 진행했다. 여기서 만약 복사본의 hp값을 변경하면 원본 hp가 변할까?

```text
실행 결과

copyPlayer.lv : 5
copyPlayer.hp : 9999
copyPlayer.mp : 3
copyPlayer.exp : 100
player.hp : 10
```

정답은 원본은 변경되지 않는다. 구조체 간의 복사는 얕은 복사가 아닌 깊은 복사로 이루어지기 때문이다. 그래서 나머지 값은 복사된 값이고, hp는 서로 다른 값을 가지게 되었다.

구조체는 C언어에서 자료 구조의 기초를 이루는 중요한 기능이다. 이를 응용하면 연결 리스트, 트리, 그래프 등 다양한 자료 구조를 C에서 직접 구현할 수 있고, 복잡한 소프트웨어 설계에서도 구조체를 통해 데이터의 논리적 구분을 명확히 할 수 있다. 적재적소에 활용해 더 좋은 코드를 만들어보도록 하자.
