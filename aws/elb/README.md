ELB
===
>https://aws.amazon.com/elasticloadbalancing/?nc1=h_ls

### Elastic Load Balancing
하나 이상의 가용 영역 (AZ) 에 있는 여러 대상 및 가상 어플라이언스에서 들어오는 애플리케이션 트래픽을 자동으로 분산함

<br>

### 종류
||ALB<br>(Application)|NLB<br>(Network)|GLB<br>(Gateway)|CLB<br>(Classic)|
|-|-|-|-|-|
|로드 밸런서 유형|계층 7|계층 4|계층 3 + 계층 4<br>(Gateway + LB)|계층 4, 7|
|대상 유형|IP, 인스턴스, 람다|IP, 인스턴스, ALB|IP, 인스턴스||
|프록시 동작 종료|예|예|아니오|예|
|프로토콜 리스너|HTTP, HTTPS, gRPC|TCP, UDP, TLS|IP|TCP, SSL/TLS, HTTP, HTTPS|
|연결 가능|VIP|VIP|라우팅 테이블 목록||

<br>

### NLB 특징
>https://medium.com/tenable-techblog/lessons-from-aws-nlb-timeouts-5028a8f65dda
1. NLB의 경우 유휴 제한 시간이 350초로 고정되어있고 수정할 수 없기 때문에 세션 끊김에 대한 문제가 발생할 수 있음
1. NLB는 350초가 지나면 클라이언트, 서버 아무에게도 알려주지 않고 세션을 없애버리기 때문에 서버 (1번) 는 계속 소켓을 열어두게 됨. 350초가 지난 후에는 소켓이 열려있지 않은 서버 (2번) 에 Handshake가 되어있지도 않는 요청이 들어오니 [`RST`](../../etc/tcp-flag/README.md#rst-reset)를 보내게 됨
1. ALB는 유휴 제한 시간 수정이 가능하고 해당 시간이 지나면 [`FIN`](../../etc/tcp-flag/README.md#fin-finish)으로 종단 간의 종료를 알리게 되기 때문에 소켓이 종료가 되면 [`RST`](../../etc/tcp-flag/README.md#rst-reset)를 반환하지 않음
1. __해결책__
    * `net.ipv4.tcp_keepalive_time` 파라미터 값을 350미만으로 설정
    * Application에서 `SO_KEEPALIVE` 설정
       >위 2개가 AND 조건임

1. 참고
    ```sh
    sudo sysctl -a | grep -i keepalive
    #net.ipv4.tcp_keepalive_intvl = 10
    #net.ipv4.tcp_keepalive_probes = 3
    #net.ipv4.tcp_keepalive_time = 300
    ```

<br>
