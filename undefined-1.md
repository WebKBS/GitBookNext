---
description: create-next-app을 제외하고 시작
---

# 설치

#### 1. 프로젝트 폴더 생성

#### 2. package.json 파일 생성후 입력

```json
{
  "name": "next-reviews",
  "private": true,
}
```

#### 3. npm install로 next 와 react, react-dom 설치

```bash
npm install next react react-dom
```

위 처럼 설치하고 나면 package.json 파일 확인

```json
{
  "name": "next-reviews",
  "private": true,
  "dependencies": {
    "next": "^13.4.19",
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  }
}
```

버전과 설치된 목록을 확인한다. node\_modules 폴더를 보면 react와 next가 설치된것을 확인할 수 있다.

