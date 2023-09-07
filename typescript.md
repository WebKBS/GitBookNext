---
description: Next js에서 타입스크립트를 설치하는 방법.
---

# Typescript

이 방법은 매우 간단할 수 있다.

next-create-app을 사용해서 프로젝트 파일 생성시

typescript 설치하시겠습니까? 에서 yes를 누르면 설치 당시 함께 설치 되지만

만약, typescript를 설치하지 않았을때 방법이다.



우선, 모든 jsx 파일을 tsx파일로 교체한다.

그리고 npm run dev를 실행시킨다.

그럼 자동으로 typescript 파일이 설치된다.&#x20;

파일을 확인해보면 tsconfig.json 파일이 설치된것을 볼 수 있다.

{% hint style="info" %}
만약 서버가 실행중이라면 tsx파일로 수정시 자동으로 설치가 된다.
{% endhint %}



끝이다.

tsconfig.json에서 원하는 설정을 셋팅하면 된다.
