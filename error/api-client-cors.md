---
description: Next js client cors 해결하기
---

# API 호출 Client CORS

외부 api 요청시 서버에서 굳이 호출이 필요하지 않다면

클라이언트사이드에서 api를 호출해야할때가 있다.



클라이언트에서 api 호출시 요청하는 서버가 header설정에서 모두 허용하지 않았다면

cors 에러 문제가 생기는데 next js에서 이를 해결하기 위해 가상 proxy를 만드는 방법이다.



### 아래는 클라이언트(프론트)에서 문제가 생긴다.

```tsx
useEffect(() => {
  const interval = setInterval(() => {
    axios
      .get('(해당주소)/api/v4/public/ticker') // 해당 주소는 https://사이트...
      .then((res) => {
        setCurrentPrice(res.data['UNI_USDT'].last_price);

        console.log(res.data['UNI_USDT'].last_price);
      })
      .catch((err) => {
        console.log(err);
      });
  }, 3000);

  return () => clearInterval(interval);
}, []);
```



클라이언트에서 cors문제가 생길 때 next js cofig 설정으로 proxy를 만들어 이를 해결할수 있다.

```javascript
// next.config.js

/** @type {import('next').NextConfig} */
const nextConfig = {
  async rewrites() {
    return [
      {
        source: '/:path*',
        destination: '(주소입력)/:path*',
      },
    ];
  },
};

module.exports = nextConfig;


```

(주소입력) 부분에 사용할 api주소를 넣는다.

클라이언트에서 사용방법은 url에 path이름만 사용한다.

(next.config.js 에서)  이미 url을 등록했기때문

```tsx
useEffect(() => {
  const interval = setInterval(() => {
    axios
      .get('/api/v4/public/ticker')
      .then((res) => {
        setCurrentPrice(res.data['UNI_USDT'].last_price);

        console.log(res.data['UNI_USDT'].last_price);
      })
      .catch((err) => {
        console.log(err);
      });
  }, 3000);

  return () => clearInterval(interval);
}, []);
```



위처럼 사용하면 해당 cors를 해결할 수 있다.



참고 공식문서 URL

[https://nextjs.org/docs/app/api-reference/next-config-js/rewrites](https://nextjs.org/docs/app/api-reference/next-config-js/rewrites)

