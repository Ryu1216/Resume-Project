# HTML, CSS를 활용한 이력서 배포 자동화 프로젝트

## 프로젝트 개요
이 프로젝트는 HTML과 CSS로 구축된 반응형 이력서 웹사이트로, GitHub Actions를 사용하여 AWS S3에 자동 배포되는 특징을 가지고 있습니다. 라이브 사이트는 [http://resumazebucket.s3-website.ap-northeast-2.amazonaws.com/](http://resumazebucket.s3-website.ap-northeast-2.amazonaws.com/)에서 확인할 수 있습니다.

![이력서 웹사이트 미리보기](preview.png)

## 사용된 기술
- HTML5
- CSS3 (반응형 디자인)
- GitHub Actions (CI/CD)
- AWS S3 (호스팅)
- AWS CloudFront (콘텐츠 전송)

## 프로젝트 구조
```
.
├── index.html              # 메인 HTML 이력서 파일
├── style.css               # CSS 스타일링
├── profile.jpg             # 헤더에 사용되는 프로필 이미지
├── Profile Picture2.jpg    # 블러 효과가 적용된 배경 이미지
├── README.md               # 프로젝트 문서
└── .github/
    └── workflows/
        └── deploy.yml      # 자동 배포를 위한 GitHub Action
```

## 주요 특징

### 1. 세련된 헤더 섹션
헤더는 그라데이션 배경과 블러 처리된 프로필 이미지 오버레이를 통해 깊이감을 생성합니다:

```css
.header-section {
  background-color: #13111C;
  background: radial-gradient(circle at 30% 30%, #2D2B38 0%, #13111C 70%);
  color: #ffffff;
  position: relative;
  height: 400px;
  overflow: hidden;
}

.header-section::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: url('Profile Picture2.jpg');
  background-size: cover;
  background-position: 50% 40%;
  filter: blur(15px);
  z-index: 1;
}
```

### 2. 반응형 2열 레이아웃
이력서 내용은 다양한 화면 크기에 적응하는 2열 레이아웃으로 구성되어 있습니다:

```css
.container {
  display: flex;
  background-color: #fff;
  color: #000;
}

.left-column {
  width: 300px;
  background-color: #ffffff;
  padding: 40px;
}

.right-column {
  flex: 1;
  padding: 40px;
  background-color: #ffffff;
}
```

### 3. 모바일 반응성
미디어 쿼리를 사용하여 작은 화면에서도 디자인이 자연스럽게 적응합니다:

```css
@media (max-width: 768px) {
  .container {
    flex-direction: column;
  }
  
  .left-column, .right-column {
    width: 100%;
    padding: 20px;
  }
}

@media (max-width: 480px) {
  .header-section {
    height: auto;
    min-height: 300px;
  }
  
  .header-info h1 {
    font-size: 2.5rem;
  }
}
```

### 4. 타이포그래피 및 스타일링
이력서는 Inter 폰트 패밀리를 사용하여 깔끔한 타이포그래피와 세심하게 설계된 간격 및 텍스트 스타일을 특징으로 합니다:

```css
body {
  font-family: 'Inter', sans-serif;
  line-height: 1.6;
  max-width: 210mm;
  margin: 0 auto;
  padding: 20px;
}

.section-title {
  font-size: 1.1rem;
  font-weight: 700;
  margin-bottom: 20px;
  color: #000;
  text-transform: uppercase;
}
```

## 자동화된 배포

이 프로젝트는 GitHub Actions를 사용하여 메인 브랜치에 변경 사항이 푸시될 때마다 AWS S3에 자동으로 배포합니다:

```yml
name: Deploy to S3

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-22.04

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v3
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ${{ secrets.AWS_REGION }}

      - name: Sync files to S3
        run: |
          aws s3 sync . s3://${{ secrets.S3_BUCKET_NAME }} --delete
```

이 워크플로우는:
1. 메인 브랜치로의 푸시에 의해 트리거됩니다
2. 저장소 시크릿에서 AWS 자격 증명을 설정합니다
3. 모든 파일을 구성된 S3 버킷에 동기화하고 `--delete` 플래그를 사용하여 오래된 파일을 제거합니다

## 이 프로젝트 사용 방법

1. 이 저장소를 Star하고 fork 합니다
2. 개인 정보로 index.html을 업데이트합니다
3. 프로필 이미지를 자신의 것으로 교체합니다
4. style.css를 선호하는 색상 배합에 맞게 사용자 정의합니다
5. AWS S3 호스팅을 설정하고 GitHub 저장소 시크릿에 자격 증명을 추가합니다
6. 메인 브랜치에 푸시하여 배포를 트리거합니다
