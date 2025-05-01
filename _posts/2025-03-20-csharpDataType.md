---
title: "25.03.20 / C# 데이터 타입"
excerpt: ""

categories:
  - Programming
  - C#
tags:
  - []

toc: true
toc_sticky: true

date: 2025-03-20
last_modified_at: 2025-04-29
---

C#에서 쓰이는 데이터 타입은 C와 대부분 비슷하지만 약간의 차이가 있다.

```csharp
class DataTypes
{
    bool b = true; // true / false

    char c = 'a';

    byte by = 1;
    sbyte sby = -1;

    int i = -10;
    uint ui = 10;
    nint ni = 0;

    long l = -100;
    ulong ul = 100;

    float f = 3.14f;
    double d = 3.14;
    decimal dec = 3.14m;

    public void BoolExample()
    {
        Console.WriteLine("bool : " + b + " (" + sizeof(bool) + " bytes)");
    }

    ...
}
```

출력 함수는 코드가 너무 길어져서 하나만 잘라왔다. 나머지도 출력하는 방식은 다 똑같다.

자료형을 담은 DataTypes 클래스를 만들었으니 이번엔 Program에서 불러와보자.

```csharp
class Program
{
    static void Main()
    {
        DataTypes dataTypes = new DataTypes();

        dataTypes.BoolExample();
        Console.WriteLine();

        dataTypes.CharExample();
        dataTypes.ByteExample();
        dataTypes.SByteExample();
        Console.WriteLine();

        dataTypes.IntExample();
        dataTypes.UIntExample();
        dataTypes.LongExample();
        dataTypes.ULongExample();
        Console.WriteLine();

        dataTypes.FloatExample();
        dataTypes.DoubleExample();
        dataTypes.DecimalExample();
    }
}
```

```text
bool : True (1 bytes)

char : a (2 bytes)
byte : 1 (1 bytes)
sbyte : -1 (1 bytes)

int : -10 (4 bytes)
uint : 10 (4 bytes)
long : -100 (8 bytes)
ulong : 100 (8 bytes)

float : 3.14 (4 bytes)
double : 3.14 (8 bytes)
decimal : 3.14 (16 bytes)
```

잘 보면 저번 C랑 다른게 보이는데, 우선 char가 2바이트가 되었고, 본 적 없는 byte, sbyte, uint, ulong, decimal이 눈에 보인다. 표를 만들어 알아보도록 하자.

먼저 정수형 데이터 타입이다.

| C# 형식 / 키워드 | 범위                                                    | 사이즈                            | .NET 형식      |
| ---------------- | ------------------------------------------------------- | --------------------------------- | -------------- |
| sbyte            | \-128 ~ 127                                             | 부호 있는 8비트 정수              | System.SByte   |
| byte             | 0 ~ 255                                                 | 부호 없는 8비트 정수              | System.Byte    |
| short            | \-32,768 ~ 32,767                                       | 부호 있는 16비트 정수             | Sytstem.Int16  |
| ushort           | 0 ~ 65,535                                              | 부호 없는 16비트 정수             | System.UInt16  |
| int              | \-2,147,483,648 ~ 2,147,483,647                         | 부호 있는 32비트 정수             | System.Int32   |
| uint             | 0 ~ 4,294,967,295                                       | 부호 없는 32비트 정수             | System.UInt32  |
| long             | \-9,223,372,036,854,775,808 ~ 9,223,372,036,854,775,807 | 부호 있는 64비트 정수             | System.Int64   |
| ulong            | 0 ~ 18,446,744,073,709,551,615                          | 부호 없는 64비트 정수             | System.UInt64  |
| nint             | 플랫폼에 따라 다름 (런타임에 계산됨)                    | 부호 있는 32비트 또는 64비트 정수 | System.IntPtr  |
| nuint            | 플랫폼에 따라 다름 (런타임에 계산됨)                    | 부호 없는 32비트 또는 64비트 정수 | System.UIntPtr |

1바이트짜리 byte가 새로 생겼다. 그리고 unsigned, signed가 없어졌기 때문에 sbyte, ushort, uint 등이 생겼다. C에서 4바이트였던 long은 8바이트가 됐다. 마지막 nint와 nuint는 운영체제가 32비트인지 64비트인지에 따라 달라지는 포인터 자료형이다.

가장 왼쪽의 C# 형식 키워드는 해당하는 .NET 형식의 별칭이다. 두 개를 바꿔 사용할 수 있다.

이번엔 실수형 데이터 타입을 알아보자.

| C# 형식 / 키워드 | 근사 범위                        | 전체 자릿수      | 사이즈   | 리터럴      | .NET 형식      |
| ---------------- | -------------------------------- | ---------------- | -------- | ----------- | -------------- |
| float            | ±1.5 x 10^−45 ~ ±3.4 x 10^38     | ~6-9개 자릿수    | 4바이트  | 없거나 d, D | System.Single  |
| double           | ±5.0 × 10^−324 ~ ±1.7 × 10^308   | ~15-17개 자릿수  | 8바이트  | f, F        | System.Double  |
| decimal          | ±1.0 x 10^\-28 ~ ±7.9228 x 10^28 | 28-29개의 자릿수 | 16바이트 | m, M        | System.Decimal |

C언어의 long double이 사라지고 decimal이 생겼다. 역시 C# 형식 키워드를 .NET 형식으로 바꿔 쓸 수 있다.

bool이라는 참과 거짓을 나타내는 자료형도 있다. 용량은 1바이트를 차지한다.

```csharp
bool check = true;
Console.WriteLine(check ? "Checked" : "Not checked");  // output: Checked

Console.WriteLine(false ? "Checked" : "Not checked");  // output: Not checked
```

char가 조금 바뀌었는데, 기존 C언어에서는 아스키 코드를 가지는 1바이트짜리 자료형이었다. 그런데 이제 글로벌한 문자를 표현하기 위해 유니코드를 지원하기 시작해서 2바이트로 늘어났다.

| C# 형식 / 키워드 | 범위              | 사이즈           | .NET 형식   |
| ---------------- | ----------------- | ---------------- | ----------- |
| char             | U +0000 ~ U +FFFF | 16비트 (2바이트) | System.Char |

마이크로소프트 홈페이지에 가면 더 많은 정보를 얻을 수 있으니 한 번씩 들려보자.

[https://learn.microsoft.com/ko-kr/dotnet/csharp/language-reference/builtin-types/value-types](https://learn.microsoft.com/ko-kr/dotnet/csharp/language-reference/builtin-types/value-types)
