UiPath (C#)
===

### 오브젝트를 반드시 확인하고 넘어가는 방법
* `Element Exists` 액티비티에 Timeout 수치를 조정하는 방법이 일반적이지만 무한정으로 값을 조정할 수는 없음. 그러므로 `Element Exists` 액티비티를 `While` 액티비티 안에 선언 후 확인됐을 때 `While` 액티비티를 나가도록 하면 됨.
  1. `While` 액티비티를 선언하고 그 안에 `Element Exists` 액티비티를 선언
  2. boolean 변수를 하나 선언하여 `Element Exists` 액티비티의 Output에 할당
  3. `While` 액티비티의 조건에 boolean 변수가 true일 때 나가도록 설정

<br>

### 설렉터에 변수 할당
* `{{변수명}}`

<br>

### 마우스 우클릭을 Send Hotkey로 처리하는 방법
* `Shift + F10`

<br>

### 크로미움 기반 브라우저의 URL 복사하기
* `Alt + d`를 하면 주소창이 선택된 상태로 바뀌게 되는데 그 때 `Copy Selected Text` 액티비티로 가져올 수 있다.

<br>

### 컴파일러 오류 CS0542
```
멤버가 생성자가 아닌 경우 클래스 또는 구조체의 멤버는 클래스 또는 구조체와 동일한 이름을 가질 수 없습니다
メンバー名をそれを囲む型の名前と同じにすることはできません
```
* xaml 파일 이름을 확인하고 클래스, 구조체 이름과 같지 않도록 변경

<br>

### 컴퓨터 기본 디렉토리 가져오기
* `Get Environment Folder` 액티비티 사용

<br>

