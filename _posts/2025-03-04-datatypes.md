---
title: "25.03.04 / 자료형(DataTypes)"
excerpt: ""

categories:
  - Programming
tags:
  - [자료형, 데이터타입]

toc: true
toc_sticky: true

date: 2025-03-04
last_modified_at: 2025-04-23
---

<table style="color: #666666; text-align: center; border-collapse: collapse; width: 100.465%; height: 309px;" border="1" data-ke-style="style1" data-ke-align="alignLeft">
<tbody>
<tr style="height: 20px;">
<td style="text-align: center; width: 7.98041%; height: 20px;">구분</td>
<td style="text-align: center; width: 17.7922%; height: 20px;">자료형</td>
<td style="text-align: center; width: 12.6783%; height: 20px;">크기</td>
<td style="text-align: center; width: 25.9702%; height: 20px;">데이터 범위</td>
<td style="text-align: center; width: 11.1548%; height: 20px;">출력</td>
<td style="text-align: center; width: 15.7601%; height: 20px;">의미</td>
<td style="text-align: center; width: 9.97307%; height: 20px;">리터럴</td>
</tr>
<tr style="height: 20px;">
<td style="text-align: center; width: 7.98041%; height: 74px;" rowspan="4">문자형</td>
<td style="text-align: center; width: 17.7922%; height: 40px;" rowspan="2">char</td>
<td style="text-align: center; width: 12.6783%; height: 40px;" rowspan="2">1바이트</td>
<td style="width: 25.9702%; height: 40px;" rowspan="2">-128 ~ 127</td>
<td style="width: 11.1548%; height: 20px;">%c</td>
<td style="text-align: center; width: 15.7601%; height: 20px;">1개 문자 출력</td>
<td style="text-align: center; width: 9.97307%; height: 40px;" rowspan="2">없음</td>
</tr>
<tr style="height: 20px;">
<td style="width: 11.1548%; height: 20px;">%d</td>
<td style="text-align: center; width: 15.7601%; height: 20px;">10진법 정수 출력</td>
</tr>
<tr style="height: 17px;">
<td style="text-align: center; width: 17.7922%; height: 34px;" rowspan="2">unsigned char</td>
<td style="text-align: center; width: 12.6783%; height: 34px;" rowspan="2">1바이트</td>
<td style="width: 25.9702%; height: 34px;" rowspan="2">0 ~ 255</td>
<td style="width: 11.1548%; height: 17px;">%c</td>
<td style="text-align: center; width: 15.7601%; height: 17px;">1개 문자 출력</td>
<td style="text-align: center; width: 9.97307%; height: 34px;" rowspan="2">없음</td>
</tr>
<tr style="height: 17px;">
<td style="width: 11.1548%; height: 17px;">%u</td>
<td style="text-align: center; width: 15.7601%; height: 17px;">10진법 양수 출력</td>
</tr>
<tr style="height: 20px;">
<td style="text-align: center; width: 7.98041%; height: 155px;" rowspan="8">정수형</td>
<td style="text-align: center; width: 17.7922%; height: 20px;">short</td>
<td style="text-align: center; width: 12.6783%; height: 20px;">2바이트</td>
<td style="width: 25.9702%; height: 20px;">-32,768 ~ 32,767</td>
<td style="width: 11.1548%; height: 20px;">%d, %hd<br />%i, %hi</td>
<td style="text-align: center; width: 15.7601%; height: 77px;" rowspan="4">10진법 정수 출력</td>
<td style="text-align: center; width: 9.97307%; height: 40px;" rowspan="2">없음</td>
</tr>
<tr style="height: 20px;">
<td style="text-align: center; width: 17.7922%; height: 20px;">int</td>
<td style="text-align: center; width: 12.6783%; height: 20px;">4바이트</td>
<td style="width: 25.9702%; height: 20px;">-2,147,483,648 ~ <br />2,147,483,647</td>
<td style="width: 11.1548%; height: 20px;">%d, %i</td>
</tr>
<tr style="height: 20px;">
<td style="text-align: center; width: 17.7922%; height: 20px;">long</td>
<td style="text-align: center; width: 12.6783%; height: 20px;">4바이트</td>
<td style="width: 25.9702%; height: 20px;">-2,147,483,648 ~ <br />2,147,483,647</td>
<td style="width: 11.1548%; height: 20px;">%ld, %li</td>
<td style="text-align: center; width: 9.97307%; height: 20px;">L</td>
</tr>
<tr style="height: 17px;">
<td style="text-align: center; width: 17.7922%; height: 17px;">long long</td>
<td style="text-align: center; width: 12.6783%; height: 17px;">8바이트</td>
<td style="width: 25.9702%; height: 17px;">-2^63 ~ 2^63 - 1</td>
<td style="width: 11.1548%; height: 17px;">%lld, %lli</td>
<td style="text-align: center; width: 9.97307%; height: 17px;">LL</td>
</tr>
<tr style="height: 20px;">
<td style="text-align: center; width: 17.7922%; height: 20px;">unsigned short</td>
<td style="text-align: center; width: 12.6783%; height: 20px;">2바이트</td>
<td style="width: 25.9702%; height: 20px;">0 ~ 65,535</td>
<td style="width: 11.1548%; height: 20px;">%u, %hu</td>
<td style="text-align: center; width: 15.7601%; height: 78px;" rowspan="4">10진법 양수 출력</td>
<td style="text-align: center; width: 9.97307%; height: 20px;">없음</td>
</tr>
<tr style="height: 20px;">
<td style="text-align: center; width: 17.7922%; height: 20px;">unsigned int</td>
<td style="text-align: center; width: 12.6783%; height: 20px;">4바이트</td>
<td style="width: 25.9702%; height: 20px;"><span style="color: #666666; text-align: center;">0 ~ 4,294,967,295</span></td>
<td style="width: 11.1548%; height: 20px;">%u</td>
<td style="text-align: center; width: 9.97307%; height: 20px;">U</td>
</tr>
<tr style="height: 18px;">
<td style="text-align: center; width: 17.7922%; height: 18px;">unsigned long</td>
<td style="text-align: center; width: 12.6783%; height: 18px;">4바이트</td>
<td style="width: 25.9702%; height: 18px;">0 ~ <span style="color: #666666; text-align: center;">4,294,967,295</span></td>
<td style="width: 11.1548%; height: 18px;">%lu</td>
<td style="text-align: center; width: 9.97307%; height: 18px;">UL</td>
</tr>
<tr style="height: 20px;">
<td style="text-align: center; width: 17.7922%; height: 20px;">unsigned long long</td>
<td style="text-align: center; width: 12.6783%; height: 20px;">8바이트</td>
<td style="width: 25.9702%; height: 20px;"><span style="color: #666666; text-align: center;">0 ~ 2^64 - 1</span></td>
<td style="width: 11.1548%; height: 20px;">%llu</td>
<td style="text-align: center; width: 9.97307%; height: 20px;">ULL</td>
</tr>
<tr style="height: 20px;">
<td style="text-align: center; width: 7.98041%; height: 60px;" rowspan="3">실수형</td>
<td style="text-align: center; width: 17.7922%; height: 20px;">float</td>
<td style="text-align: center; width: 12.6783%; height: 20px;">4바이트</td>
<td style="width: 25.9702%; height: 20px;">-3.4 x 10^38 ~ <br />3.4 x 10^38</td>
<td style="width: 11.1548%; height: 20px;">%f</td>
<td style="text-align: center; width: 15.7601%; height: 20px;">유효 자릿수 7</td>
<td style="text-align: center; width: 9.97307%; height: 20px;">f</td>
</tr>
<tr style="height: 20px;">
<td style="text-align: center; width: 17.7922%; height: 20px;">double</td>
<td style="text-align: center; width: 12.6783%; height: 20px;">8바이트</td>
<td style="width: 25.9702%; height: 20px;">-1.79 x 10^308 ~ <br />1.79 x 10^308</td>
<td style="width: 11.1548%; height: 20px;">%lf</td>
<td style="text-align: center; width: 15.7601%; height: 20px;">유효 자릿수 15</td>
<td style="text-align: center; width: 9.97307%; height: 20px;">없음</td>
</tr>
<tr style="height: 20px;">
<td style="text-align: center; width: 17.7922%; height: 20px;">long double</td>
<td style="text-align: center; width: 12.6783%; height: 20px;">8바이트 이상</td>
<td style="width: 25.9702%; height: 20px;">double과 동일하거나<br />그 이상</td>
<td style="width: 11.1548%; height: 20px;">%Lf</td>
<td style="text-align: center; width: 15.7601%; height: 20px;">유효 자릿수<br />15 이상</td>
<td style="text-align: center; width: 9.97307%; height: 20px;">L</td>
</tr>
</tbody>
</table>
---

각 자료형의 특징을 간단하게 알아보자.

char (Character, 문자)의 경우 1바이트로 255개의 숫자를 사용 가능하다. 영어의 경우 컴퓨터가 문자를 인식하지 못해 각 문자별로 대응되는 아스키 코드 (ASCII Code)가 존재한다. 예를 들어 A는 65, a는 97로 변환된다. 그래서 %i, %d를 사용해 출력하면 해당하는 아스키 코드 번호가 나온다. 그럼 해당하는 아스키 코드를 적으면 모든 자료형이 문자를 저장할 수 있는가? 하면 안된다고 볼 수는 없겠지만 아스키 코드는 0부터 255까지 밖에 존재하지 않고 1바이트만 사용해 char로 나타낼 수 있는데 만약 2바이트 이상인 short, int 등을 사용하면 메모리 낭비가 생기게 된다. 그런데 -128부터 127까지인 signed char로는 아스키 코드를 다 표현할 수 없다. 이럴 땐 양수만 사용해 더 많은 수를 표기하는 unsigned char를 사용한다.

```c
int test = 65;
printf("%c", test);

// 결괏값 : A
```

short의 경우 2바이트를 차지하기 때문에 대략 6만 5천개 정도의 숫자를 사용 가능하다. int의 절반이기 때문에 half int에서 따온 %hi를 사용해 출력할 수도 있다.

int (Integer, 정수)는 4바이트를 차지하는 약 43억개 정도의 숫자를 사용 가능한 가장 기본이 되는 정수 자료형이다. int는 CPU가 연산할 수 있는 가장 기준이 되는 자료형이고 계산 속도가 가장 빠르다. 특이하게 int는 옛날엔 크기가 달랐는데, int는 컴퓨터가 몇 비트인지에 따라 달라졌다. 4바이트 = 32비트인데, 이는 32비트 컴퓨터가 되며 int가 4바이트로 정해진 것이다. 물론 지금은 64비트가 주류이지만 64비트는 32비트 프로그램을 호환하고, int가 8비트가 되어버리면 long이 int보다 작은 이상한 상황이 일어나기 때문에 int가 64비트에서도 4바이트로 남아있다. 컴퓨터가 16비트였던 시절엔 int도 각각 2바이트였다고 한다.

long (long int)도 4바이트다. int와 똑같은 4바이트인데 왜 long으로 나누어져 있냐면 위에 언급했듯 과거 16비트 시절엔 int가 2바이트로 더 적은 용량을 차지했기 때문에 그보다 큰 long이 4바이트였지만 int가 4바이트로 커져버려 같아졌다고 한다. long int에서 따온 %li, %ld 등을 사용할 수 있고 L이라는 리터럴이 붙는다.

long long (long long int)은 8바이트를 차지하고 대략 1800경개 정도의 숫자를 사용할 수 있다. long 처럼 %lli, %lld 로 출력할 수 있고 LL이라는 리터럴이 붙는다.

리터럴을 표기하는 이유는 위 코드처럼 변수를 선언할 경우 리터럴 상수 (위 코드의 65)는 test에 대입되기 전 4바이트인 int 형태로 메모리에 임시 저장된다. 그런데 만약 int의 허용 범위를 넘어서는 리터럴 상수를 저장하려 하면 변수에 대입되기 전에 데이터가 손실이 나버려 손실된 데이터가 변수에 저장되게 된다. 이를 막기 위해 L이나 LL을 붙여 이 리터럴 상수는 int보다 더 크다는 것을 컴퓨터에게 알리는 것이다.

unsigned 정수 자료형은 크기는 똑같은 대신 0을 포함한 양수만 사용 가능한 자료형이다. 더 큰 수를 사용하기 위해 unsigned를 사용하게 되었고, 부호를 검사하지 않기 때문에 속도가 더 빠르다.

float는 Floating-Point (부동소수점)에서 따온 자료형이다. 이 때 부동의 부는 아닐 부가 아니라 뜰 부를 사용한다. 4바이트를 사용하고, 출력법으론 %f를 사용한다. 과거엔 4바이트에서 앞 2바이트를 정수부, 뒤 2바이트를 실수부로 사용하는 고정소수점 (Fixed Point)를 사용했지만 현재는 부동소수점을 주로 사용한다. float는 가장 작은 실수 자료형인데도 리터럴 f를 사용하는데, 실수의 기본 리터럴 상수가 double로 만들어지기 때문이다. 이에 따라 float로 변수를 만들면 리터럴 상수의 4바이트가 낭비되기 때문에 f를 붙여 float의 리터럴 상수는 4바이트라는 걸 표기한다. 왜 가장 작은 단위인 float가 리터럴 상수의 기준이 아닌지는 모르겠다.

double은 Double Precision Floating-Point (배 정밀도 부동소수점)에서 따온 자료형이다. 8바이트를 사용하고, long float에서 따온 %lf를 출력법으로 사용한다. 실수 자료형의 기본 리터럴 상수가 double형으로 만들어진다. 그렇기 때문에 double 형 변수는 리터럴을 붙이지 않는다.

왜 리터럴은 double 기준이고 출력법은 float 기준인지는 의문스럽다. float가 half double이어야 하는거 아닌가? double의 %d가 겹쳐서 float의 %f를 쓴걸까?

long double은 8바이트 이상을 사용하는 실수 자료형이다. 또 이상하게 출력법으로 %llf (long long float)가 아닌 %Lf 를 사용한다. 아주 지 맘대로다. long double은 컴파일러, 언어 버전, 운영 체제에 따라 8바이트 이상을 사용하는 경우가 있어 8바이트 이상으로 표기한다.
