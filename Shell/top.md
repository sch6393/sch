top
===

### 내용
```sh
top - 20:19:26 up 146 days, 23:34,  7 users,  load average: 0.00, 0.01, 0.05
Tasks: 125 total,   1 running, 123 sleeping,   1 stopped,   0 zombie
%Cpu(s):  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem :  1842980 total,    80880 free,   231004 used,  1531096 buff/cache
KiB Swap:        0 total,        0 free,        0 used.  1328400 avail Mem

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
    1 root      20   0  191248   3700   2088 S   0.0  0.2  23:18.16 systemd
    2 root      20   0       0      0      0 S   0.0  0.0   0:01.69 kthreadd
    4 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 kworker/0:0H
    6 root      20   0       0      0      0 S   0.0  0.0   1:00.35 ksoftirqd/0
    7 root      rt   0       0      0      0 S   0.0  0.0   0:01.83 migration/0
    8 root      20   0       0      0      0 S   0.0  0.0   0:00.00 rcu_bh
  ...
```

<br>

### Load Average
```sh
# 1분, 5분, 15분 동안 대기하고 있었던 태스크 수 평균
load average: 0.00, 0.01, 0.05
```

<br>

### 계산
* 커널이 CPU를 사용하기 위해 대기 중인 프로세스 (RUNNING), I/O 완료를 대기 중인 프로세스 (UNINTERRUPTIBLE) 의 수를 계산한 값 
  >Load Average로 확인할 수 있는 시스템 부하는 CPU와 I/O 2가지
  <br>
  CPU 바운드인지 I/O 바운드인지 확인 ➞ [sar](./sar.md)
