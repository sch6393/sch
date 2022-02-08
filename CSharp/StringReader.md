StringReader
===

### string을 한 줄씩 읽어서 처리
```C#
string strtmp = "";
string strLine = "";
StringReader sr = new StringReader(strtmp);

while ((strLine = sr.ReadLine()) != null)
{
  // Logic
}
```
