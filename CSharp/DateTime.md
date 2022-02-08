DateTime
===

### 기본 날짜 표현 형식
```C#
// 년(0000) 월(00) 일(00) 시(24) 분(00) 초(00)
  "yyyy     MM     dd     HH     mm     ss"
```
>https://docs.microsoft.com/en-us/dotnet/standard/base-types/custom-date-and-time-format-strings?redirectedfrom=MSDN

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
