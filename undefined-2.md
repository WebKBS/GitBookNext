---
description: Next js 파일을 생성해보자.
---

# 파일 생성

#### 1. 최상위 root 폴더에 app 폴더를 만든다.

```textile
Project
    ㄴ .next
    ㄴ app
    ㄴ node_modules
```

#### 2. app폴더 안에 layout.jsx 파일을 만든다.

확장자명을 js로 해도 상관 없으나, 그래도 조금더 멋있어보이는 jsx확장자를 사용하자.\
사실 intellisense(자동완성) 기능에도 차이점이 있다.

{% hint style="info" %}
App Router 기준이다. 반드시 app의 jsx 파일이름은 layout.jsx(또는 js) 파일로 만들어야한다.

정해져있다.
{% endhint %}

#### 3. layout.jsx 파일에 첫 코드를 적어본다.

```jsx
export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  );
}
```

app폴더의 가장 첫 layout.jsx파일은 Next js의 가장 최상위 파일이다.

HTML 엘리먼트와 body엘리먼트를 작성해보자.

RootLayout에 props중 children을 받아서 body에 넣어주면 준비 끝.

#### 4. 서버 실행.

페이지를 하나 만들었으니 서버를 실행 해보자.

<img src=".gitbook/assets/404.png" alt="" data-size="original">

이와 같이 404가 나타는 것을 볼 수 있다. 왜냐하면 layout은 만들었지만 실제 컨텐츠가 들어갈 page가 없기때문이다.

브라우저 화면에 컨텐츠를 나타내기 위해서는 반드시 page.jsx가 필요하다.

{% hint style="info" %}
page.js나 page.jsx는 약간의 차이만 있을뿐 동일하다. 앞으로 통일감을 주기위해 jsx만 사용하겠습니다.
{% endhint %}

#### 5. page.jsx 파일을 만들고 함수를 만든다.

```jsx
export default function HomePage() {
  return <h1>Hello World</h1>;
}

// 함수이름은 무엇을 사용해도 상관없다.
```

또는

```jsx
function HomePage() {
  return <h1>Hello World</h1>;
}

export default HomePage;
```

{% hint style="warning" %}
함수 내보내기할때는 무조건 export default를 해야한다.

간혹 리액트 같은 경우 export만 사용할때도 있는데, next에서는 반드시 export default를 사용해야한다.

규칙이다.
{% endhint %}

#### 6. 터미널에서 npx next dev를 실행시키고 다시 확인해보자.

Hello world나 본인이 작성한 문자열을 확인하면 성공!



#### 7. 이제 package.json 파일을 열어 scripts를 작성해보자.

create-next-app 처럼 npm을 시작할때 npm run dev를 설정하는 방법이다.

아래 코드처럼 "scripts" 부분을 작성한다.

```json
{
  "name": "next-reviews",
  "private": true,
  "scripts": {
    "dev": "next dev"
  },
  "dependencies": {
    "next": "^13.4.19",
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  }
}
```

이제 npm run dev를 터미널에서 next 서버를 실행 시킬수 있다.\
\*\* "dev"는 개발모드
