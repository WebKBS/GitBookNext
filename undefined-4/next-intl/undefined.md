---
description: 설치 및 세팅 방법
---

# 설치 및 세팅

```bash
npm install next-intl
```



## 1. 파일 설정

next-intl 패키지를 설치하고 아래와 같이 파일구조를 맞춰서 파일을 생성한다.

```
├── messages
│   ├── en.json
│   └── ...
├── next.config.js
└── src
    ├── i18n.ts
    ├── middleware.ts
    └── app
        └── [locale]
            ├── layout.tsx
            └── page.tsx
```

{% hint style="info" %}
messages 폴더에 json파일은 실제 텍스트가 번역될 공간이다. 각 원하는 언어의 json을 설정하면 된다.
{% endhint %}



## 2. 파일 셋팅

### messages/en.json

```json
{
  "Index": {
    "title": "Hello world!"
  }
}
```

다국어 번역될 json 파일이다.



### next.config.js

```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {};

const withNextIntl = require('next-intl/plugin')();

module.exports = withNextIntl(nextConfig);

```

next.config.js 파일에서 위와 같이 플러그인을 셋팅해준다.

nextConfig를  반드시withNextIntl 로 감싸주자.



### src/i18n.ts

```typescript
import {getRequestConfig} from 'next-intl/server';
 
export default getRequestConfig(async ({locale}) => ({
  messages: (await import(`../messages/${locale}.json`)).default
}));
```

src/i18n.ts 파일을 위와같이 설정하자.

import ../messages 의 파라미터로 들어간 locale을 사용해 웹사이트 첫 로딩될 default 설정된 json 파일을 사용해서 사이트에 제공한다. (아래  middleware.ts에서 default를 설정한다.)



### middleware.ts

```typescript
import createMiddleware from 'next-intl/middleware';
 
export default createMiddleware({
  // 지원 할 locales 목록
  locales: ['en', 'de'],
 
  // 기본 locales, src폴더 i18n.ts에서 설정한 default 언어
  defaultLocale: 'en'
});
 
export const config = {
  // 국제화된 경로 이름
  matcher: ['/', '/(de|en)/:path*']/
};

```



## 3. app 폴더의 파일을 교체한다.

<figure><img src="../../.gitbook/assets/Image 195.png" alt=""><figcaption></figcaption></figure>

\[locale] 폴더를 만든다.

layout.tsx와 page.tsx, globals.css 파일을 \[locale] 폴더 안으로 이동 시킨다.

{% hint style="info" %}
\[ ] 대괄호를 써서 폴더 이름을 만드는 것은. 다이나믹 라우팅때문에 만드는 것이다.

다국어라 언어마다 페이지가 필요하기  때문에 반드시 위처럼 세팅을 해주어야한다.\


&#x20;\* 다이나믹  라우팅

[https://nextjs.org/docs/app/building-your-application/routing/dynamic-routes](https://nextjs.org/docs/app/building-your-application/routing/dynamic-routes)
{% endhint %}
