Postgres Session
===

>[https://www.npmjs.com/package/connect-pg-simple](https://www.npmjs.com/package/connect-pg-simple)

### Node js에서 Postgres로 세션 사용
1. `connect-pg-simple` 추가
    ```js
    npm i connect-pg-simple
    ```

1. 해당 모듈 안의 `table.sql`을 실행
    ```sql
    CREATE TABLE "session" (
      "sid" varchar NOT NULL COLLATE "default",
      "sess" json NOT NULL,
      "expire" timestamp(6) NOT NULL
    )
    WITH (OIDS=FALSE);

    ALTER TABLE "session" ADD CONSTRAINT "session_pkey" PRIMARY KEY ("sid") NOT DEFERRABLE INITIALLY IMMEDIATE;

    CREATE INDEX "IDX_session_expire" ON "session" ("expire");
    ```

1. Session
    ```js
    import session from 'express-session';
    import pgSession from 'connect-pg-simple';
    const pgs = pgSession(session);

    app.use(session({
      store: new pgs({
        // id:pw@host:port/database
        conString: 'pg://postgres:postgres@localhost:5432/postgres',
        ttl: '60',
      }),
      key: 'key',
      secret: 'secret',
      resave: true,             // 세션 변경이 없어도 덮어 쓸지 설정
      secure: true,             // https
      httpOnly: true,           // js에서 세션, 쿠키를 사용할 수 없음
      saveUninitialized: false, // 초기화되지 않은 세션을 저장할지 설정
      cookie: {
        // 쿠키
      },
    }));

    console.log('Log:', req.session);
    ```

<br>

### 참조
* json 데이터를 SELECT
  ```sql
  SELECT sess -> 'cookie' -> 'expires' FROM session;
  ```
