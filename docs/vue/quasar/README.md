Quasar
===

>[https://quasar.dev/](https://quasar.dev/)

### 설치
```
npm init quasar

√ What would you like to build? » App with Quasar CLI, let's go!
√ Project folder: ... project_name
√ Pick Quasar version: » Quasar v2 (Vue 3 | latest and greatest)
√ Pick script type: » Javascript
√ Pick Quasar App CLI variant: » Quasar App CLI with Webpack
√ Package name: ... project_name
√ Project product name: (must start with letter if building mobile apps) ... Quasar App
√ Project description: ... A Quasar Project
√ Author: ... Son ChangHan <sch6393@gmail.com>
√ Pick your CSS preprocessor: » Sass with SCSS syntax
√ Check the features needed for your project: » ESLint, Axios
√ Pick an ESLint preset: » Prettier

 Quasar • Generating files...

 - babel.config.js
 - quasar.config.js
 - README.md
 - .editorconfig
 - .gitignore
 - .postcssrc.js
 - jsconfig.json
 - package.json
 - public/favicon.ico
 - src/App.vue
 - src/index.template.html
 - .vscode/extensions.json
 - .vscode/settings.json
 - public/icons/favicon-128x128.png
 - public/icons/favicon-16x16.png
 - public/icons/favicon-32x32.png
 - public/icons/favicon-96x96.png
 - src/assets/quasar-logo-vertical.svg
 - src/boot/.gitkeep
 - src/components/EssentialLink.vue
 - src/layouts/MainLayout.vue
 - src/pages/ErrorNotFound.vue
 - src/pages/IndexPage.vue
 - src/router/index.js
 - src/router/routes.js
 - src/css/app.scss
 - src/css/quasar.variables.scss
 - src/boot/axios.js
 - .eslintignore
 - .eslintrc.js

 Quasar •  SUCCESS  • The project has been scaffolded

√ Install project dependencies? (recommended) » Yes, use npm

npm WARN deprecated stable@0.1.8: Modern JS already guarantees Array#sort() is a stable sort, so this library is deprecated. See the compatibility table on MDN: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort#browser_compatibility

added 926 packages, and audited 927 packages in 38s

122 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities



> ppp@0.0.1 lint
> eslint --ext .js,.vue ./



To get started:

  cd project_name
  quasar dev # or: yarn quasar dev # or: npx quasar dev

Documentation can be found at: https://v2.quasar.dev

Quasar is relying on donations to evolve. We'd be very grateful if you can
read our manifest on "Why donations are important": https://v2.quasar.dev/why-donate
Donation campaign: https://donate.quasar.dev
Any amount is very welcome.
If invoices are required, please first contact Razvan Stoenescu.

Please give us a star on Github if you appreciate our work:
  https://github.com/quasarframework/quasar

Enjoy! - Quasar Team
```

<br>

### 실행
```
quasar dev
```

<br>

### 빌드
```
quasar build
```

<br>

### 파이어베이스 호스팅
1. `npm install -g firebase-tools`
1. `firebase login`
1. `firebase init`
1. `? Are you ready to proceed?` ➞ Yes
1. `Hosting: Configure files for Firebase Hosting and (optionally) set up GitHub Action deploys`
1. `Use an existing project` ➞ sch6393-system (sch6393-system)
1. `? What do you want to use as your public directory?` ➞ dist/spa
1. `? Configure as a single-page app (rewrite all urls to /index.html)` ➞ No
1. `? Set up automatic builds and deploys with GitHub?` ➞ No
1. `? File build/web/index.html already exists. Overwrite?` ➞ No

<br>

### 파이어베이스 디플로이
* `firebase deploy`

<br>

### 파이어베이스 깃헙 액션
1. `firebase init`
1. `? Are you ready to proceed?` ➞ Yes
1. `Hosting: Set up GitHub Action deploys`
1. Firebase CLI Login
1. `? For which GitHub repository would you like to set up a GitHub workflow? (format: user/repository)` ➞ sch6393/sch6393-system-quasar
1. `? Set up the workflow to run a build script before every deploy?` ➞ Yes
1. `? What script should be run before every deploy?` ➞ quasar build
    ```
    "build": "quasar build",
    ```
1. `? Set up automatic deployment to your site's live channel when a PR is merged?` ➞ Yes
1. `? What is the name of the GitHub branch associated with your site's live channel?` ➞ main

<br>

### 파이어베이스 추가
```
quasar new boot firebase
```
```js
// src/boot/firebase.js 수정
import { initializeApp } from 'firebase/app';

// TODO: Replace the following with your app's Firebase project configuration
const firebaseConfig = {
  //...
};

const app = initializeApp(firebaseConfig);
```
```js
// quasar.config.js 수정
boot: [
  'firebase'
  
],
```

<br>

### axios 추가
```
quasar new boot axios
```
```js
// src/boot/axios.js 수정
import { boot } from 'quasar/wrappers'
import axios from 'axios'

// Be careful when using SSR for cross-request state pollution
// due to creating a Singleton instance here;
// If any client changes this (global) instance, it might be a
// good idea to move this instance creation inside of the
// "export default () => {}" function below (which runs individually
// for each client)
const api = axios.create({ baseURL: 'https://api.example.com' })

export default boot(({ app }) => {
  // for use inside Vue files (Options API) through this.$axios and this.$api

  app.config.globalProperties.$axios = axios
  // ^ ^ ^ this will allow you to use this.$axios (for Vue Options API form)
  //       so you won't necessarily have to import axios in each vue file

  app.config.globalProperties.$api = api
  // ^ ^ ^ this will allow you to use this.$api (for Vue Options API form)
  //       so you can easily perform requests against your app's API
})

export { api }
```
```js
// quasar.config.js 수정
boot: [
  'axios'
  
],
```

<br>

### pinia 추가
```
quasar new store store_name
```
```js
// 변수 선언
state: () => ({
  counter: 0
}),

// 값 가져오기
getters: {
  gettersCounter: (state) => state.counter,
},

// 액션
actions: {
  increment () {
    this.counter++;
  }
},
```

<br>
