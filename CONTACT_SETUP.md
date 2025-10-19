# 📧 고객센터 문의 기능 설정 가이드

현재 웹사이트의 문의 폼은 **mailto:** 링크를 사용합니다.
사용자가 문의하기 버튼을 누르면 기본 이메일 클라이언트(Outlook, Gmail 등)가 열립니다.

## 🎯 현재 동작 방식

1. 사용자가 폼 작성 (이름, 이메일, 제목, 내용)
2. "문의하기" 버튼 클릭
3. 이메일 클라이언트가 자동으로 열림 (수신: gmlehddl95@gmail.com)
4. 사용자가 "전송" 버튼 클릭

**장점:**
- ✅ 즉시 사용 가능 (추가 설정 불필요)
- ✅ 무료
- ✅ 간단함

**단점:**
- ⚠️ 사용자의 이메일 클라이언트 필요
- ⚠️ 웹 브라우저에서 직접 전송 불가

---

## 🚀 더 나은 방법: EmailJS 설정 (선택사항)

웹 브라우저에서 직접 이메일을 보내려면 EmailJS를 사용하세요.

### 1단계: EmailJS 계정 만들기

1. https://www.emailjs.com 접속
2. "Sign Up Free" 클릭
3. Google 계정으로 로그인

### 2단계: Email Service 연결

1. Dashboard → "Add New Service"
2. **Gmail** 선택
3. "Connect Account" → Google 계정 연결 (gmlehddl95@gmail.com)
4. Service ID 복사 (예: `service_abc123`)

### 3단계: Email Template 생성

1. Dashboard → "Email Templates" → "Create New Template"
2. Template Name: `contact_form`
3. Template 내용:

```
From: {{from_name}}
Email: {{from_email}}

제목: {{subject}}

문의 내용:
{{message}}

──────────────────
TeleNews Bot 웹사이트
```

4. Template ID 복사 (예: `template_xyz789`)

### 4단계: Public Key 복사

1. Dashboard → "Account" → "General"
2. Public Key 복사 (예: `Abc123XyZ`)

### 5단계: 코드에 적용

**website/script.js** 파일을 열고 다음 부분을 수정:

```javascript
// 1. 초기화 (5번 줄 근처)
emailjs.init('Abc123XyZ');  // 실제 Public Key로 교체

// 2. 폼 제출 부분 (36번 줄 근처)
// 아래 주석 처리된 EmailJS 코드를 주석 해제:

emailjs.send('service_abc123', 'template_xyz789', {
    from_name: formData.name,
    from_email: formData.email,
    subject: formData.subject,
    message: formData.message
}).then(function(response) {
    // 성공
    formMessage.textContent = '✅ 문의가 전송되었습니다!';
    formMessage.className = 'form-message success';
    contactForm.reset();
    submitButton.disabled = false;
    submitButton.innerHTML = originalText;
}, function(error) {
    // 실패
    formMessage.textContent = '❌ 전송 실패. 직접 이메일로 문의해주세요.';
    formMessage.className = 'form-message error';
    submitButton.disabled = false;
    submitButton.innerHTML = originalText;
});
```

**website/index.html**에서 EmailJS SDK 추가:

```html
<!-- 261번 줄에 추가 -->
<script src="https://cdn.jsdelivr.net/npm/@emailjs/browser@3/dist/email.min.js"></script>
```

### 6단계: 테스트

1. 웹사이트 새로고침
2. 문의 폼 작성
3. "문의하기" 클릭
4. gmlehddl95@gmail.com으로 이메일 도착 확인!

---

## 💰 EmailJS 무료 티어

- ✅ 200 이메일/월
- ✅ 충분한 양 (소규모 서비스)
- ⚠️ 초과 시: 유료 플랜 ($7/월, 1000 이메일)

---

## 📋 현재 설정 (mailto)

지금은 간단한 mailto 방식을 사용 중입니다.
바로 사용 가능하며, 대부분의 경우 충분합니다.

더 프로페셔널한 경험을 원하시면 위의 EmailJS 설정을 진행하세요!

