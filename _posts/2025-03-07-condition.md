---
title: "25.03.07 / 조건문"
excerpt: ""

categories:
  - Programming
tags:
  - []

toc: true
toc_sticky: true

date: 2025-03-07
last_modified_at: 2025-04-23
---

조건문은 조건이 참인 경우 내부 코드를 실행하는 코드를 말한다. 여러 가지의 조건문을 알아보자.

```c
#include <stdio.h>

void main() {
    int value = 10;

    if (value < 15) {
    	printf("%d < 15", value);
    }
}

// if문 형태 : if (조건) {실행 코드};

// 실행값
// 10 < 15
```

if문을 사용한 코드이다. value를 10으로 초기화했고 if문의 조건인 value < 15가 만족되어 참(1)이 되기 때문에 내부 코드인 printf가 실행된다.

```c
#include <stdio.h>

void main() {
    int value = 10

    if (value > 10) {
        printf("%d > 10", value);
    }
    else {
    	printf("%d <= 10", value);
    }
}

// 실행값
// 10 <= 10
```

if ~ else문을 사용한 코드이다. if문의 조건이 참인 경우 if문이 실행되고 그렇지 않을 경우 else문이 실행된다. 이번엔 조건이 거짓이기 때문에 else문이 실행되어 출력값은 10 <= 10.

```c
#include <stdio.h>

void main() {
    int value = 10;

    if (value > 10) {
	    printf("%d > 10\n", value);
    }
    else if (value < 10) {
	    printf("%d < 10\n", value);
    }
    else {
	    printf("%d = 10\n", value);
    }
}

// 실행값
// 10 = 10
```

if ~ else if문을 사용한 코드이다. if문의 조건을 확인하고 조건이 거짓인 경우 else if의 조건을 확인한다. else if는 여러개 작성할 수 있고, 만약 모두 거짓이라면 else문이 실행된다.

if문을 활용한 코드 예시이다.

```c
#include <stdio.h>

void main() {
    int hp = 4;
    for (int i = 0; i < 10; i++) {
	    if (hp <= 0) {
		    printf("YOU DIED\n");
		    hp = 0; // 예외처리 (Exception)
		    break;
	    }
	    else {
		    if (i < hp) printf("■");
		    else printf("□");
	    }
    }
    if (hp > 0) printf("(%d)\n", hp);
}

// 실행값
// ■■■■□□□□□□(4)
```

다음은 switch문이다.

```c
#include <stdio.h>

void main() {
    int var = 5;

    switch (var) {
    case 10:
        printf("var is 10\n");
        break;
    case 5:
        printf("var is 5\n");
        break;
    case 3:
        printf("var is 3\n");
        break;
    default :
        printf("var is not 10, 5, 3\n");
        break;
    }
}

// 실행값
// var is 5
```

switch의 조건인 var가 몇인지 보고 해당하는 case에 가서 코드를 실행하는 방식이다.

switch문의 경우엔 몇 가지 특징이 있는데,

```c
int atk = 3;
int dmg = 0;

switch (atk) {
case 5:
    dmg += 5;
case 4:
    dmg += 4;
case 3:
    dmg += 3;
case 2:
    dmg += 2;
case 1:
    dmg += 1;
}

printf("Total Damage : %d", dmg);


// 실행값
// dmg = 6
```

바로 해당하는 case의 아래 코드를 전부 실행한다는 점이다. 위 코드는 case3을 만족하는데 case3, 2, 1이 모두 실행되어 dmg에 3 + 2 + 1인 6이 저장된다. 이를 방지하기 위해 각 case 마지막에 break를 붙인다.

```c
switch (menu) {
case 3:
    printf("Play");
    break;
case 2:
    printf("Option");
    break;
case 1:
    printf("Quit);
    break;
}
```

위와 같은 코드를 작성했다고 가정하자. menu의 값에 따라 Play, Option, Quit를 출력하는 코드이다. 그런데 여기서 case가 3, 2, 1과 같은 숫자로 작성되어 있기 때문에 가독성이 좋지 않다. 그럼 변수를 사용하면 안되나?

```c
int play = 3;
int option = 2;
int quit = 1;

switch (menu) {
case play:
    printf("Play");
    break;
case option:
    printf("Option");
    break;
case quit:
    printf("Quit);
    break;
}
```

이런 식으로 코드를 작성해봤다. 그러나 이 코드는 실행하지 못하는데, 이는 case가 상수인 값만 확인할 수 있기 때문이다.

```c
const int play = 3;

switch (menu) {
case play:
    printf("Play");
    break;
case 2:
    printf("Option");
    break;
case 1:
    printf("Quit);
    break;
}
```

그럼 이런식으로 변수를 상수화한다면 어떨까? 아쉽게도 이번에도 실행에 실패한다. 이는 컴파일 과정 때문인데, const를 이용하여 상수화를 한 경우 컴파일 타임에선 play를 상수로 인식하지 못하고 런타임에서 상수화가 진행되기 때문이다. 그래서 이를 해결하기 위해 아래의 방법이 있다.

```c
#include <stdio.h>
#define MENU_OPTION 2
enum EMenu { kQuit = 1 };

void main() {
    int menu = 0;
    switch (menu) {
    case 3:
        printf("Play");
        break;
    case MENU_OPTION:
        printf("Option");
        break;
    case kQuit:
        printf("Quit);
        break;
}
```

이런 식으로 main 함수 내에서가 아닌 전처리기를 사용해 변수를 상수화하면 컴파일 타임에서 처리되기 때문에 실행이 가능해진다. 우리 눈엔 MENU_OPTION, kQuit 등 변수로 보이지만 컴퓨터 입장에선 2, 1로 인식된다 생각하면 편하다. 가독성이 중요한 switch문의 경우 이런 방식을 활용하도록 하자.

```c
int case1 = 1;
const int case2 = 2;

case 5.2: break;
case case1: break;
case case2: break;
```

위 코드는 실행되지 않는 case의 모음이다. case의 값은 실수가 될 수 없고, 위에서 설명했듯 변수로는 처리할 수 없다.
