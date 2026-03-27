# 🚀 Medi Log 빠른 접근 가이드

## 📱 현재 접근 가능한 링크들

### **개발 환경**
- 🏠 **로컬 서버**: http://localhost:8080
- 🌐 **네트워크 접근**: http://172.16.0.52:8080
- 📂 **GitHub 소스**: https://github.com/biomach0107/TIL1

### **AWS 배포 환경** (배포 후 생성됨)

#### **1. AWS Amplify** (추천)
```
🔗 앱 URL: https://main.d[12자리랜덤].amplifyapp.com
⚙️ 관리 콘솔: https://console.aws.amazon.com/amplify/
📊 배포 상태: 실시간 확인 가능
💰 비용: 월 $0-3
```

#### **2. Amazon S3 정적 웹사이트**
```
🔗 웹사이트 URL: http://medi-log-app-[타임스탬프].s3-website-us-east-1.amazonaws.com
⚙️ S3 콘솔: https://s3.console.aws.amazon.com/
📦 스토리지: 무제한 확장
💰 비용: 월 $1-3
```

#### **3. CloudFront CDN**
```
🔗 CDN URL: https://d[13자리랜덤].cloudfront.net
⚙️ 관리 콘솔: https://console.aws.amazon.com/cloudfront/
🌍 글로벌: 전 세계 빠른 접근
💰 비용: 월 $1-5
```

#### **4. EC2 서버** (동적 기능 필요시)
```
🔗 HTTP URL: http://ec2-[IP주소].compute-1.amazonaws.com:8080
🔒 HTTPS URL: https://ec2-[IP주소].compute-1.amazonaws.com (SSL 설정 시)
⚙️ EC2 콘솔: https://console.aws.amazon.com/ec2/
💰 비용: 월 $8-50
```

## 🔑 AWS 콘솔 빠른 접근

### **주요 서비스 콘솔 링크**
```
🌐 Amplify: https://console.aws.amazon.com/amplify/
🗄️ S3: https://s3.console.aws.amazon.com/
🚀 CloudFront: https://console.aws.amazon.com/cloudfront/
🖥️ EC2: https://console.aws.amazon.com/ec2/
🌍 Route 53: https://console.aws.amazon.com/route53/
🔒 Certificate Manager: https://console.aws.amazon.com/acm/
📊 CloudWatch: https://console.aws.amazon.com/cloudwatch/
```

### **AWS CLI 빠른 명령어**
```bash
# 현재 계정 확인
aws sts get-caller-identity

# Amplify 앱 목록
aws amplify list-apps

# S3 버킷 목록
aws s3 ls

# EC2 인스턴스 상태
aws ec2 describe-instances --query 'Reservations[].Instances[].[InstanceId,State.Name,PublicIpAddress]'

# CloudFront 배포 목록
aws cloudfront list-distributions --query 'DistributionList.Items[].[Id,DomainName,Status]'
```

## 📋 배포 체크리스트

### **배포 전 확인사항**
- [ ] GitHub 레포지토리 업데이트 완료
- [ ] AWS 계정 및 권한 확인
- [ ] 도메인 준비 (선택사항)
- [ ] SSL 인증서 준비 (HTTPS 사용시)

### **배포 후 테스트**
- [ ] 메인 페이지 로딩 확인
- [ ] 월별 트렌드 차트 작동
- [ ] 약물 정보 모달 기능
- [ ] 모바일 반응형 디자인
- [ ] HTTPS 보안 연결 (프로덕션)

## 🛠️ 문제 해결 가이드

### **일반적인 문제들**

#### **1. 배포 실패**
```bash
# Amplify 빌드 로그 확인
aws amplify get-job --app-id [APP_ID] --branch-name main --job-id [JOB_ID]

# 해결 방법: amplify.yml 파일 확인
```

#### **2. 도메인 접근 불가**
```bash
# DNS 전파 확인
nslookup yourdomain.com
dig yourdomain.com

# 해결 방법: Route 53 레코드 확인
```

#### **3. HTTPS 인증서 오류**
```bash
# 인증서 상태 확인
aws acm list-certificates
aws acm describe-certificate --certificate-arn [ARN]

# 해결 방법: 도메인 검증 완료 확인
```

#### **4. 성능 이슈**
```bash
# CloudWatch 메트릭 확인
aws cloudwatch get-metric-statistics --namespace AWS/CloudFront --metric-name Requests

# 해결 방법: CloudFront 캐싱 설정 최적화
```

## 📞 지원 연락처

### **AWS 지원**
- 📧 기본 지원: AWS 콘솔 > Support Center
- 💬 커뮤니티: https://forums.aws.amazon.com/
- 📚 문서: https://docs.aws.amazon.com/

### **GitHub 이슈**
- 🐛 버그 리포트: https://github.com/biomach0107/TIL1/issues
- 💡 기능 요청: GitHub Issues 탭 활용

## 🎯 권장 배포 순서

### **1단계: 개발 환경 테스트**
```bash
cd TIL1
python3 server.py
# http://localhost:8080 에서 테스트
```

### **2단계: GitHub 업데이트**
```bash
git add .
git commit -m "feat: 새로운 기능 추가"
git push origin main
```

### **3단계: AWS Amplify 배포**
1. AWS Amplify 콘솔 접속
2. GitHub 레포지토리 연결
3. 자동 배포 대기 (2-3분)
4. 생성된 URL로 접속 테스트

### **4단계: 도메인 연결** (선택)
1. Route 53에서 도메인 설정
2. Certificate Manager에서 SSL 인증서 발급
3. Amplify에서 커스텀 도메인 연결

---

## 🏆 최종 결과물

**완성된 Medi Log 애플리케이션**:
- ✅ 월별 트렌드 차트 (5일 단위 데이터 포인트)
- ✅ 복용률 피드백 메시지 시스템
- ✅ 처방전 분석 및 약물 정보
- ✅ 반응형 디자인 및 접근성 기능
- ✅ AWS 클라우드 배포 준비 완료
- ✅ 전 세계 접근 가능한 HTTPS 링크

**예상 배포 시간**: 3-5분 ⏱️
**예상 월 비용**: $0-5 💰
**글로벌 접근**: 전 세계 어디서나 🌍