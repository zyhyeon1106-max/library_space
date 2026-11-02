# 학교도서관 공간 구성 의견 수렴 사이트

## 화면
- 학생 / 학부모 / 교사: 탭을 구분한 실명 의견 제출 (만족·불만족·희망)
- 사서교사: 이메일/비밀번호 로그인, 집단별 응답 수, 빈출 주제 TOP5, 개인별 원문, 검색, CSV 추출, 기본 관계 노드
- 응답은 Cloud Firestore에 저장됩니다. 응답자끼리 서로의 응답을 읽을 수 없도록 보안 규칙으로 제한합니다.

## 준비물
Firebase 프로젝트, Firebase Authentication, Cloud Firestore, Firebase Hosting, Node.js 및 Firebase CLI(배포용).

## 배포 순서
1. https://console.firebase.google.com/ 에서 프로젝트 생성 → 웹 앱 등록 → 설정 객체 확인
2. `public/js/config.js`에서 `REPLACE_ME` 4곳을 웹 앱 구성 값으로 바꾸세요. 이 값은 공개 가능한 클라이언트 설정입니다.
3. Authentication > Sign-in method에서 **Anonymous** 와 **Email/Password** 활성화.
4. Authentication > Users에서 담당자용 이메일/비밀번호 계정을 생성. 이 계정의 **UID**를 복사하세요. 비밀번호를 코드에 적지 마세요.
5. `firestore.rules`의 `REPLACE_WITH_ADMIN_UID`를 위 UID로 변경합니다. **절대로 응답 전체에 공개 읽기 권한을 주지 마세요.**
6. Cloud Firestore 데이터베이스를 생성합니다. 보안 규칙 파일을 배포하기 전에 실제 이름 데이터를 수집하지 마세요.
7. 터미널에서 `npm install -g firebase-tools`, `firebase login`, `firebase use --add`(프로젝트 선택) 실행.
8. `firebase deploy --only hosting,firestore:rules` 실행. 첫 배포 전에 `firebase init hosting firestore`를 사용해도 되지만, 이 폴더에는 기본 `firebase.json`이 포함되어 있습니다.
9. Firebase Console의 Authentication > Settings > Authorized domains에서 배포 도메인을 확인하세요.
10. 담당자 계정으로 로그인해 데이터 열람을 테스트하고, 학생/학부모 화면에서 타인의 응답을 조회할 수 없는지 규칙 테스트를 수행하세요.

## 안내 및 주의
- 개인정보: **실명과 자유 의견**을 저장하므로, 학생/학부모 대상 수집 전 학교의 개인정보 처리방침 및 관계 법령에 맞는 적법한 고지·동의(필요시 보호자 동의)를 준비하세요. `public/index.html`의 개인정보 안내에 보관 기간, 삭제 요청 창구, 개인정보 처리자 등을 실제 학교 기준으로 기입해야 합니다. 미확정 상태로 실제 운영하지 마세요.
- 학교급 선택은 학생에서만 표시됩니다. 학급/학생 식별번호는 수집하지 않습니다.
- 응답자 계정은 익명 Firebase 인증(기술적 세션)으로만 이용합니다. 이는 **설문 응답의 실명 수집**과 별개입니다.
- 본 MVP에는 전용 제출 속도 제한·봇 방어·관리자 다중 계정·삭제 요청 처리·중복 제출 차단·조직별 다중 학교 분리 기능이 없습니다. 외부 공개 전 App Check 등 남용 방지, 삭제 처리 및 운영 통제를 추가하세요.
- 관리자 UID 1명만 열람 허용되며, Firestore 규칙은 앱 UI가 아니라 서버 측 데이터 접근을 통제합니다.
- TOP5는 정규표현식 기반 분류이며 동일 응답이 여러 주제에 포함될 수 있습니다. 뜻이 다른 표현의 오분류가 발생하므로 교사가 원문을 검토해야 합니다.
- '네트워크' 영역은 빈출 주제 노드의 기초 표현입니다. 이해관계자 간 대립/지원 연결선이 검증된 ANT 분석 결과는 아직 아닙니다.
- Borich/IPA, 자동 평면 배치, 실제 예산 시뮬레이션, 교육청 기준 검증, 집단 가중치 분석은 구현되지 않았습니다. 주관식 3문항만으로 Borich/IPA를 계산하지 않습니다.
- CDN에서 Firebase SDK를 로드하므로 인터넷이 필요합니다. `file://` 방식이 아닌 Hosting이나 개발용 로컬 HTTP 서버에서 실행하세요.

## 폴더
```
school_library_space_site/
  public/
    index.html
    styles.css
    js/
      config.js
      app.js
  firestore.rules
  firebase.json
  .firebaserc.example
  README.md
```
