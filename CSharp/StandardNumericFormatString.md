Standard Numeric Format String
===

>[https://docs.microsoft.com/en-us/dotnet/standard/base-types/standard-numeric-format-strings](https://docs.microsoft.com/en-us/dotnet/standard/base-types/standard-numeric-format-strings)

<br>

### 정수 표현 형식
```C#
int iNumber = 1234;
Console.WriteLine(iNumber.ToString("D"));
// Displays 1234

Console.WriteLine(iNumber.ToString("D8"));
// Displays 00001234
```

<br>

### 실수 표현 형식
```C#
double dNumber = 123.456;
Console.WriteLine(dNumber.ToString("F"));
// Displays 123.46

Console.WriteLine(dNumber.ToString("F0"));
// Displays 123

Console.WriteLine(dNumber.ToString("F1"));
// Displays 123.5

Console.WriteLine(dNumber.ToString("F3", CultureInfo.CreateSpecificCulture("ja-JP")));
// Displays 123.456
```

<br>

### 

