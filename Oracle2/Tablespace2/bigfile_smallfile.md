BIGFILE과 SMALLFILE의 차이
===

### BIGFILE 테이블스페이스
1. 10g 버전부터 Addressing 기법이 변경되면서 하나의 데이터 파일에 40억개의 블록이 가능해짐
1. 그래서 구분하기 위해 기존의 테이블스페이스는 SMALLFILE 테이블스페이스로 명명

<br>

### BIGFILE 테이블스페이스 특징
1. Locally Managed 테이블스페이스 ➞ 데이터파일을 추가하는 작업이 없으므로 관리해야할 데이터파일이 줄어듬
    >기존 테이블스페이스와 데이터파일의 관계가 `1:M` 인 반면, BIGFILE 테이블스페이스는 논리적으로 `1:1` 이 됨
1. 저장 공간이 매우 큼 (8EB)

<br>

### BIGFILE 테이블스페이스 주의점
1. 동적으로 볼륨을 확장할 수 있어야하며 ASM (Automatic Storage Management) 또는 LVM (Logical Volume Manager) 을 사용할 수 있어야 한다 ➞ 스트라이핑/레이드를 지원하지 않으면 성능과 병렬처리 속도가 떨어지기 때문
1. BIGFILE 테이블스페이스의 ROWID는 `DBMS_ROWID` 패키지를 사용해야 함

<br>

### BIGFILE 테이블스페이스 ROWID
|SMALLFILE|BIGFILE|
|-|-|
|`OOOOOO FFF BBBBBB RRR`|`OOOOOO LLL LLLLLL RRR`|

|단위|설명|
|-|-|
|OOOOOO|세그먼트를 지정하는 데이터 오브젝트 Number|
|FFF|Relative 파일 Number|
|BBBBBB|실제 Row를 가지고 있는 데이터 블록 Number|
|RRR|특정 블록 안의 Row를 지정하고 있는 슬롯 Number|
|LLL, LLLLLL|Encoded 블록 Number|

<br>
