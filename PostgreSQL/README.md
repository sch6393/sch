SHOW timezone;

PostgreSQL에서는 보편적으로 UTC로 시간 데이터를 저장후, 애플리케이션 / Gui Client에서의 타임존에 맞춰 노출시키는 방식을 선택하는데요.

해당 컬럼의 타입이 timestamp with time zone 로 되어있어야만 합니다.

Client Timezone에 따라 자동 전환이 되다보니 서버 애플리케이션에서 접속 하게 되면 UTC가 KST로 잘 전환이되어 노출 되는데요.
