---
title: "25.03.25 / String을 Char 배열로 변환해 대소문자 바꾸기"
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

StringUpDown 클래스

```csharp
class StringUpDown
{
    public static string StringUpper(string _str)
    {
        char[] tmpChar = _str.ToCharArray();
        for (int i = 0; i < _str.Length; ++i)
        {
            if ((int)tmpChar[i] >= 97 && (int)tmpChar[i] <= 122)
            {
                int tmpInt = (int)tmpChar[i];
                tmpInt -= 32;
                tmpChar[i] = (char)tmpInt;
            }
        }
        string tmpString = new string(tmpChar);

        return tmpString;
    }

    public static string StringLower(string _str)
    {
        char[] tmpChar = _str.ToCharArray();
        for (int i = 0; i < _str.Length; ++i)
        {
            if ((int)tmpChar[i] >= 65 && (int)tmpChar[i] <= 90)
            {
                int tmpInt = (int)tmpChar[i];
                tmpInt += 32;
                tmpChar[i] = (char)tmpInt;
            }
        }
        string tmpString = new string(tmpChar);

        return tmpString;
    }
}
```

실제 실행 Program

```csharp
class Program
{
    static void Main()
    {
    	string strTest1 = "Hello World";
        string strTestUpper = StringUpDown.StringUpper(strTest1);
        string strTestLower = StringUpDown.StringLower(strTest1);
        Console.WriteLine("strTest1 Upper : {0}", strTestUpper);
        Console.WriteLine("strTest1 Lower : {0}", strTestLower);
    }
}
```

실행 결과

```text
strTest1 Upper : HELLO WORLD
strTest1 Lower : hello world
```

아스키 코드를 이용해서 upper는 소문자 범위일 경우 -32, lower는 대문자 범위일 경우 +32를 해서 대소문자가 반전되게 했다. 그 과정에서 string을 다른 자료형으로 변환하려 하니 오류가 계속 나서 string을 char배열로 바꾸는 ToCharArray에 대한 정보를 구글링해서 찾았다. 입력 자체를 char배열로 받는다면 ToCharArray 없이도 만들 수 있을 것 같다.
