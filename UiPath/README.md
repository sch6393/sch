UiPath (C#)
===

### 오브젝트를 반드시 확인하고 넘어가는 방법
Element Exists 액티비티에 Timeout 수치를 조정하는 방법이 일반적이지만 무한정으로 값을 조정할 수는 없음. 그러므로 Element Exists 액티비티를 While 안에 선언 후 확인됐을 때 While을 나가도록 하면 됨.
  1. While 액티비티를 선언하고 그 안에 Element Exists를 선언
  2. boolean 변수를 하나 선언하여 Element Exists의 Output에 할당
  3. While 조건에 boolean 변수가 true일 때 나가도록 설정 



<br>

### 

