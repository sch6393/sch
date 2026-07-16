Amplify
===
>[https://docs.aws.amazon.com/amplify/latest/userguide/welcome.html](https://docs.aws.amazon.com/amplify/latest/userguide/welcome.html)

### 준비
[AWS CLI](../cli/README.md)를 설치, 설정할 필요가 있음

<br>

### 始め
1. vueで生成 (Vite基盤)

        npm create vue@latest
        cd vue-amplify-project
        npm install
        npm run dev      # 正常動作確認

1. 設定するオプション

        □ JSX Support
        ■ Router (SPA development)
        ■ Pinia (state management)
        ■ Vitest (unit testing)
        □ End-to-End Testing
        ■ Linter (error prevention)
        ■ Prettier (code formatting)

        ◻ Replace Prettier with Oxfmt
        ◻ Vue 3.6 (beta)

    >必要に応じて選択する。

1. Amplify App作成、環境作成

        aws amplify create-app --name vue-amplify-app   # 実行したらAPP IDが表示される
        aws amplify create-branch --app-id AWS_AMPLIFY_APP_ID --branch-name main

1. `.gitlab-ci.yml` 作成

        stages:
        - deploy

        cache:
        key: ${CI_COMMIT_REF_SLUG}
        paths:
            - node_modules/
            - .npm/

        amplify_backend_and_frontend_deploy:
        stage: deploy
        image: node:22
        variables:
            BRANCH_NAME: "main"
        script:
            # 1. Library and AWS CLI (aptget for Node Image)
            - apt-get update && apt-get install -y zip awscli
            - npm ci --cache .npm --prefer-offline
            
            # 2. Unit Test
            - npm run test:unit -- --run

            # 3. Backend infra deploy and create amplify_outputs.json
            - npx ampx pipeline-deploy --branch $BRANCH_NAME --app-id $AWS_AMPLIFY_APP_ID

            # 4. frontend build
            - npm run build

            # 5. AWS Amplify Hosting upload and deploy
            - cd dist && zip -r ../deployment.zip . && cd ..
            - |
            DEPLOY_TOKEN=$(aws amplify create-deployment --app-id $AWS_AMPLIFY_APP_ID --branch-name $BRANCH_NAME)
            UPLOAD_URL=$(echo $DEPLOY_TOKEN | grep -o '"zipUploadUrl": "[^"]*' | sed 's/"zipUploadUrl": "//')
            JOB_ID=$(echo $DEPLOY_TOKEN | grep -o '"jobId": "[^"]*' | sed 's/"jobId": "//')
            
            curl -H "Content-Type: application/zip" --upload-file deployment.zip "$UPLOAD_URL"
            aws amplify start-deployment --app-id $AWS_AMPLIFY_APP_ID --branch-name $BRANCH_NAME --job-id $JOB_ID

        only:
            - main

1. gitlab CI/CDに以下の環境変数登録
    * `AWS_ACCESS_KEY_ID`
    * `AWS_AMPLIFY_APP_ID`
    * `AWS_REGION`
    * `AWS_SECRET_ACCESS_KEY`

1. gitlabにPushして正常にDeployできるのか確認

### Backend追加
1. Backnd Amplify設置

        cd vue-amplify-project
        npm create amplify@latest

1. Sandbox実行

        npx amplify sandbox

1. `amplify_outputs.json` 生成確認

1. Amplify Library設置 (SandboxのConsoleとは別のConsoleで実行)

        npm install aws-amplify

1. `src/main.ts` 修正

        import { createApp } from 'vue'
        import { createPinia } from 'pinia'
        import App from './App.vue'
        import router from './router'

        // AWS Amplify Library
        import { Amplify } from 'aws-amplify'
        import outputs from '../amplify_outputs.json'

        // Init Backed Connection
        Amplify.configure(outputs)

        const app = createApp(App)

        app.use(createPinia())
        app.use(router)

        app.mount('#app')

1. 正常に作動できるのか確認

---

# ERROR

### 404エラー発生時
Vueの場合はSPAなのでRedirect時にRouterに設定されているファイルがない。

Hosting ➞ Rewrites and redirects ➞ 以下のJSONを追加

    [
        {
            "source": "</^[^.]+$|\\.(?!(css|gif|ico|jpg|js|png|txt|svg|woff|woff2|ttf|map|json)$)([^.]+$)/>",
            "target": "/index.html",
            "status": "200",
            "condition": null
        }
    ]
