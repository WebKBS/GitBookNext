---
description: next-intl 설정 완료후 각 페이지 번역을 위한 Link 페이지 이동이다.
---

# 라우팅 (Link)

Link버튼과 useRouter 또는 usePathname 등

번역 상태는 유지하되, 해당하는 Link 이동이나, 라우팅 변경시 번역해제 없이 그대로 사용할수 있도록 만드는 방법이다.

\
src 폴더에 navigation.ts 파일 생성.

```typescript
// src/navigation.ts

import { createSharedPathnamesNavigation } from 'next-intl/navigation';

export const locales = ['ko', 'en', 'ru'] as const;

export const { Link, redirect, usePathname, useRouter } =
  createSharedPathnamesNavigation({ locales });

```

위와 같이 파일을 세팅해준다. locales는 사용할 언어이다.



설치 및 세팅에서 만들었던 middleware.ts 파일의 설정을 확인해보고  변경하자.

<pre class="language-typescript"><code class="lang-typescript">// src/middleware.ts

<strong>import createMiddleware from 'next-intl/middleware';
</strong>import { locales } from './navigation';

export default createMiddleware({
  defaultLocale: 'ko',
  locales,
});

export const config = {
  matcher: ['/', '/(ko|en|ru)/:path*'],
};
</code></pre>

navigation ts에서 locales를 불러왔고 defaultLocale 언어 설정과 locales를 createMiddleware로 만들어준다.



### 중요한점!

navigation.ts 파일을 살펴보면

```typescript
export const { Link, redirect, usePathname, useRouter } =
  createSharedPathnamesNavigation({ locales });
```

Link, redirect, usePathname, useRouter

이와 같은 함수가 있는데.&#x20;

이건 기존 next의 Link, redirect, usePathname, useRouter와 사용을 대신하는 것이다! \*\* 매우중요.



그래서 위 함수들을 사용하기 위해서는 반드시 import할때 /navigation.ts 파일에서 가져와서 사용해야한다.

예시)

{% hint style="info" %}
```tsx
import { Link } from '@/navigation';
```
{% endhint %}

반드시 @/navigation에서 호출했는지 확인해야한다.



만약 기존 next Link 방식으로 불러온다면 절대 안된다!

번역된 페이지 이동이 불가능하게 된다.

```tsx
import Link from 'next/link'; // 사용 금지
```



이 처럼 Link 뿐만아니라 usePathname, useRouter등 navigation.ts파일에서 설정해서 설정한 함수들만&#x20;

import해서 사용해야한다.
