Oracle DMS
===
>https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Source.Oracle.html

### 전체적인 순서
1. 이행할 환경에 DMS를 사용하기 위한 초기 설정
    * CDC 구성을 LogMinor 또는 Binary Reader로 설정 (LOB 타입을 CDC, 또는 이행할 환경이 RAC인 경우는 Binary Reader로 구성해야만 함)
    * DMS의 유저 권한 설정
    * 아카이브 로그 확인
    * Supplemental Logging 확인
    
    <br>

1. 이행 후 환경의 메타데이터를 구축
    * 유저 정보, 테이블 스페이스, 프로시저, 테이블, 인덱스 등 ➞ [`Datapump`](../../../oracle/datapump/README.md) 를 이용

    <br>

1. 데이터 이행
    * CDC가 되는 테이블
    * CDC가 안되는 테이블
    * LOB 데이터를 포함한 테이블

    <br>

<br>
