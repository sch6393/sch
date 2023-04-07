Aurora
===
>[https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/CHAP_AuroraOverview.html](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/CHAP_AuroraOverview.html)

### Aurora의 구성
```
┌Amazon Aurora DB Cluster───────────────┐
│┌AZ a─────┐┌AZ b─────┐┌AZ c─────┐│
││ ┌────┐ ││ ┌────┐ ││ ┌────┐ ││
││ │Primary │ ││ │Replica │ ││ │Replica │ ││
││ │Instance│ ││ │Instance│ ││ │Instance│ ││
││ └────┘ ││ └────┘ ││ └────┘ ││
││ Read↑│     ││ Read↑       ││ Read↑       ││
││     │├───────│┬───────│┐     ││
││     │↓Write││     │↓Write││     │↓Write││
││┌───────────────────────┐││
│││ ┌┐┌┐          ┌┐┌┐          ┌┐┌┐ │││
│││ └┘└┘          └┘└┘          └┘└┘ │││
│││Data Copies       Data Copies      Data Copies│││
││└────────Cluster Volume────────┘││
│└───────┘└───────┘└───────┘│
└───────────────────────────┘
```
>[https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Aurora.Overview.html](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Aurora.Overview.html)

<br>

### Aurora의 특징
1. 인스턴스가 데이터를 가지고 있지 않음. 그러므로 인스턴스의 추가, 삭제가 매우 빠름

<br>
