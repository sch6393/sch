DateTime
===

>[https://docs.microsoft.com/en-us/dotnet/standard/base-types/custom-date-and-time-format-strings?redirectedfrom=MSDN](https://docs.microsoft.com/en-us/dotnet/standard/base-types/custom-date-and-time-format-strings?redirectedfrom=MSDN)

### 기본 날짜 표현 형식
```C#
// 년(0000) 월(00) 일(00) 시(24) 분(00) 초(00)
  "yyyy     MM     dd     HH     mm     ss"
```

<br>

### 현재 날짜 가져오기
```C#
string strDatetime = DateTime.Now.ToString("yyyyMMdd HHmmss");
```

<br>

### 매주 특정 요일 날짜 취득
```C#
// Monday
DateTime dtToday = DateTime.Today;
DateTime dtMonday = dtToday.AddDays(DayOfWeek.Monday - dtToday.DayOfWeek);
strDate = dtMonday.ToString("yyyMMdd");
```
```C#
namespace System
{
  [Serializable]
  [ComVisible(true)]
  public enum DayOfWeek
  {
    Sunday = 0,
    Monday = 1,
    Tuesday = 2,
    Wednesday = 3,
    Thursday = 4,
    Friday = 5,
    Saturday = 6,
  }
}
```

<br>

### 날짜 계산
```C#
DateTime dt1 = new DateTime(2022, 02, 02);
DateTime dt2 = new DateTime(2022, 07, 07);
Console.WriteLine((dt2 - dt1).Days);
```

<br>
