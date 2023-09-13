---
description: head태그 안에 들어가는 메타데이터를 설정해보자.
---

# MetaData

웹 개발을 하다보면 SEO ( _search engine optimization_ ) 검색엔진 최적화를 해야한다.

그 중에 한 방법이 메타데이터를 설정하는 것이다.&#x20;

NEXT JS에서 메타데이터를 입력해보자.



사실 간단하게 입력할 수 있다.

```jsx
export const metadata = {
    title: ".....",
    description: "....."
}
```

이렇게 page.jsx에 상단 함수위에 입력하면 메타데이터가 설정된다.



만약 전체 페이지 글로벌로 설정하고싶다면 app 디렉터리에 layout.jsx에 RootLayout 함수 위쪽에 입력하면 전체 글로벌 페이지에 메타데이터가 설정된다.



글로벌페이지에 입력하더라도 만약 개별 페이지에 또 다른 메타데이터를 추가하면\
글로벌 페이지 메타데이터를 덮어쓰고 새로운 메타데이터가 추가된다.

```jsx
// app/layout.jsx
export const metadata = {
    title: ".....",
    description: "....."
}


// app/about/page.jsx
export const metadata = {
    title: "<여기 메타데이터는 변경된다.>"
    description: "....."
}
```



### 만약 글로벌 메타데이터 문자열을 고스란히 가지되 다른 페이지에서도 사용하려면?

이럴때가있다. 홈페이지 이름을 그대로 넣으면서 다른 페이지에 타이틀을 변경하고 싶을때가 있다.

예시)  About - 홈페이지이름,\
&#x20;         Contact - 홈페이지이름  등..

"홈페이지이름" 은 글로벌 페이지에서 가져오되 페이지마다 해당하는 페이지의 이름을 변경하고 싶을때 사용하는 방법이다.

app/layout.tsx 페이지에 입력한다.

```jsx
// app/layout.tsx
export const metadata = {
    title: {
        default: "기본 설정 이름",
        template: "%s - 글로벌설정 이름"
    },
}
```

title을 객체로 default와 template를 추가한다.

default는 말그대로 기본 이름을 설정하는 것이고.

template는 다른페이지에 metadata를 설정하면 입력될 값이다.

"%s" 에 다른 페이지에서 메타데이터 입력한 값이 추가된다. 이후 문자열은 원하는데로 자유롭게 입력가능하다.
