Data Format
===
>[https://mita2db.hateblo.jp/entry/2020/12/07/000000](https://mita2db.hateblo.jp/entry/2020/12/07/000000)

### COLLATION 비교
|COLLATION|A,a|はは,<br>ぱぱ|はは,<br>ハハ|びょういん,<br>びよういん|🍣,🍺|＋,+|
|-|-|-|-|-|-|-|
|utf8mb4_bin|X|X|X|X|X|X|
|utf8mb4_0900_bin|X|X|X|X|X|X|
|utf8mb4_0900_ai_ci|O|O|O|O|X|O|
|utf8mb4_0900_as_ci|O|X|O|O|X|O|
|utf8mb4_general_ci|O|X|X|X|O|X|
|utf8mb4_unicode_ci|O|O|O|O|O|O|
|utf8mb4_unicode_520_ci|O|O|O|O|X|O|
|utf8mb4_ja_0900_as_cs|X|X|O|X|X|O|
|utf8mb4_ja_0900_as_cs_ks|X|X|X|X|X|O|
>SELECT 할때 같은 값으로 인식하면 O, 아니라면 X<br>ci 표시가 있다면 기본적으로 대소문자 구분을 하지 않는다.

<br>

### ci 비교
* general_ci

  속도를 높이는데 중점을 두었기 때문에 빠르며 일반적인 경우 이쪽을 사용함. 다만 `À`, `Á`, `Å`, `å`, `ā`, `ă` 등 과 같은 악센트 문자가 없기 때문에 해당 단어의 대문자인 `A` 로 치환, 비교

* unicode_ci

  유니코드 규칙을 기반으로 하여 정확한 정렬을 목표로 수행

<br>
