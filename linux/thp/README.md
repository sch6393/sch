Transparent HugePages
===

### 내용
* HugePage 내용은 [관련 문서](../hugepage/README.md) 참고
* HugePage는 커널 파라미터를 지정해 필요할 때 마다 변경, 적용하거나 재부팅 등 관리 요소를 필요로 하였기에 자동으로 관리해주는 Transparent Huge Pages (THP) 가 도입
* 그러나 THP를 사용함으로서 오히려 더 성능 하락이나 충돌 등 여러 문제가 발생하는 경우가 많아짐에 따라 일부 어플리케이션에서는 THP를 사용하지 않도록 권장하고 있음

<br>

### 확인
```sh
cat /sys/kernel/mm/transparent_hugepage/enabled
cat /sys/kernel/mm/transparent_hugepage/defrag

# 활성화가 되어 있는 경우는 always에 [] 표시가 나타남
#[always] madvise never
#[always] defer defer+madvise madvise never

# 비활성화가 되어 있는 경우는 never에 [] 표시가 나타남
#always madvise [never]
#always defer defer+madvise madvise [never]
```

<br>

### 비활성화 방법
1. 서비스 생성 및 등록, 시작
    ```sh
    vi /etc/systemd/system/disable-thp.service

    # 추가할 내용
    [Unit]
    Description=Disable Transparent Huge Pages (THP)
    [Service]
    Type=simple
    ExecStart=/bin/sh -c "echo 'never' >/sys/kernel/mm/transparent_hugepage/enabled && echo 'never' >/sys/kernel/mm/transparent_hugepage/defrag"
    [Install]
    WantedBy=multi-user.target

    # 서비스 시작
    systemctl daemon-reload
    systemctl start disable-thp
    systemctl enable disable-thp
    systemctl status disable-thp
    ```

1. 커널 명령어 수정
    ```sh
    vi /etc/default/grub

    # GRUB_CMDLINE_LINUX에 transparent_hugepage 파라미터 추가
    GRUB_CMDLINE_LINUX_DEFAULT="console=tty0 console=ttyS0,115200n8 nvme_core.io_timeout=4294967295 rd.emergency=poweroff rd.shell=0 selinux=1 security=selinux quiet transparent_hugepage=never"

    # 변경사항 적용
    grub2-mkconfig -o /boot/grub2/grub.cfg
    ```

<br>
