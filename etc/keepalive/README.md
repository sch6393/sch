Keepalive
===

### HTTP Keepalive
1. Apache나 Nginx 같은 웹 어플리케이션의 Keepalive Timeout을 의미함.
1. TCP Keepalive와는 다르게 설정 시간 동안 요청이 없다면 스스로 연결을 끊음
1. 즉, LB의 유휴 제한 시간보다 짧다면 LB는 세션이 끊긴 지 모르고 요청을 보내게 됨. LB의 유휴 제한 시간보다 길게 설정해야함

<br>

### TCP Keepalive
1. 커널 파라미터로 소켓 통신 간에 두 종단 간의 연결을 유지해줌.
1. `net.ipv4.tcp_keepalive_time` 의 설정값 주기로 Keepalive 신호를 보내 응답이 온다면 연결을 유지함.
1. 즉, 소켓 간에 패킷을 주기적으로 던져서 요청이 없더라도 강제로 연결을 유지시키게 됨. LB의 유휴 제한 시간보다 짧게 설정해야함

* 참고
    ```sh
    sudo sysctl -a | grep -i keepalive
    #net.ipv4.tcp_keepalive_intvl = 10
    #net.ipv4.tcp_keepalive_probes = 3
    #net.ipv4.tcp_keepalive_time = 300
    ```

<br>
