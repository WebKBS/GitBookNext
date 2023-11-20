---
description: 설치 방법
---

# 설치

```bash
npm install next-intl
```



next-intl 패키지를 설치하고 아래와 같이 파일구조를 맞춰서 파일을 생성한다.

```
├── messages (1)
│   ├── en.json
│   └── ...
├── next.config.js (2)
└── src
    ├── i18n.ts (3)
    ├── middleware.ts (4)
    └── app
        └── [locale]
            ├── layout.tsx (5)
            └── page.tsx (6)
```
