## Question #62
A company is deploying a new public web application to AWS. 
The application will run behind an Application Load Balancer (ALB). 
The application needs to be encrypted at the edge with an SSL/TLS certificate that is issued by an external certificate authority (CA). 
The certificate must be rotated each year before the certificate expires.

What should a solutions architect do to meet these requirements?

A. Use AWS Certificate Manager (ACM) to issue an SSL/TLS certificate. Apply the certificate to the ALB. Use the managed renewal feature to automatically rotate the certificate.

B. Use AWS Certificate Manager (ACM) to issue an SSL/TLS certificate. Import the key material from the certificate. Apply the certificate to the ALUse the managed renewal feature to automatically rotate the certificate.

C. Use AWS Certificate Manager (ACM) Private Certificate Authority to issue an SSL/TLS certificate from the root CA. Apply the certificate to the ALB. Use the managed renewal feature to automatically rotate the certificate.

D. Use AWS Certificate Manager (ACM) to import an SSL/TLS certificate. Apply the certificate to the ALB. Use Amazon EventBridge (Amazon CloudWatch Events) to send a notification when the certificate is nearing expiration. Rotate the certificate manually.

## Question #62 분석

### ✅ 요구사항
- **퍼블릭 웹 애플리케이션** (ALB 뒤에서 실행)
- **외부 CA에서 발급한 SSL/TLS 인증서** 사용 필요
- **엣지에서 암호화** (ALB에서 SSL 터미네이션)
- **연간 인증서 로테이션** (만료 전)

### ✅ 선택지 분석

**A. ACM으로 SSL/TLS 인증서 발급 + 자동 갱신**
- **ACM 발급**: AWS가 인증서 발급 (외부 CA 아님) ❌
- **요구사항 위반**: "외부 CA 발급" 조건 불충족 ❌
- **자동 갱신**: ACM 발급 인증서만 지원 ❌

**B. ACM 발급 + 키 자료 가져오기 + 자동 갱신**
- **모순**: ACM 발급인데 키 자료 가져오기는 논리적 오류 ❌
- **외부 CA 아님**: 여전히 AWS가 발급 ❌
- **기술적 불가능**: ACM 발급과 키 가져오기 동시 불가 ❌

**C. ACM Private CA로 루트 CA에서 발급**
- **Private CA**: 퍼블릭 웹사이트에 부적합 ❌
- **내부 CA**: 외부 CA 요구사항과 다름 ❌
- **브라우저 신뢰**: 퍼블릭 신뢰 체인 부족 ❌

**D. ACM으로 SSL/TLS 인증서 가져오기 + EventBridge + 수동 로테이션** ⭐
- **외부 CA 인증서**: 가져오기로 외부 CA 조건 충족 ✅
- **ALB 적용**: 가져온 인증서 ALB에 연결 가능 ✅
- **EventBridge 알림**: 만료 임박 시 알림 설정 ✅
- **수동 로테이션**: 외부 CA 인증서는 수동 갱신 필요 ✅

### 📋 핵심 개념 정리

#### **ACM 인증서 타입**
```yaml
ACM 발급 인증서:
  ✅ AWS가 도메인 검증 후 발급
  ✅ 자동 갱신 지원
  ✅ 퍼블릭 신뢰 체인
  ❌ 외부 CA 아님

ACM 가져온 인증서:
  ✅ 외부 CA에서 발급된 인증서 지원
  ✅ 기존 인증서 활용 가능
  ❌ 자동 갱신 불가능
  ⚠️ 만료 전 수동 갱신 필요

Private CA:
  ✅ 내부 조직용
  ❌ 퍼블릭 신뢰 체인 없음
  ❌ 브라우저 경고 발생
```

#### **외부 CA vs AWS CA**
```yaml
외부 CA (예: DigiCert, Let's Encrypt):
  ✅ 기존 인증서 정책 활용
  ✅ 특정 CA 요구사항 충족
  ✅ 조직 표준 준수
  ⚠️ 수동 갱신 프로세스

AWS CA (ACM):
  ✅ 완전 자동화
  ✅ AWS 서비스 통합
  ✅ 비용 효율적
  ❌ 외부 CA 요구사항 불충족
```

#### **정답 시나리오 (D번)**
```yaml
1. 외부 CA에서 SSL/TLS 인증서 획득
   - 도메인 검증 완료
   - CSR 생성 및 인증서 발급 요청
   
2. ACM으로 인증서 가져오기
   - 인증서 파일 (.crt)
   - 개인 키 파일 (.key)
   - 인증서 체인 (중간 CA)
   
3. ALB에 인증서 적용
   - HTTPS 리스너 설정
   - 가져온 ACM 인증서 선택
   
4. 만료 알림 설정
   - EventBridge 규칙 생성
   - ACM 인증서 만료 이벤트 감지
   - SNS 토픽으로 알림 발송
   
5. 연간 로테이션 프로세스
   - 만료 30일 전 알림 수신
   - 외부 CA에서 새 인증서 발급
   - ACM에 새 인증서 가져오기
   - ALB 설정 업데이트

EventBridge 규칙 예시:
  이벤트 소스: ACM
  이벤트 타입: Certificate Approaching Expiration
  대상: SNS 토픽 (운영팀 알림)
```

#### **인증서 가져오기 요구사항**
```yaml
필수 구성 요소:
  ✅ SSL/TLS 인증서 (PEM 형식)
  ✅ 개인 키 (PEM 형식)
  ✅ 인증서 체인 (중간 CA)

보안 고려사항:
  ✅ 개인 키 보안 관리
  ✅ 인증서 체인 완성도
  ✅ 만료 날짜 모니터링
```
