mysqld
===

### 기동 순서
|순서|파일 이름|설명|
|-|-|-|
|1|`/etc/my.cnf`|Global Options|
|2|`/etc/mysql/my.cnf`|Global Options|
|3|`*SYSCONFDIR*/my.cnf`|Global Options|
|4|`$MYSQL_HOME/my.cnf`|여러 인스턴스가 있는 경우 특정 인스턴스용으로 사용|
|5|`defaults-extra-file`|`defaults-extra-file` 이 지정된 경우 사용 (공통 my.cnf가 있고 특정 인스턴스에 다른 옵션을 적용하는 경우)|
|6|`~/.my.cnf`|특정 OS 유저에 맞는 설정으로 사용|
|7|`~/.mylogin.cnf`|특정 OS 유저에 맞는 로그인 옵션으로 사용|
|8|`*DATADIR*/mysqld-auto.cnf`|마지막으로 [SET PERIST 혹은 SET PERIST_ONLY](../perist/README.md) 에 의해 설정된 파일|

<br>
