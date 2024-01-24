# Google Analytics

## 구글 애널리틱스 추가하기.

{% embed url="https://analytics.google.com/analytics/web" %}

위 사이트에서 구글 애널리틱스를 가입한다.



next js 전용 서드파티를 설치한다.

```bash
npm install @next/third-parties
```



아래코드 처럼 layout tsx 파일에 추가해준다.

```tsx
import { GoogleAnalytics } from '@next/third-parties/google'
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <body>{children}</body>
      <GoogleAnalytics gaId="G-XYZ" /> // 바디 아래 추가. gaId는 구글에서 받은것을 추가.
    </html>
  )
}
```

gaId는 애널리틱스 가입 후 google에서 제공해주는 Id로 입력하면된다.

아이디는 G 로시작하는 ga 아이디가 있다. 그 아이디를 추가해주면 된다.



끝. 매우 간단하다.





공식문서 참고

{% embed url="https://nextjs.org/docs/messages/next-script-for-ga" %}
