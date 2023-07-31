Huge Page
===

### [HugePage 내용](../../linux/hugepage/README.md)

<br>

### 구성 방법
1. 적정한 HugePage 값 계산
    ```sql
    # (InnoDB Buffer 사이즈 + 10% 여유) / HugePage 사이즈
    SELECT (@@INNODB_BUFFER_POOL_SIZE + (@@INNODB_BUFFER_POOL_SIZE * 0.1 )) / 1024 / 1024 / 2 AS "Recommended setting: vm.nr_hugepages";
    /*
    +--------------------------------------+
    | Recommended setting: vm.nr_hugepages |
    +--------------------------------------+
    |                                  XXX |
    +--------------------------------------+
    */
    ```

1. 계산한 값으로 HugePages 변경 및 커널 파라미터 변경
```sh
sysctl -w vm.nr_hugepages=XXX
sysctl -w vm.swappiness=1

# mysql gid 값 확인
cat /etc/group | grep mysql
#mysql:x:XX:

# 확인된 gid 값으로 해당 파라미터 설정
sysctl -w vm.hugetlb_shm_group=XX

# 재기동 이후에도 적용될 수 있도록 함
vi /etc/sysctl.conf

# 추가할 내용
vm.nr_hugepages=XXX
vm.swappiness=1
vm.hugetlb_shm_group=XX
```

1. memlock 변경
```sh
vi /etc/security/limits.conf

# 추가할 내용
mysql soft memlock unlimited
mysql hard memlock unlimited
```

1. MySQL 설정 변경
```sh
vi /etc/my.cnf

# 추가할 내용 ([mysqld])
large-pages
```

1. 변경된 값 확인 및 MySQL 재기동
```sh
# 변경된 값 확인
sysctl -p

# HugePage 사용 현황 확인
cat /proc/meminfo | grep -i huge

# MySQL 재기동
service mysqld restart
```

1. MySQL에서 사용하고 있는 HugePages 수 확인
```sh
# pl 파일 생성
vi usedhugepages.pl

# 추가할 내용
sub UsedHugePages {
    my $pid=$_[0];
    open (NUMAMAPS, "/proc/$pid/numa_maps") || die "can't open numa_maps";
    my $HUGEPAGECOUNT=0;

    while (<NUMAMAPS>) {
        if (/huge.*dirty=(\d+)/) {
            $HUGEPAGECOUNT+=$1;
        }
    }

    close NUMAMAPS;

    return ($HUGEPAGECOUNT);
}

printf "%d huge pages\n", UsedHugePages($ARGV[0]);

# mysql pid
ps -ef | grep mysql | grep -v root

# pl 파일 샐행
perl usedhugepages.pl mysql_pid
#XXX huge pages
```

<br>
