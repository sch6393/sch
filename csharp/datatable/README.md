DataTable
===

### 기본 형식
```C#
DataTable dt = new DataTable();

// Add Column
dt.Columns.Add("A", typeof(string));
dt.Columns.Add("B", typeof(string));
dt.Columns.Add("C", typeof(string));

// Create Data Row
DataRow dr = dt.NewRow();
dr["A"] = "A";
dr["B"] = "B";
dr["C"] = "C";

// Add Row
dt.Rows.Add(dr);

// Get Data
// i : Row, j : Column
string strtmp = dt.Rows[i][j].ToString();
```

<br>

### 원본 객체와 동일하게 할 경우
```C#
DataTable dttmp = new DataTable();

// 스키마, 관계, 제약 조건들을 원본 객체와 동일하게 적용
dttmp = dtSource.Clone();

// 특정 행에 대한 데이터 복사
dttmp.ImportRow(dtSource.Rows[0]);

// 전체 복사
dttmp = dtSource.Copy();
```

<br>

### 필터링
```C#
DataTable dttmp = new DataTable();

// Select
foreach (DataRow dr in dtSource.Select("column_name = 'value'"))
{
    dttmp.ImportRow(dr);
}

// DataView의 Filter
DataView dv = dtSource.DefaultView;
dv.RowFilter = "column_name = 'value'";
for (int i = 0; i <= dv.Count; i++)
{
    dttmp.ImportRow(dv[i].Row);
}
```

<br>
