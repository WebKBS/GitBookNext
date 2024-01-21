---
description: viewport 설정
---

# viewport

예시)

모바일에서 input 클릭시 확대되는 현상을 막기위해서 meta 태그 안의 viewport를 설정해야한다.

next js에서는 아래와 같이 객체를 만들어야한다.

```typescript
export const viewport = {
  with: 'device-width',
  initialScale: 1,
  maximumScale: 1,
  userScalable: 'no',
};
```



공식문서 참고

{% embed url="https://nextjs.org/docs/app/api-reference/functions/generate-viewport" %}
