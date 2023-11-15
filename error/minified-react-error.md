---
description: >-
  Uncaught Error: Minified React error #418; visit
  https://reactjs.org/docs/error-decoder.html?invariant=418 for the full message
  or use the non-minified dev environment for full errors and additional..
---

# Minified React error

Minified React error의 종류는 다양하다.

에러#코드418, #425 등..



개발시 dev 모드와, build후 start 모드에서 발견되지 않았던 에러인데,

vercel에 배포 하고 난 후&#x20;

Uncaught Error: Minified React error #418; visit https://reactjs.org/docs/error-decoder.html?invariant=418 for the full message or use the non-minified dev environment for full errors and additional..

이와 같은 에러가 콘솔에 나타났다.

처음에 정말 원인을 알수없어서 엄청 헤맸는데, 구글링 결과&#x20;



new Date() 브라우저 내장 메서드를 사용할때 나타난다는 에러를 많이 봤다.

GPT도 이번 에러는 해결해주지않았다.

이 에러를 해결하려면 new Date가 되는 부분을 useEffect로 감싸주고, state를 통한 데이터를 사용하면 된다.

예시)

```tsx
const [writeDate, setWriteDate] = useState<string>();

useEffect(() => {
  setWriteDate(moment(date).format('MMM. D, YYYY - HH:mm'));
}, [date]);
```
