---
description: .env 및 git ignore 셋팅
---

# 환경변수 설정

{% content-ref url="https://app.gitbook.com/s/DJRNo6R7LcuStcxdHN7o/" %}
[AWS S3](https://app.gitbook.com/s/DJRNo6R7LcuStcxdHN7o/)
{% endcontent-ref %}

위  aws s3 설정에서 설정한 것을 토대로 .env파일에 환경 변수를 설정한다.

{% hint style="danger" %}
깃허브에 env파일이 올라가면 안되니 미리 gitignore 세팅부터한다.
{% endhint %}

next js파일을 설치하면 .gitignore 파일이 이미 있을 것이다. gitignore파일에 .env 를 추가해준다.

```textile
AWS_S3_REGION=
AWS_S3_BUCKET_NAME=
AWS_S3_ACCESS_KEY_ID=
AWS_S3_SECRET_ACCESS_KEY=
```

위처럼 리전, 액세스키, 시크릿키, 버킷 이름을 설정한다.



AWS\_S3\_REGION=

리전의 이름이다. S3 대시보드에 들어가면 버킷 이름 옆 리전이 있다.

<figure><img src="../.gitbook/assets/스크린샷 2024-01-18 시간 22.45.05.png" alt=""><figcaption></figcaption></figure>

색상으로 표시 된 부분만 입력하면 된다.



AWS\_S3\_BUCKET\_NAME=

버킷 이름은 위 이미지에서 리전 옆 버킷의 이름을 입력하면 된다. (필자같은 경우는 next-s3-test-1)



AWS\_S3\_ACCESS\_KEY\_ID=

AWS\_S3\_SECRET\_ACCESS\_KEY=&#x20;

위 두 키는 아래 페이지에서 설정한 IAM 액세스 키와 시크릿 키를 추가하면된다.

{% content-ref url="https://app.gitbook.com/s/DJRNo6R7LcuStcxdHN7o/aws-iam" %}
[AWS IAM](https://app.gitbook.com/s/DJRNo6R7LcuStcxdHN7o/aws-iam)
{% endcontent-ref %}
