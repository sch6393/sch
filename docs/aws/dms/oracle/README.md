Oracle DMS
===
>https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Source.Oracle.html

### 전체적인 순서
1. 이행할 환경에 DMS를 사용하기 위한 초기 설정
    * CDC 구성을 LogMinor 또는 Binary Reader로 설정 (LOB 타입을 CDC, 또는 이행할 환경이 RAC인 경우는 Binary Reader로 구성해야만 함)
    * DMS의 유저 권한 설정
    * 아카이브 로그 확인
    * Supplemental Logging 확인
    * 파라미터 확인 (PFILE, SPFILE)
    
    <br>

1. 이행 후 환경의 메타데이터를 구축
    * 유저 정보, 테이블 스페이스, 프로시저, 테이블, 인덱스 등 ➞ [`Datapump`](../../../oracle/datapump/README.md) 를 이용
    * 파라미터 그룹 작성
    * 옵션 그룹 작성 (타임존, S3 INTEGRATION 등)
    * 각 유저 별로 타겟 엔드포인트를 생성

    <br>

1. 데이터 이행 (태스크 구성)
    * 각 유저 (스키마) 별로 구성하는 것이 좋음. 그 후 각 환경의 기준에 맞춰서 구성함으로서 효율적인 태스크 관리
        * CDC가 되는 테이블
        * CDC가 안되는 테이블
        * LOB 데이터를 포함한 테이블

    <br>

* 데이터가 많다면 AWS DMS 스크립트를 실행해 AWS 서포트에게 보내서 분석결과를 받을 수 있음
    >https://docs.aws.amazon.com/dms/latest/userguide/CHAP_SupportScripts.Oracle.html

<br>
