Schema
===

### 이름 변경
* MySQL에선 스키마 이름 변경을 지원하는 명령어가 없음<br>
그러므로 덤프를 한 후 이름을 변경할 스키마에 넣어야 함

  1. 변경할 이름의 스키마 생성
      ```sql
      CREATE SCHEMA `schema_name_new` DEFAULT CHARACTER SET character_set_name COLLATE collate_name;
      ```

  1. 덤프
      ```sh
      mysqldump -u user_name -p schema_name_old > dmpfile.dmp
      ```

  1. 임포트
      ```sh
      mysql -u user_name -p -D schema_name_new < dmpfile.dmp
      ```

  1. 이전 스키마 제거
      ```sql
      DROP SCHEMA `schema_name_old`;
      ```

<br>
