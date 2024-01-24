---
description: NEXT JS Progressive Web App
---

# PWA

설치

```bash
npm i @ducanh2912/next-pwa
```



## next.config.mjs 설정

```javascript
/** @type {import('next').NextConfig} */

import withPWAInit from '@ducanh2912/next-pwa';

const withPWA = withPWAInit({
  dest: 'public',
  cacheOnFrontEndNav: true, // 사용자가 페이지를 이동할 때 캐시를 사용할지 여부를 결정한다.
  aggressiveFrontEndNavCaching: true, // <link rel="stylesheet"> 및 <script> 태그를 캐시한다.
  reloadOnOnline: true, // 앱이 온라인 상태로 전환될 때 새로고침한다.
  disable: false, // PWA 비활성화 여부
  workboxOptions: {
    disableDevLogs: true, // 개발 모드에서 로그를 활성화한다.
  },
});

const nextConfig = {};

export default withPWA(nextConfig);
```

## manifest.json 파일을 public 폴더 안에 생성

public/manifest.json

```json
{
  "name": "My awesome PWA app",
  "short_name": "PWA App",
  "icons": [
    {
      "src": "/app-192.png", // 이미지는 반드시 png로
      "sizes": "192x192",
      "type": "image/png",
      "purpose": "maskable" // 마스크할 이미지 - 반드시 필요.
    },
    {
      "src": "/app-256.png",
      "sizes": "256x256",
      "type": "image/png"
    },
    {
      "src": "/app-512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ],
  "theme_color": "#030512", // 배경색 지정
  "background_color": "#030512", // 배경색 지정
  "start_url": "/",
  "display": "standalone",
  "orientation": "portrait"
}
```

각 컨셉에 맞춰 설정을 하면 된다. 단 이미지는 반드시 png파일로 해야하고, 192 사이즈와 512사이즈 이미지는 필수로 있어야한다.&#x20;



## 최상단 layout.tsx에서 metadata 및 viewport 설정

```typescript
const APP_TITLE = '타이틀 ';
const APP_DESCRIPTION = ' description ';

export const metadata: Metadata = {
  metadataBase: new URL('https://your-url/'), // 베이스 url, og 이미지 설정시 필요
  robots: 'index,follow',  // robots 크롤링 허용
  applicationName: APP_TITLE,
  title: {
    default: APP_TITLE,
    template: '%s | ' + APP_TITLE,
  },
  icons: {
    icon: '/app-192.png',
    apple: '/app-192.png',
    shortcut: '/app-192.png',
  },
  description: APP_DESCRIPTION,
  keywords: [
   "키워드",
   "키워드"
  ],
  manifest: '/manifest.json',
  appleWebApp: {
    capable: true,
    title: APP_TITLE,
    statusBarStyle: 'default',
    startupImage: '/app-512.png',
  },
  formatDetection: {
    telephone: false,
  },
  openGraph: {
    type: 'website',
    title: APP_TITLE,
    description: APP_DESCRIPTION,
    siteName: APP_TITLE,
    url: "도메인이름",
    images: [
      {
        url: '/app-1240.png',
        width: 1240,
        height: 620,
        alt: APP_TITLE,
      },
    ],
  },
  twitter: {
    title: APP_TITLE,
    description: APP_DESCRIPTION,
    card: 'summary',
    images: [
      {
        url: '/app-1240.png',
        width: 1240,
        height: 620,
        alt: APP_TITLE,
      },
    ],
  },
};

```

위 코드는 SEO를 포함한 내용이다.

반드시 필요한것은 manifest.json 파일을 추가해주는 것과 viewport에서 themeColor를 설정해주어야 한다.



{% embed url="https://ducanh-next-pwa.vercel.app/docs/next-pwa/getting-started" %}
