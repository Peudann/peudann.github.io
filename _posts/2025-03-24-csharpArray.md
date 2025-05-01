---
title: "25.03.24 / C# 배열"
excerpt: ""

categories:
  - Programming
  - C#
tags:
  - []

toc: true
toc_sticky: true

date: 2025-03-24
last_modified_at: 2025-04-29
---

C#에서 배열을 선언하는 법은 C와는 약간 다르다.

```csharp
class Program
{
  static void Main()
  {
    int[] staticArr = { 1, 2, 3, 4, 5 };
    staticArr[0] = 10;
    Console.WriteLine("staticArr Length : " + staticArr.Length);

    int[] dynamicArr = new int[3] { 1, 2, 3 };
    dynamicArr[1] = 20;

    foreach (int item in dynamicArr)
    {
      Console.Write("[{0}] - ", item);
    }
    Console.WriteLine("({0})", dynamicArr.Length);
    Console.WriteLine();
  }
}
```

이런 식으로 배열을 선언할 수 있다.

그럼 다차원 배열은 어떻게 선언할까?

```csharp
int[,] staticArr2d =
{
    { 11, 12, 13 },
    { 21, 22, 23 }
};
int[,,] staticArr3d;

int[,] dynamicArr2d = new int[2, 3];
// dynamicArr2d[1][2] = 100;
dynamicArr2d[1, 2] = 100;
Console.WriteLine("dynamicArr2d Length : {0}", dynamicArr2d.Length);
Console.WriteLine();
```

이런 식으로 대괄호 사이에 , 를 넣어 차원을 구분한다. 3차원이면 ,,를 넣어 선언할 수 있고, 동적 할당을 할 때에도 마찬가지로 new int\[2, 3\] 이런 식으로 선언할 수 있다.

그리고 가변 배열이라는 게 추가되었다.

```csharp
int[][] jaggedArr = new int[2][]
{
    new int[3] { 11, 12, 13 },
    new int[2] { 21, 22 }
};

foreach (int[] arr in jaggedArr)
{
    foreach(int item in arr)
    {
        Console.Write("[{0}] - ", item);
    }
    Console.WriteLine("({0})", arr.Length);
}
Console.WriteLine("jaggedArr Length : {0}", jaggedArr.Length);
Console.WriteLine();
```

\[11\] - \[12\] - \[13\] - (3)

\[21\] - \[22\] - (2)

jaggedArr Length : 2

이런 식으로 각 행의 내부 열 수를 다르게 선언하는 게 가능하다.

만약 배열을 복사하면 어떻게 복사될까?

```csharp
int[] copyArr = staticArr;
copyArr[0] = 777;
Console.WriteLine("staticArr[0] : {0}", staticArr[0]);
Console.WriteLine();
```

staticArr\[0\] : 777

복사한 배열의 값을 변경하면 이렇게 원본 값이 변한다. 이는 배열의 복사가 참조 타입이라 주소가 복사되어 복사본의 변경 사항이 원본에까지 영향을 끼치기 때문이다.

가변 배열도 복사할 수 있을까?

```csharp
int[][] copyJaggedArr = jaggedArr;
foreach (int[] arr in copyJaggedArr)
{
    foreach (int item in arr)
    {
        Console.Write("[{0}] - ", item);
    }
    Console.WriteLine("({0})", arr.Length);
}
Console.WriteLine("copyJaggedArr Length : {0}", copyJaggedArr.Length);
Console.WriteLine();
```

\[11\] - \[12\] - \[13\] - (3)

\[21\] - \[22\] - (2)

copyJaggedArr Length : 2

이렇게 원본 형태대로 잘 복사되는 걸 볼 수 있다.
