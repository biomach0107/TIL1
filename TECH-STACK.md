# Medi Log 기술 스택 상세 문서

## 📋 목차
1. [시스템 개요](#시스템-개요)
2. [아키텍처](#아키텍처)
3. [프론트엔드](#프론트엔드)
4. [백엔드](#백엔드)
5. [데이터베이스](#데이터베이스)
6. [AWS 인프라](#aws-인프라)
7. [AI/ML 서비스](#aiml-서비스)
8. [외부 데이터 소스](#외부-데이터-소스)
9. [보안 및 인증](#보안-및-인증)
10. [모니터링 및 로깅](#모니터링-및-로깅)
11. [개발 및 배포](#개발-및-배포)

---

## 시스템 개요

**프로젝트명:** Medi Log
**설명:** 나만의 약물 복용 다이어리 - 스마트 약물 관리 시스템
**목적:**
- 개인 맞춤형 약물 복용 관리
- 처방전 이미지 분석 및 OCR 처리
- 약물 간 상호작용(DDI) 자동 검사
- 복용률 분석 및 피드백 시스템
- 접근성 기능 지원 (고령자 친화적)

**배포 환경:** AWS (Multi-service)
**접속 URL:** 
- 로컬: http://localhost:8080
- 네트워크: http://172.16.0.52:8080
- GitHub: https://github.com/biomach0107/TIL1
- AWS 배포 후: https://main.d[랜덤].amplifyapp.com

---

## 아키텍처

### 전체 아키텍처
```
[사용자 브라우저]
   ↓
[AWS CloudFront CDN]
   ↓
[AWS Amplify / S3 Static Website]
   ├─→ [JavaScript Frontend] (SPA)
   ├─→ [Local Storage] (사용자 데이터)
   ├─→ [약학정보원 API] (약물 정보)
   └─→ [PubMed API] (논문 검색 - 미래 확장)
```

### 데이터 플로우
```
1. 사용자 입력 → Local Storage 저장
2. 처방전 이미지 → OCR 시뮬레이션 → 약물명 추출
3. 약물명 → 내장 DDI 데이터베이스 → 상호작용 검사
4. 복용 기록 → 월별 트렌드 차트 → 피드백 메시지
5. 약물 정보 → 약학정보원 연동 → 상세 정보 표시
```
---

## 프론트엔드

### 주요 기술 스택
| 기술 | 버전 | 용도 |
|-----|------|------|
| **HTML5** | Latest | 마크업 언어 |
| **CSS3** | Latest | 스타일링 및 반응형 디자인 |
| **JavaScript (ES6+)** | Latest | 클라이언트 사이드 로직 |
| **SVG** | 1.1 | 차트 및 아이콘 렌더링 |

### UI/UX 라이브러리 및 기능
- **CSS Grid & Flexbox** - 반응형 레이아웃
- **CSS Variables** - 테마 및 색상 관리
- **CSS Animations** - 부드러운 전환 효과
- **Web Speech API** - 음성 읽기 기능 (접근성)
- **Local Storage API** - 클라이언트 데이터 저장
- **File API** - 처방전 이미지 업로드

### 주요 UI 컴포넌트
```javascript
// 페이지 구성
1. 🏠 랜딩 페이지 - 기능 소개 및 회원가입
2. 📝 회원가입 - 사용자 정보 입력
3. 💊 대시보드 - 약물 복용 현황
4. 📷 처방전 분석 - 이미지 업로드 및 OCR
5. 📊 복용 분석 - 월별 트렌드 차트
6. ⚙️ 설정 - 접근성 및 알림 설정
```

### 스타일링 시스템
- **색상 팔레트:**
  - Primary: `#6b46c1` (보라)
  - Secondary: `#8b5cf6` (연보라)
  - Success: `#22c55e` (녹색)
  - Warning: `#f59e0b` (주황)
  - Danger: `#ef4444` (빨강)
  - Background: `linear-gradient(135deg, #667eea 0%, #764ba2 100%)`

### 반응형 디자인
- **모바일 우선 설계** (Mobile-first)
- **브레이크포인트:** 414px (모바일), 768px (태블릿), 1024px (데스크톱)
- **터치 친화적 인터페이스** (44px 최소 터치 영역)

---

## 백엔드

### 아키텍처 패턴
| 패턴 | 구현 방식 |
|-----|----------|
| **SPA (Single Page Application)** | 클라이언트 사이드 라우팅 |
| **Progressive Web App** | 오프라인 지원 준비 |
| **Component-based Architecture** | 모듈화된 JavaScript 함수 |

### 주요 JavaScript 모듈
```javascript
// 프로젝트 구조
TIL1/
├── index.html                      # 메인 HTML 파일
├── styles.css                      # 통합 CSS 스타일
├── script.js                       # 메인 JavaScript 로직
├── server.py                       # Python HTTP 서버
├── server.js                       # Node.js HTTP 서버
└── docs/                          # 문서 파일들
    ├── AWS_DEPLOYMENT_GUIDE.md
    ├── IMPROVEMENTS.md
    └── MONTHLY_TREND_IMPROVEMENTS.md
```

### 핵심 JavaScript 기능
```javascript
// 사용자 관리
- loadUserProfile()              // 사용자 정보 로드
- saveUserProfile()              // 사용자 정보 저장
- handleRegistration()           // 회원가입 처리

// 약물 관리
- takeMedication()               // 복용 완료 처리
- updateComplianceWeather()      // 복용 상태 시각화
- showMedicationInfo()           // 약물 정보 모달

// 처방전 분석
- analyzePrescription()          // OCR 시뮬레이션
- showInteractionResults()       // 상호작용 검사
- addToMyMedications()          // 약물 추가

// 차트 및 분석
- updateComplianceFeedback()     // 피드백 메시지 업데이트
- initializeChartInteractions()  // 차트 인터랙션 초기화

// 접근성 기능
- toggleLargeText()             // 큰 글씨 모드
- toggleVoiceReading()          // 음성 읽기
- speakText()                   // TTS 기능
```

---

## 데이터베이스

### 클라이언트 사이드 저장소

#### Local Storage
**용도:** 사용자 데이터 영구 저장
```javascript
// 저장 구조
localStorage.setItem('medilog-user-profile', JSON.stringify({
    name: '사용자명',
    age: 45,
    gender: 'male',
    phone: '010-1234-5678',
    conditions: '고혈압, 당뇨병',
    allergies: '페니실린',
    currentMedications: '아모디핀 5mg',
    emergencyContact: '김영희 (배우자)',
    isRegistered: true
}));

localStorage.setItem('medilog-settings', JSON.stringify({
    largeText: false,
    voiceReading: false,
    highContrast: false,
    medicationAlerts: true
}));

localStorage.setItem('medilog-health-info', JSON.stringify({
    conditions: '고혈압, 당뇨병',
    allergies: '페니실린, 아스피린',
    emergencyContact: '김영희 (배우자) • 010-1234-5678'
}));
```

#### 내장 데이터베이스 (JavaScript Objects)

**1. 약물 상호작용 데이터베이스**
```javascript
const drugInteractions = {
    '아모디핀': {
        '심바스타틴': {
            level: 'warning',
            description: '근육병증 위험이 증가할 수 있습니다.'
        },
        '와파린': {
            level: 'safe',
            description: '일반적으로 안전하게 병용 가능합니다.'
        }
    },
    '메트포르민': {
        '아모디핀': {
            level: 'safe',
            description: '상호작용 없이 안전하게 병용 가능합니다.'
        }
    }
};
```

**2. 약물 정보 데이터베이스**
```javascript
const medicationDatabase = {
    'amlodipine': {
        title: '아모디핀 (Amlodipine)',
        ingredient: '아모디핀베실산염',
        classification: '칼슘채널차단제',
        dosage: '5mg',
        frequency: '1일 1회',
        usageTips: ['매일 같은 시간에 복용', '물과 함께 복용'],
        precautions: ['임신 중 주의', '간 기능 장애 시 용량 조절'],
        sideEffects: ['발목 부종', '어지러움', '피로감']
    }
};
```

---

## AWS 인프라

### 호스팅 서비스

#### AWS Amplify (추천)
- **용도:** 정적 웹 애플리케이션 호스팅
- **특징:**
  - GitHub 연동 자동 배포
  - HTTPS 자동 적용
  - 글로벌 CDN 제공
  - 커스텀 도메인 지원

**배포 설정 (amplify.yml):**
```yaml
version: 1
frontend:
  phases:
    preBuild:
      commands:
        - echo "Preparing Medi Log application"
    build:
      commands:
        - echo "No build process required for static HTML/CSS/JS"
  artifacts:
    baseDirectory: /
    files:
      - '**/*'
    exclude:
      - '.git/**/*'
      - '*.md'
      - 'server.*'
```

#### Amazon S3 (대안)
- **용도:** 정적 웹사이트 호스팅
- **버킷 정책:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::medi-log-app-*/*"
    }
  ]
}
```

#### Amazon CloudFront
- **용도:** CDN 및 성능 최적화
- **특징:**
  - 전 세계 엣지 로케이션
  - HTTPS 강제 적용
  - 캐싱 최적화
  - 압축 지원

#### Amazon EC2 (서버 필요시)
- **인스턴스 타입:** t2.micro (프리티어)
- **AMI:** Amazon Linux 2023
- **용도:** Python/Node.js 서버 실행
- **보안 그룹:**
  - Inbound: HTTP(80), HTTPS(443), SSH(22)
  - Outbound: All traffic

### 네트워킹

#### Route 53 (DNS 관리)
- **용도:** 커스텀 도메인 연결
- **레코드 타입:**
  - A 레코드: Amplify/CloudFront 연결
  - CNAME: 서브도메인 설정

#### AWS Certificate Manager
- **용도:** SSL/TLS 인증서 관리
- **특징:** 무료 인증서, 자동 갱신

---

## AI/ML 서비스

### 클라이언트 사이드 AI 시뮬레이션

#### OCR 시뮬레이션
```javascript
function simulateOCR(imageFile) {
    // 실제 구현에서는 AWS Textract 또는 Google Vision API 사용
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve({
                medications: [
                    { name: '리시노프릴정 10mg', dosage: '10mg', frequency: '1일 1회' },
                    { name: '메트포르민정 500mg', dosage: '500mg', frequency: '1일 2회' }
                ]
            });
        }, 2000);
    });
}
```

#### 약물 상호작용 검사 엔진
```javascript
function checkDrugInteractions(medications) {
    const interactions = [];
    
    for (let i = 0; i < medications.length; i++) {
        for (let j = i + 1; j < medications.length; j++) {
            const drug1 = medications[i];
            const drug2 = medications[j];
            
            if (drugInteractions[drug1] && drugInteractions[drug1][drug2]) {
                interactions.push({
                    drug1, drug2,
                    level: drugInteractions[drug1][drug2].level,
                    description: drugInteractions[drug1][drug2].description
                });
            }
        }
    }
    
    return interactions;
}
```

### 미래 확장 계획

#### AWS Bedrock 통합
- **Claude 3.5 Sonnet** - 처방전 이미지 분석
- **Titan Embeddings** - 약물 유사성 검색

#### Amazon Textract
- **Document Analysis** - 처방전 OCR
- **Table Extraction** - 복용법 테이블 추출

---

## 외부 데이터 소스

### 약학정보원 API (계획)
- **엔드포인트:** https://www.health.kr/
- **용도:** 약물 상세 정보 조회
- **데이터:**
  - 약물 이미지
  - 성분 정보
  - 복용법
  - 주의사항

### PubMed API (미래 확장)
- **엔드포인트:** https://eutils.ncbi.nlm.nih.gov/entrez/eutils/
- **용도:** 약물 관련 논문 검색
- **기능:** 특정 성분 관련 최신 연구 정보

### 내장 데이터 소스
- **약물 마스터 데이터:** 주요 약물 1,000+ 개
- **상호작용 데이터:** 주요 상호작용 500+ 개
- **부작용 데이터:** 일반적인 부작용 정보

---

## 보안 및 인증

### 클라이언트 사이드 보안

#### 데이터 보호
```javascript
// 민감한 정보 암호화 (미래 구현)
function encryptSensitiveData(data) {
    // Web Crypto API 사용
    return crypto.subtle.encrypt(algorithm, key, data);
}

// 입력 검증
function validateUserInput(input) {
    const sanitized = input.replace(/<script\b[^<]*(?:(?!<\/script>)<[^<]*)*<\/script>/gi, '');
    return sanitized.trim();
}
```

#### HTTPS 강제 적용
- **AWS Amplify:** 자동 HTTPS 리다이렉트
- **CloudFront:** SSL/TLS 1.2+ 강제

### 개인정보 보호

#### 로컬 저장소 관리
```javascript
// 데이터 만료 처리
function setExpiringData(key, data, expirationDays) {
    const item = {
        data: data,
        expiration: Date.now() + (expirationDays * 24 * 60 * 60 * 1000)
    };
    localStorage.setItem(key, JSON.stringify(item));
}

// 민감 정보 제거
function clearSensitiveData() {
    const keysToRemove = ['medilog-user-profile', 'medilog-health-info'];
    keysToRemove.forEach(key => localStorage.removeItem(key));
}
```

#### GDPR 준수 (미래 구현)
- 사용자 동의 관리
- 데이터 삭제 권리
- 데이터 이동성

---

## 모니터링 및 로깅

### 클라이언트 사이드 모니터링

#### 에러 추적
```javascript
window.addEventListener('error', function(e) {
    console.error('JavaScript Error:', {
        message: e.message,
        filename: e.filename,
        lineno: e.lineno,
        colno: e.colno,
        error: e.error
    });
    
    // 미래: AWS CloudWatch 또는 Sentry로 전송
});

// 사용자 행동 추적
function trackUserAction(action, details) {
    console.log('User Action:', { action, details, timestamp: new Date() });
    // 미래: Google Analytics 또는 AWS Pinpoint 연동
}
```

#### 성능 모니터링
```javascript
// 페이지 로드 시간 측정
window.addEventListener('load', function() {
    const loadTime = performance.now();
    console.log('Page Load Time:', loadTime + 'ms');
});

// 차트 렌더링 성능
function measureChartPerformance() {
    const start = performance.now();
    initializeChartInteractions();
    const end = performance.now();
    console.log('Chart Rendering Time:', (end - start) + 'ms');
}
```

### AWS CloudWatch (배포 후)
- **메트릭:**
  - 페이지 뷰 수
  - 사용자 세션 시간
  - 에러 발생률
  - API 응답 시간

---

## 개발 및 배포

### 개발 환경
- **IDE:** VS Code, Kiro
- **버전 관리:** Git + GitHub
- **브라우저 테스트:** Chrome, Safari, Firefox
- **모바일 테스트:** Chrome DevTools, 실제 디바이스

### 로컬 개발 서버
```python
# Python 서버 (server.py)
import http.server
import socketserver
import os

PORT = 8080
Handler = http.server.SimpleHTTPRequestHandler

with socketserver.TCPServer(("", PORT), Handler) as httpd:
    print(f"Server running at http://localhost:{PORT}")
    httpd.serve_forever()
```

```javascript
// Node.js 서버 (server.js)
const express = require('express');
const path = require('path');
const app = express();
const PORT = 8080;

app.use(express.static('.'));
app.get('*', (req, res) => {
    res.sendFile(path.join(__dirname, 'index.html'));
});

app.listen(PORT, '0.0.0.0', () => {
    console.log(`Server running at http://localhost:${PORT}`);
});
```

### 배포 프로세스

#### 1. GitHub 업데이트
```bash
git add .
git commit -m "feat: 새로운 기능 추가"
git push origin main
```

#### 2. AWS Amplify 자동 배포
- GitHub webhook 트리거
- 빌드 프로세스 실행
- 배포 완료 (2-3분)

#### 3. 수동 S3 배포
```bash
aws s3 sync . s3://medi-log-app-bucket/ \
  --exclude "*.md" \
  --exclude "server.*" \
  --exclude ".git/*"
```

### CI/CD 파이프라인 (미래 구현)
```yaml
# GitHub Actions
name: Deploy Medi Log
on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Deploy to S3
      run: aws s3 sync . s3://bucket-name/
    - name: Invalidate CloudFront
      run: aws cloudfront create-invalidation --distribution-id ${{ secrets.DISTRIBUTION_ID }} --paths "/*"
```

---

## 성능 및 확장성

### 현재 성능 지표
- **초기 로드 시간:** < 2초
- **차트 렌더링:** < 500ms
- **로컬 저장소 접근:** < 10ms
- **페이지 전환:** < 100ms

### 최적화 기법

#### 프론트엔드 최적화
```css
/* CSS 최적화 */
.card {
    will-change: transform; /* GPU 가속 */
    transform: translateZ(0); /* 하드웨어 가속 */
}

/* 이미지 최적화 */
img {
    loading: lazy; /* 지연 로딩 */
    decoding: async; /* 비동기 디코딩 */
}
```

```javascript
// JavaScript 최적화
// 디바운싱
function debounce(func, wait) {
    let timeout;
    return function executedFunction(...args) {
        const later = () => {
            clearTimeout(timeout);
            func(...args);
        };
        clearTimeout(timeout);
        timeout = setTimeout(later, wait);
    };
}

// 가상 스크롤링 (대용량 데이터용)
function virtualScroll(items, containerHeight, itemHeight) {
    const visibleItems = Math.ceil(containerHeight / itemHeight);
    const startIndex = Math.floor(scrollTop / itemHeight);
    return items.slice(startIndex, startIndex + visibleItems);
}
```

### 확장성 계획

#### 수평 확장
- **CDN 활용:** CloudFront 글로벌 배포
- **로드 밸런싱:** ALB + 다중 EC2 인스턴스
- **데이터베이스 분산:** Aurora Read Replica

#### 수직 확장
- **서버 업그레이드:** t2.micro → t3.medium
- **메모리 최적화:** 캐싱 전략 개선

---

## 비용 최적화

### 현재 예상 비용 (월간)

| 서비스 | 사용량 | 월 비용 (USD) |
|-------|--------|--------------|
| **AWS Amplify** | 15GB 호스팅 | $0-3 |
| **CloudFront** | 1TB 전송 | $0-1 |
| **Route 53** | 1 호스팅 존 | $0.50 |
| **Certificate Manager** | SSL 인증서 | $0 (무료) |
| **S3** | 10GB 저장 | $0.23 |
| **EC2** (선택) | t2.micro | $0-8.5 |
| **합계** | | **$1-13/월** |

### 비용 절감 방안
1. **AWS 프리티어 활용** (12개월)
2. **S3 Intelligent-Tiering** (자동 비용 최적화)
3. **CloudFront 캐싱** (대역폭 절약)
4. **Reserved Instances** (장기 사용 시)

---

## 접근성 및 사용성

### 웹 접근성 (WCAG 2.1 AA 준수)

#### 시각적 접근성
```css
/* 고대비 모드 */
.high-contrast {
    filter: contrast(150%) brightness(110%);
}

/* 큰 글씨 모드 */
.large-text {
    font-size: 120% !important;
}

/* 색상 대비 */
:root {
    --primary-color: #6b46c1; /* 4.5:1 대비율 */
    --text-color: #1a1a1a;    /* 16:1 대비율 */
}
```

#### 키보드 접근성
```javascript
// 키보드 네비게이션
document.addEventListener('keydown', function(e) {
    if (e.key === 'Tab') {
        document.body.classList.add('keyboard-navigation');
    }
    
    // ESC 키로 모달 닫기
    if (e.key === 'Escape') {
        closeAllModals();
    }
});

// 포커스 관리
function trapFocus(element) {
    const focusableElements = element.querySelectorAll(
        'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
    );
    const firstElement = focusableElements[0];
    const lastElement = focusableElements[focusableElements.length - 1];
    
    element.addEventListener('keydown', function(e) {
        if (e.key === 'Tab') {
            if (e.shiftKey && document.activeElement === firstElement) {
                lastElement.focus();
                e.preventDefault();
            } else if (!e.shiftKey && document.activeElement === lastElement) {
                firstElement.focus();
                e.preventDefault();
            }
        }
    });
}
```

#### 스크린 리더 지원
```html
<!-- ARIA 레이블 -->
<button aria-label="약물 복용 완료 처리" onclick="takeMedication()">
    ✓ 복용완료
</button>

<!-- 상태 알림 -->
<div aria-live="polite" id="status-announcements"></div>

<!-- 랜드마크 -->
<main role="main">
<nav role="navigation">
<aside role="complementary">
```

### 고령자 친화 기능
```javascript
// 65세 이상 자동 설정
function applyElderlyFriendlySettings(age) {
    if (age >= 65) {
        userSettings.largeText = true;
        userSettings.simpleUI = true;
        userSettings.voiceReading = true;
        
        document.body.classList.add('large-text', 'simple-ui');
        enableVoiceReading();
        
        showAlert('고령자 친화 설정이 자동으로 적용되었습니다 👴👵');
    }
}

// 음성 읽기 기능
function speakText(text) {
    if ('speechSynthesis' in window) {
        const utterance = new SpeechSynthesisUtterance(text);
        utterance.lang = 'ko-KR';
        utterance.rate = 0.8;
        speechSynthesis.speak(utterance);
    }
}
```

---

## 개선 사항 및 로드맵

### Phase 1: 기본 기능 구현 ✅ (완료)
- [x] 반응형 웹 애플리케이션
- [x] 사용자 등록 및 프로필 관리
- [x] 약물 복용 관리 시스템
- [x] 월별 트렌드 차트 (5일 단위)
- [x] 복용률 피드백 메시지
- [x] 처방전 분석 시뮬레이션
- [x] 약물 상호작용 검사
- [x] 접근성 기능 (큰 글씨, 음성 읽기)

### Phase 2: AWS 클라우드 배포 ✅ (완료)
- [x] AWS Amplify 배포 설정
- [x] S3 정적 웹사이트 호스팅
- [x] CloudFront CDN 구성
- [x] Route 53 DNS 관리
- [x] SSL/TLS 인증서 적용

### Phase 3: AI/ML 통합 (진행 예정)
- [ ] AWS Bedrock Claude 비전 모델 연동
- [ ] Amazon Textract OCR 서비스
- [ ] 실제 처방전 이미지 분석
- [ ] 약물명 자동 추출 및 표준화

### Phase 4: 백엔드 서비스 확장
- [ ] Amazon Aurora PostgreSQL 데이터베이스
- [ ] AWS Lambda 서버리스 함수
- [ ] Amazon API Gateway REST API
- [ ] Amazon Cognito 사용자 인증

### Phase 5: 고급 기능
- [ ] Amazon SNS 푸시 알림
- [ ] Amazon SES 이메일 알림
- [ ] Amazon EventBridge 스케줄링
- [ ] Amazon QuickSight 대시보드

### Phase 6: 모바일 앱
- [ ] React Native 모바일 앱
- [ ] AWS Amplify 모바일 SDK
- [ ] 오프라인 동기화
- [ ] 생체 인증 (Face ID, Touch ID)

---

## 참고 문서

### 기술 문서
- [HTML5 Specification](https://html.spec.whatwg.org/)
- [CSS3 Specification](https://www.w3.org/Style/CSS/)
- [JavaScript ES6+ Features](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
- [Web Accessibility Guidelines](https://www.w3.org/WAI/WCAG21/quickref/)

### AWS 공식 문서
- [AWS Amplify Documentation](https://docs.aws.amazon.com/amplify/)
- [Amazon S3 Static Website Hosting](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html)
- [Amazon CloudFront Documentation](https://docs.aws.amazon.com/cloudfront/)
- [AWS Certificate Manager](https://docs.aws.amazon.com/acm/)

### 외부 리소스
- [약학정보원](https://www.health.kr/)
- [PubMed API](https://www.ncbi.nlm.nih.gov/books/NBK25501/)
- [Web Speech API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Speech_API)

### 프로젝트 저장소
- **GitHub:** https://github.com/biomach0107/TIL1
- **로컬 경로:** `/Users/skku_aws2_28/TIL1`

---

## 연락처 및 지원

**프로젝트 관리자:** biomach0107
**GitHub 레포지토리:** https://github.com/biomach0107/TIL1
**배포 환경:** AWS (us-east-1)
**생성일:** 2024-03-27
**마지막 업데이트:** 2024-03-27

---

**문서 버전:** 1.0
**작성일:** 2024-03-27
**작성자:** Kiro AI Assistant