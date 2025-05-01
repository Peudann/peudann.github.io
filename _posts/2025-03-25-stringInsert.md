---
title: "25.03.25 / String을 Char 배열로 변환해 중간에 문자열 삽입하기"
excerpt: ""

categories:
  - Programming
  - C#
tags:
  - []

toc: true
toc_sticky: true

date: 2025-03-25
last_modified_at: 2025-04-29
---

StringInsert 클래스

```csharp
class StringInsert
{
    public static string StrInsert(string _str, int _idx, string _insert)
    {
        char[] tmpCharArr = _str.ToCharArray();
        char[] tmpInsert = _insert.ToCharArray();
        char[] resultCharArr = new char[_str.Length + _insert.Length];


        for (int i = 0; i < _idx; ++i)
        {
            resultCharArr[i] = tmpCharArr[i];
        }
        for (int i = _idx, j = 0; i < _idx + _insert.Length; ++i, ++j)
        {
            resultCharArr[i] = tmpInsert[j];
        }
        for (int i = _idx + _insert.Length; i < _str.Length + _insert.Length; ++i)
        {
            resultCharArr[i] = tmpCharArr[i - _insert.Length];
        }

        string resultString = new string(resultCharArr);

        return resultString;
    }
}
```

실제 실행 Program

```csharp
class Program
{
    static void main()
    {
        string strTest2 = new string("Hello World");
        string strInsert = "123";
        Console.WriteLine("strtest2 Insert : {0}", StringInsert.StrInsert(strTest2, 5, strInsert));
    }
}
```

실행 결과

```text
strtest2 Insert : Hello123 World
```

원래 문자열의 길이와 더해질 문자열의 길이의 합을 길이로 가지는 배열 resultCharArr을 선언해, 앞에서부터 \_idx까지, \_idx에서 \_idx + \_insert.Length까지, \_idx + \_insert.Length부터 \_str.Length + \_insert.Length까지 3부분으로 나눠서 새로 만들 배열에 데이터를 복사했다.
