DateTime
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
