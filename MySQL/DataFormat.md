DataFormat
===

### COLLATION 비교
```
SELECT 할때 같은 값으로 인식하면 O, 아니라면 X
ci 표시가 있다면 기본적으로 대소문자 구분을 하지 않는다.

COLLATION                | A,a | はは,ぱぱ | はは,ハハ | びょういん,びよういん | 🍣,🍺 | ＋,+
utf8mb4_bin              |  X  |     X     |     X     |           X           |    X    |  X
utf8mb4_0900_bin         |  X  |     X     |     X     |           X           |    X    |  X
utf8mb4_0900_ai_ci       |  O  |     O     |     O     |           O           |    X    |  O
utf8mb4_0900_as_ci       |  O  |     X     |     O     |           O           |    X    |  O
utf8mb4_general_ci       |  O  |     X     |     X     |           X           |    O    |  X
utf8mb4_unicode_ci       |  O  |     O     |     O     |           O           |    O    |  O
utf8mb4_unicode_520_ci   |  O  |     O     |     O     |           O           |    X    |  O
utf8mb4_ja_0900_as_cs    |  X  |     X     |     O     |           X           |    X    |  O
utf8mb4_ja_0900_as_cs_ks |  X  |     X     |     X     |           X           |    X    |  O
```
>https://mita2db.hateblo.jp/entry/2020/12/07/000000

<br>

