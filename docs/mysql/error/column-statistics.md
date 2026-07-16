Warning: column statistics not supported by the server.
===
>옵티마이저가 [ANALYZE](../analyze/README.md)를 생성하지 못함

1. mysqldump 사용 시 8.0 이후로 [ANALYZE](../analyze/README.md)를 자동으로 생성함 (통계 목적)

1. 8.0에서 5.7 이하에 접근해 mysqldump를 사용하면 해당 표시가 발생함

1. `--skip-column-statistics` 옵션을 사용해면 해당 표시가 발생하지 않음
