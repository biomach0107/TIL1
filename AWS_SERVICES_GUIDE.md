# 🔧 Medi Log 프로젝트 AWS 서비스 활용 가이드

## 📋 활용된 AWS 서비스 목록

### 1. 🌐 **AWS Amplify** (웹 애플리케이션 호스팅)
**용도**: 정적 웹 애플리케이션 배포 및 호스팅
**특징**: 
- GitHub 연동 자동 배포
- HTTPS 자동 적용
- 글로벌 CDN 제공
- 도메인 관리

#### 접근 방법:
```bash
# 1. AWS 콘솔 접근
https://console.aws.amazon.com/amplify/

# 2. AWS CLI 접근
aws amplify list-apps
aws amplify get-app --app-id [APP_ID]

# 3. 배포된 앱 URL
https://main.d[랜덤문자].amplifyapp.com
```

### 2. 🗄️ **Amazon S3** (정적 웹사이트 호스팅)
**용도**: HTML, CSS, JS 파일 저장 및 웹사이트 호스팅
**특징**:
- 99.999999999% 내구성
- 정적 웹사이트 호스팅 기능
- 버전 관리 지원

#### 접근 방법:
```bash
# 1. AWS 콘솔 접근
https://s3.console.aws.amazon.com/

# 2. AWS CLI 접근
aws s3 ls
aws s3 ls s3://medi-log-app-bucket/
aws s3 sync . s3://medi-log-app-bucket/

# 3. 웹사이트 URL
http://medi-log-app-bucket.s3-website-us-east-1.amazonaws.com
```

### 3. 🚀 **Amazon CloudFront** (CDN)
**용도**: 전 세계 사용자에게 빠른 콘텐츠 전송
**특징**:
- 글로벌 엣지 로케이션
- HTTPS 지원
- 캐싱 최적화

#### 접근 방법:
```bash
# 1. AWS 콘솔 접근
https://console.aws.amazon.com/cloudfront/

# 2. AWS CLI 접근
aws cloudfront list-distributions
aws cloudfront get-distribution --id [DISTRIBUTION_ID]

# 3. CDN URL
https://d[13자리랜덤].cloudfront.net
```

### 4. 🖥️ **Amazon EC2** (서버 호스팅)
**용도**: Python/Node.js 서버 실행 (동적 기능 필요시)
**특징**:
- 확장 가능한 컴퓨팅 파워
- 다양한 인스턴스 타입
- 보안 그룹 관리

#### 접근 방법:
```bash
# 1. AWS 콘솔 접근
https://console.aws.amazon.com/ec2/

# 2. SSH 접근
ssh -i "keypair.pem" ec2-user@ec2-xx-xx-xx-xx.compute-1.amazonaws.com

# 3. AWS CLI 접근
aws ec2 describe-instances
aws ec2 start-instances --instance-ids i-1234567890abcdef0

# 4. 웹 애플리케이션 URL
http://ec2-xx-xx-xx-xx.compute-1.amazonaws.com:8080
```

### 5. 🌍 **Amazon Route 53** (DNS 관리)
**용도**: 도메인 이름 관리 및 DNS 라우팅
**특징**:
- 도메인 등록
- DNS 레코드 관리
- 헬스 체크

#### 접근 방법:
```bash
# 1. AWS 콘솔 접근
https://console.aws.amazon.com/route53/

# 2. AWS CLI 접근
aws route53 list-hosted-zones
aws route53 list-resource-record-sets --hosted-zone-id [ZONE_ID]

# 3. 도메인 URL
https://medilog.yourdomain.com
```

### 6. 🔒 **AWS Certificate Manager** (SSL 인증서)
**용도**: HTTPS 보안 연결을 위한 SSL/TLS 인증서
**특징**:
- 무료 SSL 인증서
- 자동 갱신
- AWS 서비스 통합

#### 접근 방법:
```bash
# 1. AWS 콘솔 접근
https://console.aws.amazon.com/acm/

# 2. AWS CLI 접근
aws acm list-certificates
aws acm describe-certificate --certificate-arn [CERT_ARN]
```

### 7. 📊 **Amazon CloudWatch** (모니터링)
**용도**: 애플리케이션 성능 모니터링 및 로그 관리
**특징**:
- 실시간 모니터링
- 로그 수집 및 분석
- 알람 설정

#### 접근 방법:
```bash
# 1. AWS 콘솔 접근
https://console.aws.amazon.com/cloudwatch/

# 2. AWS CLI 접근
aws logs describe-log-groups
aws cloudwatch get-metric-statistics --namespace AWS/S3

# 3. 로그 확인
aws logs filter-log-events --log-group-name /aws/amplify/[APP_NAME]
```

## 🔐 AWS 서비스 접근 인증 방법

### 1. **AWS 콘솔 접근**
```
URL: https://console.aws.amazon.com/
로그인: AWS 계정 이메일 + 비밀번호
MFA: 다단계 인증 (권장)
```

### 2. **AWS CLI 설정**
```bash
# AWS CLI 설치
pip install awscli

# 자격 증명 설정
aws configure
# AWS Access Key ID: [YOUR_ACCESS_KEY]
# AWS Secret Access Key: [YOUR_SECRET_KEY]
# Default region name: us-east-1
# Default output format: json

# 프로필 확인
aws sts get-caller-identity
```

### 3. **IAM 역할 및 정책**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:*",
        "cloudfront:*",
        "amplify:*",
        "route53:*",
        "acm:*",
        "cloudwatch:*"
      ],
      "Resource": "*"
    }
  ]
}
```

## 📱 각 서비스별 접근 URL 정리

### **개발 환경**
- **로컬 서버**: http://localhost:8080
- **네트워크 접근**: http://172.16.0.52:8080
- **GitHub 소스**: https://github.com/biomach0107/TIL1

### **AWS 프로덕션 환경**

#### **Amplify 배포**
```
메인 URL: https://main.d[12자리].amplifyapp.com
브랜치 URL: https://dev.d[12자리].amplifyapp.com
관리 콘솔: https://console.aws.amazon.com/amplify/home#/[APP_ID]
```

#### **S3 정적 웹사이트**
```
웹사이트 URL: http://[버킷명].s3-website-us-east-1.amazonaws.com
S3 콘솔: https://s3.console.aws.amazon.com/s3/buckets/[버킷명]
```

#### **CloudFront CDN**
```
CDN URL: https://d[13자리].cloudfront.net
관리 콘솔: https://console.aws.amazon.com/cloudfront/home#/distributions/[ID]
```

#### **EC2 서버**
```
HTTP URL: http://ec2-[IP].compute-1.amazonaws.com:8080
HTTPS URL: https://ec2-[IP].compute-1.amazonaws.com (SSL 설정 시)
SSH 접근: ssh -i keypair.pem ec2-user@[PUBLIC_IP]
```

#### **Route 53 도메인**
```
커스텀 도메인: https://medilog.yourdomain.com
DNS 관리: https://console.aws.amazon.com/route53/home#/hostedzone/[ZONE_ID]
```

## 💰 서비스별 예상 비용

### **무료 티어 (12개월)**
- **S3**: 5GB 스토리지
- **CloudFront**: 1TB 데이터 전송
- **EC2**: t2.micro 750시간/월
- **Route 53**: 호스팅 존 1개 ($0.50/월)

### **유료 사용 시 (월 예상)**
- **Amplify**: $0-5 (트래픽에 따라)
- **S3**: $1-3 (스토리지 + 요청)
- **CloudFront**: $1-10 (데이터 전송량)
- **EC2**: $8-50 (인스턴스 타입)
- **Route 53**: $0.50-2 (도메인 + 쿼리)

## 🔧 서비스 관리 명령어

### **일괄 상태 확인**
```bash
# 모든 서비스 상태 확인
aws amplify list-apps
aws s3 ls
aws cloudfront list-distributions
aws ec2 describe-instances --query 'Reservations[].Instances[].State.Name'
aws route53 list-hosted-zones
```

### **로그 모니터링**
```bash
# Amplify 빌드 로그
aws amplify get-job --app-id [APP_ID] --branch-name main --job-id [JOB_ID]

# CloudWatch 로그
aws logs tail /aws/amplify/[APP_NAME] --follow

# EC2 시스템 로그
ssh -i keypair.pem ec2-user@[IP] "tail -f /var/log/messages"
```

### **자동화 스크립트**
```bash
#!/bin/bash
# 전체 배포 스크립트
echo "🚀 Medi Log 배포 시작..."

# S3 업로드
aws s3 sync . s3://medi-log-app/ --delete

# CloudFront 캐시 무효화
aws cloudfront create-invalidation --distribution-id [ID] --paths "/*"

# Amplify 배포 트리거
aws amplify start-job --app-id [APP_ID] --branch-name main --job-type RELEASE

echo "✅ 배포 완료!"
```

---

## 📞 지원 및 문의

**AWS 기술 지원**:
- 기본 지원: 무료 (계정/결제 문의)
- 개발자 지원: $29/월
- 비즈니스 지원: $100/월

**문서 및 자료**:
- AWS 문서: https://docs.aws.amazon.com/
- AWS 아키텍처 센터: https://aws.amazon.com/architecture/
- AWS 교육: https://aws.amazon.com/training/