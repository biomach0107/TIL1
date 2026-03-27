# 🚀 Medi Log AWS 배포 가이드

## 📋 배포 단계별 안내

### 1️⃣ AWS Amplify 배포 (추천 - 가장 간단)

#### 단계 1: AWS 콘솔 접속
1. [AWS Amplify 콘솔](https://console.aws.amazon.com/amplify/) 접속
2. "Create new app" 클릭
3. "Host web app" 선택

#### 단계 2: GitHub 연결
1. "GitHub" 선택
2. GitHub 계정 연결 승인
3. Repository: `biomach0107/TIL1` 선택
4. Branch: `main` 선택

#### 단계 3: 앱 설정
- App name: `medi-log-app`
- Environment: `production`
- Build settings: 자동 감지됨 (amplify.yml 사용)

#### 단계 4: 배포 완료
- 배포 시간: 약 2-3분
- 생성되는 URL 형식: `https://main.d[랜덤문자].amplifyapp.com`

### 2️⃣ AWS S3 정적 웹사이트 배포

#### AWS CLI 명령어:
```bash
# 1. S3 버킷 생성
aws s3 mb s3://medi-log-app-$(date +%s)

# 2. 파일 업로드
aws s3 sync . s3://medi-log-app-$(date +%s) --exclude "*.md" --exclude "server.*"

# 3. 웹사이트 호스팅 활성화
aws s3 website s3://medi-log-app-$(date +%s) --index-document index.html

# 4. 퍼블릭 액세스 허용
aws s3api put-bucket-policy --bucket medi-log-app-$(date +%s) --policy file://bucket-policy.json
```

### 3️⃣ 예상 접근 링크들

#### Amplify 배포 후 생성되는 링크:
- **메인 URL**: `https://main.d[12자리랜덤].amplifyapp.com`
- **브랜치별 URL**: `https://[브랜치명].d[12자리랜덤].amplifyapp.com`

#### S3 정적 웹사이트 링크:
- **기본 URL**: `http://medi-log-app-[타임스탬프].s3-website-us-east-1.amazonaws.com`
- **CloudFront URL**: `https://d[13자리랜덤].cloudfront.net`

## 🔧 배포 후 확인사항

### ✅ 기능 테스트 체크리스트:
- [ ] 메인 페이지 로딩
- [ ] 회원가입/로그인 기능
- [ ] 월별 트렌드 차트 표시
- [ ] 약물 정보 모달 작동
- [ ] 처방전 분석 기능
- [ ] 반응형 디자인 (모바일)
- [ ] 접근성 기능 (큰 글씨, 음성 읽기)

### 📱 모바일 최적화 확인:
- [ ] 터치 인터페이스 작동
- [ ] 화면 크기별 레이아웃
- [ ] 스크롤 및 네비게이션

## 🌐 도메인 연결 (선택사항)

### Route 53 설정:
1. 도메인 구매 또는 기존 도메인 사용
2. Hosted Zone 생성
3. A 레코드 또는 CNAME 레코드 추가
4. SSL 인증서 설정 (AWS Certificate Manager)

### 예상 도메인:
- `https://medilog.yourdomain.com`
- `https://app.medilog.com`

## 💰 예상 비용

### AWS Amplify:
- **호스팅**: 15GB/월 무료, 이후 $0.15/GB
- **빌드**: 1000분/월 무료, 이후 $0.01/분
- **예상 월 비용**: $0-3

### S3 + CloudFront:
- **S3 스토리지**: $0.023/GB/월
- **CloudFront**: 1TB/월 무료, 이후 $0.085/GB
- **예상 월 비용**: $1-5

## 🔒 보안 및 성능

### HTTPS 자동 적용:
- Amplify: 자동 SSL 인증서
- CloudFront: AWS Certificate Manager 연동

### 성능 최적화:
- CDN을 통한 전 세계 배포
- 자동 압축 및 캐싱
- 모바일 최적화

## 📊 모니터링 및 분석

### AWS CloudWatch:
- 트래픽 모니터링
- 오류 로그 추적
- 성능 메트릭

### 사용자 분석:
- Google Analytics 연동 가능
- AWS Pinpoint 사용자 행동 분석

---

## 🎯 빠른 배포 링크

**지금 바로 배포하려면:**

1. **AWS Amplify 콘솔**: https://console.aws.amazon.com/amplify/
2. **GitHub 레포지토리**: https://github.com/biomach0107/TIL1
3. **배포 후 예상 URL**: `https://main.d[랜덤].amplifyapp.com`

**배포 완료 시간**: 약 3-5분 ⏱️