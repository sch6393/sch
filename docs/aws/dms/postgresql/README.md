PostgreSQL DMS
===
>https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Source.PostgreSQL.html

### 전체적인 순서
1. 이행할 환경에 DMS를 사용하기 위한 초기 설정
    * CDC 구성을 위해 WAL 설정
    * DMS의 유저에 SELECT 권한 및 슈퍼유저 권한 설정
    * `pg_hba.conf` 로 복제 서버와의 소켓 연결 설정
    
    <br>

1. 이행 후 환경의 메타데이터를 구축
    * 유저 정보, 테이블 스페이스, 프로시저, 테이블, 인덱스 등 ➞ [`pg-dump`](../../../postgresql/pg-dump/README.md) 를 이용

    <br>

1. 데이터 이행
    * 각 유저 (스키마) 별로 구성하는 것이 좋음. 그 후 각 환경의 기준에 맞춰서 구성함으로서 효율적인 태스크 관리
        * CDC가 되는 테이블
        * CDC가 안되는 테이블
        * LOB 데이터를 포함한 테이블

* 데이터가 많다면 AWS DMS 스크립트를 실행해 AWS 서포트에게 보내서 분석결과를 받을 수 있음
    >https://docs.aws.amazon.com/dms/latest/userguide/CHAP_SupportScripts.PostgreSQL.html

<br>

### PostgreSQL에서 CDC를 사용할 때의 주의점
태스크 개시조차 되지 않는 경우가 있기 때문에 제어 테이블 설정에서 `public`을 명시적으로 넣어주는 편이 좋음

<br>
