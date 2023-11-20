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



## 4. layout.tsx 파일 설정

```tsx
import type { Metadata } from 'next';
import { Inter } from 'next/font/google';
import './globals.css';
import { notFound } from 'next/navigation';

const locales = ['en', 'ru', 'ko']; // 사용할 언어 * 반드시 일치시켜야한다.
const inter = Inter({ subsets: ['latin'] });

export const metadata: Metadata = { // meatadata
  title: '타이틀',
  description: 'Description',
};

export default function LocaleLayout({
  children,
  params: { locale }, //다이나믹 라우트의 params
}: {
  children: React.ReactNode;
  params: { locale: any };
}) {
  // 만약 locales(해당하는 언어)가 없다면 notFound 호출
  if (!locales.includes(locale as any)) notFound();

  return (
    <html lang={locale}> // locale 변수를 lang에 사용
      <body className={inter.className}>{children}</body>
    </html>
  );
}
```

공식 홈페이지와 내용이 조금 다른데 사용하는 방법에 대한 로직은 같다.



## 5. 각  page에서 사용방법.

```tsx
import { useTranslations } from 'next-intl';

export default function Home() {
  const t = useTranslations('Home');
  return <h1>{t('title')}</h1>;
}
```

useTranslations 함수를 import해서 t라는 변수로 사용한다.

useTranslations의 인수는 미리 설정한 json 파일에서 일치하는 Key이다.\
만약 Key를 인식하지 못하면 termnal 콘솔에서 에러 메세지를 출력하며. 이상한? 텍스트가 출력된다.

반드시 언어별로  key는 매칭을 해줘야한다.

<figure><img src="../../.gitbook/assets/Image 196.png" alt=""><figcaption></figcaption></figure>

위와 같이 json을 설정하면 Key와 값을 사용할수 있다.

<figure><img src="../../.gitbook/assets/Image 197.png" alt=""><figcaption></figcaption></figure>

localhost:3000/ko 에 텍스트가 출력되는걸 확인할 수 있다.



각 페이지마다 다국어 번역이 필요한곳은 useTranslations를 사용해서 언어를 호출할 수 있다.

기본적인 사용법은 여기서 끝.
