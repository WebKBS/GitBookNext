---
description: 다국어 data를 화면에 나타내는 방법
---

# 사용방법

세팅을 완료했으면 이제 페이지에서 사용해보자.

사용방법은 제법 간단하다.

import

```tsx
import { useTranslations } from 'next-intl';
```

위와같이 useTranslations를 가져온다.



Home에서 사용하는 예시

```tsx
export default function Home() {
  const t = useTranslations('Home');
  
  return (
    <main className="pt-14 bg-zinc-900 min-h-screen">
      <section>
        <Image
          src="/background/home.jpg"
          fill
          className="pointer-events-none opacity-50 object-cover"
          alt={t('title')}
        />
        <div className="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-full py-16 px-6 break-keep text-white text-center bg-black bg-opacity-50">
          <h1 className="font-bold text-4xl leading-normal mb-6">
            {t('title')}
          </h1>
          <h2 className="max-w-4xl m-auto px-6 whitespace-pre-line">
            {t('description')}
          </h2>
        </div>
      </section>
    </main>
  );
}
```

const t 라는 변수에 useTranslations을 사용한다.

useTranslations의 인수는 각 언어별 JSON파일의 가장 첫번째 Key이다.



예시) 아래 부분에서 HOME에 해당하는 데이터를 변수에 저장하는 것이다. \
Navigation과 Introduction는 제외.

```json
{
  "Navigation": {
    "home": "HOME",
    "introduction": "회사소개",
    "services": "서비스",
    "infomation": "정보자료",
    "contact": "문의"
  },
  
  "Home": {
    "meta-title": "",
    "meta-description": "",
    "title": "",
    "description": ""
  },

  "Introduction": {
    "meta-title": "회사소개",
    "meta-description": "회사소개",
    "title": "회사소개"
  }
}
```



t 변수에 할당한 HOME을 사용하기 위해서는 두번째 depth를 t("")함수에 넣어야한다.

아래와 같이 사용한다.

```tsx
t('title')
// 또는
t('description')
```

t 함수의 인수로 Home의 두번째 depth의 key를 사용한다.

이렇게 사용하면 두번째 depth의 key에 해당하는 데이터 값을 출력한다.



Navigation, Home, Introduction 처럼 첫번째 key는 가장 큰 단위이며

데이터를 구조화 할때 첫번째 Key는 각 해당하는 페이지에서 사용하도록 구성하는것이 좋다.
