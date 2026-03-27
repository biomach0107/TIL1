# AWS 배포 가이드 - Medi Log 애플리케이션

## 🌐 AWS S3 + CloudFront 배포 (추천)

### 1단계: S3 버킷 생성
```bash
# AWS CLI 설치 후
aws s3 mb s3://medi-log-app-unique-name
```

### 2단계: 정적 웹사이트 호스팅 설정
```bash
# 웹사이트 호스팅 활성화
aws s3 website s3://medi-log-app-unique-name \
  --index-document index.html \
  --error-document index.html
```

### 3단계: 파일 업로드
```bash
# 모든 파일을 S3에 업로드
aws s3 sync . s3://medi-log-app-unique-name --delete
```

### 4단계: 퍼블릭 액세스 설정
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::medi-log-app-unique-name/*"
    }
  ]
}
```

### 5단계: CloudFront 배포 생성
- Origin Domain: S3 버킷 웹사이트 엔드포인트
- Default Root Object: index.html
- Error Pages: 404 → /index.html (SPA 라우팅용)

## 🖥️ AWS EC2 배포 (서버 필요시)

### 1단계: EC2 인스턴스 생성
- AMI: Amazon Linux 2
- Instance Type: t2.micro (프리티어)
- Security Group: HTTP(80), HTTPS(443), SSH(22) 허용

### 2단계: 서버 설정
```bash
# EC2 접속 후
sudo yum update -y
sudo yum install -y python3 git

# 애플리케이션 클론
git clone https://github.com/biomach0107/TIL1.git
cd TIL1

# 서버 실행
python3 server.py
```

### 3단계: 도메인 연결 (선택사항)
- Route 53에서 도메인 구매/설정
- A 레코드로 EC2 퍼블릭 IP 연결

## 🔧 AWS Amplify 배포 (가장 간단)

### 1단계: Amplify 콘솔 접속
1. AWS Amplify 콘솔 열기
2. "New app" → "Host web app" 선택
3. GitHub 연결 선택

### 2단계: 레포지토리 연결
- Repository: biomach0107/TIL1
- Branch: main
- Build settings: 자동 감지

### 3단계: 배포 설정
```yaml
version: 1
frontend:
  phases:
    build:
      commands:
        - echo "Static site - no build required"
  artifacts:
    baseDirectory: /
    files:
      - '**/*'
```

## 📱 접근 링크 생성

### S3 정적 웹사이트 URL 형식:
```
http://medi-log-app-unique-name.s3-website-us-east-1.amazonaws.com
```

### CloudFront URL 형식:
```
https://d1234567890abc.cloudfront.net
```

### Amplify URL 형식:
```
https://main.d1234567890abc.amplifyapp.com
```

### EC2 URL 형식:
```
http://ec2-xx-xx-xx-xx.compute-1.amazonaws.com:8080
```

## 💰 비용 예상

### S3 + CloudFront (추천)
- S3 스토리지: ~$0.023/GB/월
- CloudFront: 첫 1TB 무료, 이후 ~$0.085/GB
- **월 예상 비용: $1-5**

### EC2 t2.micro
- 프리티어: 12개월 무료
- 이후: ~$8.5/월

### Amplify
- 빌드 시간: 1000분/월 무료
- 호스팅: 15GB/월 무료
- **월 예상 비용: $0-2**

## 🔒 보안 설정

### HTTPS 설정
```bash
# Let's Encrypt SSL 인증서 (EC2용)
sudo yum install -y certbot
sudo certbot --nginx -d yourdomain.com
```

### 환경 변수 설정
```bash
# .env 파일 생성
echo "NODE_ENV=production" > .env
echo "PORT=80" >> .env
```

## 📊 모니터링 설정

### CloudWatch 로그 설정
```bash
# EC2에서 CloudWatch 에이전트 설치
wget https://s3.amazonaws.com/amazoncloudwatch-agent/amazon_linux/amd64/latest/amazon-cloudwatch-agent.rpm
sudo rpm -U ./amazon-cloudwatch-agent.rpm
```

## 🚀 자동 배포 설정

### GitHub Actions (CI/CD)
```yaml
name: Deploy to AWS
on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Deploy to S3
      run: |
        aws s3 sync . s3://medi-log-app-unique-name --delete
      env:
        AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
        AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
```

## 📞 지원 및 문의

배포 중 문제가 발생하면:
1. AWS 문서 확인: https://docs.aws.amazon.com
2. AWS 지원 센터 문의
3. GitHub Issues 등록

---

**추천 배포 방법**: AWS Amplify (가장 간단) 또는 S3 + CloudFront (가장 경제적)