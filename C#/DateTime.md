DateTime
===

### 기본 날짜 표현 형식
```C#
"yyyy MM dd HH mm ss"
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

