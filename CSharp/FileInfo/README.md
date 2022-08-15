FileInfo
===

### 파일 정보
```C#
string strFilePath = @"C:\aaa\bbb.txt";
FileInfo fi = new FileInfo(strFilePath);
Console.WriteLine("File Size      : " + fi.Length + " Bytes");
Console.WriteLine("File Name      : " + fi.Name);
Console.WriteLine("Directory Name : " + fi.DirectoryName);
Console.WriteLine("Full Path      : " + fi.FullName);
Console.WriteLine("Create Time    : " + fi.CreationTime);
Console.WriteLine("Write Time     : " + fi.LastWriteTime);
Console.WriteLine("Access Time    : " + fi.LastAccessTime);
Console.WriteLine("Read Only      : " + fi.IsReadOnly);
```

<br>
