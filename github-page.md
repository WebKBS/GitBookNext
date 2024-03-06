---
description: "\breact와 github page 연동해서 배포하기"
---

# Github Page

기술블로그를 쓰기위해서 미리보기 페이지를 만들기위해

react를 사용해서 github page에 정적 사이트를 배포해보자.



## React project 생성

```bash
npm create vite@latest
```



## 패키지 설치

```bash
npm install gh-pages --save-dev
```



## 패키지 scripts에 추가

```json
"scripts": {
  // ....
  "predeploy": "npm run build",
  "deploy": "gh-pages -d dist",
}
```



최상단에 homepage를 추가한다.

```json
"homepage": "https://<username>.github.io/<repo-name>/"
```

github 이름과 저장소 이름을 추가한다.



{% hint style="warning" %}
vite 사용시 반드시 vite.config.ts 에서 base를 지정해줘야한다.

이거 하지않으면 배포시 아무것도 나타나지 않음.
{% endhint %}



```typescript
import react from '@vitejs/plugin-react';
import { defineConfig } from 'vite';

// https://vitejs.dev/config/
export default defineConfig({
  base: '/저장소 이름/',
  plugins: [react()],
});
```



이제 배포를 해준다.

```bash
npm run deploy
```



깃허브 저장소에서 setting -> page에서 배포 브렌치가 gh-pages로 되어있는것을 확인해보고 이제 해당 url로 접속하면 배포가 제대로 된것을 확인할 수 있다.
