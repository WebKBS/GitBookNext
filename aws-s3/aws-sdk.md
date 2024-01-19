# aws sdk

{% embed url="https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/client/s3/" %}

위 사이트에서 npm, yarn, pnpm 등 본인이 사용하는 것에 맞춰 복사해서 사용한다.

필자는 npm을 사용하므로 npm 설치

```bash
npm install @aws-sdk/client-s3
```



## actions.ts

actions.ts 파일을 아래와 같이 변경한다.

```tsx
'use server';

import { PutObjectCommand, S3Client } from '@aws-sdk/client-s3';
import { revalidatePath } from 'next/cache';

// S3 클라이언트 생성
const s3Client = new S3Client({
  region: process.env.AWS_S3_REGION,
  credentials: {
    accessKeyId: process.env.AWS_S3_ACCESS_KEY_ID,
    secretAccessKey: process.env.AWS_S3_SECRET_ACCESS_KEY,
  },
} as any);
```

action은 서버에서 사용해야 하므로 반드시 'use server'를 최상단에 넣어주자.

그리고 위와 같이 s3Client를 만들어준다. 각 process.env의 값은 이전에 설정한 대로 .env파일에서 가져와서 사용한다.



### UploadFile 함수

클라이언트에서 useFormState를 사용해서 서버로 보내면 Upload 파일 데이터를 받아줄 로직이다.

```tsx
// 파일 업로드 함수
export async function uploadFile(prevState: any, formData: FormData) {
  // await new Promise((resolve) => setTimeout(resolve, 3000)); // 3초 딜레이 테스트

  try {
    console.log(formData.get('file')); // 데이터 확인

    const file = formData.get('file') as File;

    // 파일이 없으면 리턴한다.
    if (file.size === 0){
      return {
        status: 'error',
        message: '파일을 선택해주세요.',
      };
    } else if (file.type !== 'image/png' && file.type !== 'image/jpeg') {
      // 데이터에 대한 조건문 추가 이미지가 png 또는 jpeg파일이 아니면 리턴한다.
      return {
        status: 'error',
        message: '파일의 형식은 png, jpeg만 가능합니다.',
      };
    } else if (file.size > 1024 * 1024 * 5) {
    // 5mb이상 파일이면 리턴
      return {
        status: 'error',
        message: '파일의 크기는 5MB를 넘을 수 없습니다.',
      };
    }

    const buffer = Buffer.from(await file.arrayBuffer()); // 파일을 버퍼로 변환

    const url = await uploadFileToS3(buffer, file.name, file.type); // 이건 이후에 추가될 예정

    revalidatePath('/'); // 파일 업로드 성공하면 캐시를 삭제하고 재요청한다. * 반드시 필요

    return {
      status: 'success',
      message: '파일 업로드를 성공했습니다.',
      imageUrl: url, // 업로드된 미리보기 이미지를 내보낸다.
    };
  } catch (error) {
    return {
      status: 'error',
      message: '파일 업로드를 실패했습니다.',
    };
  }
}
```

서버에서 데이터를 받고 조건문의 통해 파일을 검증한후,

```typescript
const buffer = Buffer.from(await file.arrayBuffer()); // 파일을 버퍼로 변환
```

버퍼로 변환을 해준다. 이후 함수에 uploadFileToS3 함수에 추가해준다.



### uploadFileToS3

s3에 업로드해줄 함수를 만들어준다.

```typescript
// S3에 파일 업로드 함수
async function uploadFileToS3(
  file: Buffer, // 버퍼로 변환된 파일을 받는다. 
  fileName: string, // 파일이름을 받는다. - 업로드시 파일 이름설정
  fileType: string // 업로드할 파일 타입
) {
  const fileBuffer = file;
  const params = {
    Bucket: process.env.AWS_S3_BUCKET_NAME, // 버켓 이름 설정
    Key: `${fileName}`, // 만약 folder를 설정하려면, `폴더이름/${fileName}`
    // 만약 파일 중복을 피하려면 `${Date.now()}_${fileName}` Date now + 파일 이름
    Body: fileBuffer,
    ContentType: fileType,
  };

  console.log(fileType);

  const command = new PutObjectCommand(params);// PutObjectCommand 생성후 데이터전달

  try {
    const response = await s3Client.send(command); // s3client에 요청
    console.log('데이터 업로드 성공', response);

    // url은 업로드 성공 후 클라이언트로 내보낼 url을 설정한다.
    const url = `https://${process.env.AWS_S3_BUCKET_NAME}.s3.${process.env.AWS_S3_REGION}.amazonaws.com/${fileName}`;
  
    console.log('url', url);

    return url;
  } catch (error) {
    console.log('데이터 업로드 실패', error);
  }
}
```

위 함수에서 s3client에 파일 데이터를 보낼 함수 로직을 만들고

s3에 파일 업로드 후 클라이언트로 내보낼 url을 리턴해준다.

uploadFileToS3 함수는 uploadfile 함수에서 사용된다.

로직 끝.



{% hint style="warning" %}
만약 한글 파일 업로드시 utf 인코딩이 깨져 인코딩 변환도 필요하다.

인코딩 변환 함수를 만들어서 params에   Key: \`${fileName}\`,  여기 fileName을 감싸주면 된다.

인코딩 방법을 택하던, 아니면 영어나 date에 따른 각 상황에 맞게 파일이름을 변경해서 저장하면 된다.
{% endhint %}

중간의 조건문이나 로직은 얼마든지 변경될수 있으니 참고만하고, s3에 대해서는 여기까지.



## 전체 코드

```typescript
'use server';

import { PutObjectCommand, S3Client } from '@aws-sdk/client-s3';
import { revalidatePath } from 'next/cache';

// S3 클라이언트 생성
const s3Client = new S3Client({
  region: process.env.AWS_S3_REGION,
  credentials: {
    accessKeyId: process.env.AWS_S3_ACCESS_KEY_ID,
    secretAccessKey: process.env.AWS_S3_SECRET_ACCESS_KEY,
  },
} as any);

// 파일 이름을 UTF-8로 인코딩하는 함수 추가
function encodeFileName(fileName: string) {
  return encodeURIComponent(fileName);
}

// S3에 파일 업로드 함수
async function uploadFileToS3(
  file: Buffer,
  fileName: string,
  fileType: string
) {
  const fileBuffer = file;
  const params = {
    Bucket: process.env.AWS_S3_BUCKET_NAME,
    Key: `${encodeFileName(fileName)}`, // 만약 folder를 설정하려면, `폴더이름/${fileName}`
    // 만약 파일 중복을 피하려면 `${Date.now()}_${fileName}` Date now + 파일 이름
    Body: fileBuffer,
    ContentType: fileType,
  };

  console.log(fileType);

  const command = new PutObjectCommand(params);

  try {
    const response = await s3Client.send(command);
    console.log('데이터 업로드 성공', response);

    const url = `https://${process.env.AWS_S3_BUCKET_NAME}.s3.${process.env.AWS_S3_REGION}.amazonaws.com/${fileName}`;

    console.log('url', url);

    return url;
  } catch (error) {
    console.log('데이터 업로드 실패', error);
  }
}

// 파일 업로드 함수
export async function uploadFile(prevState: any, formData: FormData) {
  // await new Promise((resolve) => setTimeout(resolve, 3000)); // 3초 딜레이 테스트

  try {
    console.log(formData.get('file'));

    const file = formData.get('file') as File;

    if (file.size === 0) {
      return {
        status: 'error',
        message: '파일을 선택해주세요.',
      };
    } else if (file.type !== 'image/png' && file.type !== 'image/jpeg') {
      return {
        status: 'error',
        message: '파일의 형식은 png, jpeg만 가능합니다.',
      };
    } else if (file.size > 1024 * 1024 * 5) {
      return {
        status: 'error',
        message: '파일의 크기는 5MB를 넘을 수 없습니다.',
      };
    }

    const buffer = Buffer.from(await file.arrayBuffer()); // 파일을 버퍼로 변환

    const url = await uploadFileToS3(buffer, file.name, file.type);

    revalidatePath('/');

    return {
      status: 'success',
      message: '파일 업로드를 성공했습니다.',
      imageUrl: url,
    };
  } catch (error) {
    return {
      status: 'error',
      message: '파일 업로드를 실패했습니다.',
    };
  }
}
```
