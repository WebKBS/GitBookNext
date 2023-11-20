---
description: meta태그에 입력될 next js metadata에서 다국어 사용하기
---

# MeataData에서 사용하기

설치 및 세팅에서 사용한 코드 이외 새로운 코드를 간단하게 만들었다.

app/\[locale]/layout.tsx

```tsx
import { Inter } from 'next/font/google';
import './globals.css';
import { notFound } from 'next/navigation';

const locales = ['en', 'ru', 'ko'];
const inter = Inter({ subsets: ['latin'] });

export default function LocaleLayout({
  children,
  params: { locale },
}: {
  children: React.ReactNode;
  params: { locale: string };
}) {
  if (!locales.includes(locale as any)) notFound();

  return (
    <html lang={locale}>
      <body className={inter.className}>{children}</body>
    </html>
  );
}
```

layout.tsx에서 metadata 변수를 제거하였다.

왜냐하면 page.tsx에서 generateMetadata 를 사용해 조건문을 사용해서 추가하기 위함이었다.



\
app/\[locale]/page.tsx

```tsx
import { getTranslations } from 'next-intl/server';
import { useTranslations } from 'next-intl';
import { Link } from '@/navigation';

export async function generateMetadata() {
  const t = await getTranslations('Home');

  return {
    title: t('meta-title'),
    description: t('meta-description')
  };
}

export default function Home({ params }) {
  const t = useTranslations('Home');
  return (
    <div>
      <h1>{t('title')}</h1>
    </div>
  );
}
```

개발 페이지마다 타이틀 및 description을 변경하기위해 generateMetadata 함수를 사용했다.

next-intl 공식홈페이지와 내용이 조금 다르긴한데.. 공식홈페이지에 있는namespaces를  사용하려고 했더니 되지 않아서 사용 가능한 방법을 하려고했다.

generateMetadata는 비동기 함수라서 useTranslations사용이 불가능하다.

getTranslations함수를 사용해서 json을 가져와야한다.

json파일에 meta-title, meta-description 이라고하는 Key를 만들어주고, 값을 넣어주었다.

아주 잘된다.



위처럼 사용하면 각 페이지에서 동적으로 metadata를 사용할 수 있다.

