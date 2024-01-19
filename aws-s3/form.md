# Form

업로드에 대한 폼을 만든다.

upload/(form)/form.tsx

{% hint style="info" %}
next js 에서 () 소괄호를 사용해서 폴더이름을 만드는것은 그룹을 짓는 의미이다.
{% endhint %}

```tsx
export default function UploadForm() {
  return (
    <div className="form-wrapper">
      <form action="">
        <input type="file" name="file" id="file" accept="image/*" />
        <button type="submit" className="submit-button">
          파일 업로드
        </button>
      </form>
    </div>
  );
}
```



actions 파일을 만든다.

```typescript
'use server';

import { revalidatePath } from 'next/cache';

export async function uploadFile(prevState: any, formData: FormData) {
  try {

    // 차우 입력될 파일
    console.log(formData);

    revalidatePath('/');

    return {
      status: 'success',
      message: '파일 업로드를 성공했습니다.',
    };
  } catch (error) {
    return {
      status: 'error',
      message: '파일 업로드를 실패했습니다.',
    };
  }
}
```



이제 UploadForm 컴포넌트를 다음과 같이 만든다.

```tsx
'use client';
import { useFormState, useFormStatus } from 'react-dom';
import { uploadFile } from './actions';

const initialState = { message: '', status: '' };

export default function UploadForm() {
  const [state, formAction] = useFormState(uploadFile, initialState);
  const { pending } = useFormStatus();

  return (
    <div className="form-wrapper">
      <form action={formAction}>
        <input type="file" name="file" id="file" accept="image/*" />
        <button type="submit" className="submit-button" disabled={pending}>
          {pending ? '업로드 중...' : '파일 업로드'}
        </button>
      </form>
      {state?.status && (
        <div className={`state-message ${state?.status}`}>{state?.message}</div>
      )}
    </div>
  );
}
```

{% hint style="info" %}
useFormState와 useFormStatus를 사용하면 상단에 'use client' 를 사용해야한다.
{% endhint %}

{% hint style="info" %}
state-message의 className의 부분은  데이터를 요청하고 받아오는 state의 문자열을 사용해서

css속성을 변경하기 위해 추가
{% endhint %}



useFormStatus의 pending을 사용하기 위해  button을 따로 컴포넌트로 분리시켜준다.

{% hint style="warning" %}
pending을 form 내부에서 사용하면 동작하지 않는다.

이유는 react 공식 홈페이지 Pitfall 참조

#### [https://react.dev/reference/react-dom/hooks/useFormStatus#usage](https://react.dev/reference/react-dom/hooks/useFormStatus#usage)
{% endhint %}



Button.tsx

```tsx
'use client';
import { useFormStatus } from 'react-dom';

export default function Button() {
  const { pending } = useFormStatus();

  return (
    <button type="submit" className="submit-button" disabled={pending}>
      {pending ? '업로드 중...' : '파일 업로드'}
    </button>
  );
}
```



UploadForm을 변경해준다. 약간 수정사항이 있다.

이미지를 업로드한 후 즉시 불러오기위해 img 태그를 추가하였다.

```tsx
'use client';
import { useFormState } from 'react-dom';
import Button from './Button';
import { uploadFile } from './actions';

const initialState = { message: '', status: '', imageUrl: '' };

export default function UploadForm() {
  const [state, formAction] = useFormState(uploadFile, initialState);

  console.log('state', state);

  return (
    <div className="form-wrapper">
      <form action={formAction}>
        <input type="file" name="file" id="file" accept="image/*" />
        <Button />
      </form>
      {state?.status && (
        <div className={`state-message ${state?.status}`}>{state?.message}</div>
      )}
      {state?.imageUrl && (
        <div className="form-wrapper">
          <img src={state?.imageUrl} alt="업로드 이미지" />
        </div>
      )}
    </div>
  );
}
```



참조

{% embed url="https://nextjs.org/docs/app/building-your-application/data-fetching/server-actions-and-mutations" %}

이제 aws sdk 사용해서 actions.ts파일을 완성해보자.
