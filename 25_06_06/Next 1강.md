# Next 1강

Status: Done
Due date: 06/06/2025
Task type: 👀 강의
세부사항: 기술적인 배경, 개발 역사
Updated at: 2025년 6월 7일 오후 6:09

## 상세설명

Next공부를 통해서 클라이언트 사이드 렌더링과, 서버사이드 렌더링을 이해할 수 있다. 또한 Next로 build하는 이유를 알 수 있다. 

## 세부적으로 할 일

- [x]  기술적인 배경 이해하기
- [x]  review

## 참고자료

[https://www.youtube.com/watch?v=EwY6hbAxdV8&list=PLpq56DBY9U2AyFtF0ajuFZX3IGgDIXgcb](https://www.youtube.com/watch?v=EwY6hbAxdV8&list=PLpq56DBY9U2AyFtF0ajuFZX3IGgDIXgcb)

review ⇒

## 1. 전통적 서버 사이드 렌더링(SSR) 배경

- **JSP, PHP, ASP 등 템플릿 엔진**
    - 서버가 HTML 템플릿에 데이터를 주입해 완성된 HTML을 클라이언트에 보냄
    - 예: 로그인 폼 전송 → 서버에서 `loginResult.jsp`를 렌더링 → HTML 응답
- **장점**
    - 초기 화면이 완전한 HTML 형태라 SEO(검색엔진 최적화)에 유리
    - 자바스크립트 엔진 의존성이 적어 오래된 브라우저에서도 동작
- **단점**
    - 사용자가 페이지 이동할 때마다 전체 HTML을 다시 받으므로 UX(반응성)가 떨어짐
    - 서버 로직과 프레젠테이션 로직이 뒤섞여 유지보수 복잡도 증가

---

## 2. AJAX의 등장과 CSR 시대로의 전환

- **AJAX(Asynchronous JavaScript And XML)**
    - 페이지 전체가 아니라 데이터만 비동기 요청 → 화면 일부만 갱신
    - 클라이언트에서 로딩 스피너 표시, 부분 업데이트 가능
- **CSR(Client-Side Rendering)**
    - 브라우저에 번들된 자바스크립트가 실행되어 DOM을 생성/갱신
    - 서버는 빈 `<div id="root"></div>` 같은 최소한의 HTML과 번들 스크립트만 전달
- **CSR 장점**
    - 사용자와의 인터랙션이 빠르고 부드러움
    - 화면 전환 시 전체 페이지 리로드가 없어 네이티브 앱 같은 경험
- **CSR 단점**
    1. **퍼포먼스 문제**
        - 번들 크기가 커질수록 초기 로드(white screen) 지연
        - 사용자 장치 사양에 따라 느리게 동작할 수 있음
    2. **SEO 한계**
        - 검색엔진 봇이 자바스크립트를 다 실행하지 못하면 콘텐츠 인식 불가

---

## 3. SSR의 부활과 Next.js 등장

- **문제 해결을 위한 SSR(Server-Side Rendering)**
    - 요청 시점에 서버가 React 컴포넌트를 렌더링해 HTML을 생성
    - 초기 로드에 완성된 HTML을 전달 → 빠른 첫 화면 렌더링 & SEO 확보
- **단순 SSR vs. Next.js SSR**
    - 과거 JSP처럼 서버 전체가 렌더링 로직을 담당하는 것이 아니라
    - **Next.js**는 React 생태계에 맞춰 SSR 과정을 추상화
        - `getServerSideProps` 같은 인터페이스로 데이터 패칭
        - 개발자는 서버 구성 없이 함수만 작성하면 Next가 알아서 처리
- **Next.js가 해결한 과제**
    1. **구성(Configuration) 자동화**
        - Webpack, Babel 설정 불필요
        - 파일 기반 라우팅, 코드 분할(Code Splitting) 내장
    2. **하이브리드 렌더링**
        - SSR, SSG(Static Site Generation), CSR을 페이지 단위로 선택 가능
    3. **개발자 경험(DX) 향상**
        - 자동 리로딩, 타입스크립트 지원, 에러 오버레이

---

## 4. Next.js 주요 기능과 흐름

| 기능 | 설명 |
| --- | --- |
| **파일 기반 라우팅** | `pages/` 또는 `app/` 폴더 구조에 따라 자동으로 라우트 생성 |
| **SSG** | `getStaticProps` → 빌드 시 HTML 생성, CDN에 캐싱 (빠른 응답) |
| **ISR(증분 정적 생성)** | `revalidate` 옵션으로 일정 주기마다 정적 페이지를 재생성 |
| **SSR** | `getServerSideProps` → 요청마다 서버에서 HTML 생성 (실시간 데이터) |
| **CSR** | `useEffect` 등 클라이언트에서만 처리해야 할 로직 |
| **API Routes** | `/api` 디렉터리에 함수 작성 → 서버리스(Function) 형태로 배포 가능 |
| **이미지 최적화** | `next/image` 컴포넌트로 자동 리사이징, 포맷 변환 |
| **미들웨어 & Edge** | 요청 레벨에서 미들웨어 실행, 글로벌 설정, 엣지 함수 지원 |
| **환경 변수 자동 주입** | 빌드 · 실행 환경별로 `.env.local` 등 관리 |

---

## 5. Next.js로 “빌드(build)”하는 이유

1. **성능 최적화**
    - 필요한 코드만 분할하여 로딩 (Dynamic Import)
    - SSG / ISR을 통해 CDN 캐싱 활용
2. **SEO 확보**
    - SSR/SSG가 초기 HTML을 제공 → 검색엔진 봇 색인 가능
3. **개발 생산성**
    - 복잡한 번들러·트랜스파일러 설정 생략
    - 파일 구조만으로 라우팅·코드 분할 자동화
4. **풀스택 웹앱**
    - API Routes를 통해 백엔드 엔드포인트 간편 구현
    - 미들웨어로 인증·권한·리디렉션 처리

---

## 6. 추가로 알아두면 좋은 개념

- **Hydration**
    - 서버에서 렌더된 HTML에 React를 연결해(“hydrate”) 클라이언트 인터랙션 활성화
- **Client Component vs Server Component** (Next.js 13+ / App Router)
    - `use client` 지시자를 통해 클라이언트 전용 컴포넌트 구분
    - 불필요한 번들 크기 최소화
- **Edge Runtime**
    - V8 기반 엣지 환경에서 API Routes나 미들웨어 실행 → 레이턴시 감소

---

### 요약

1. 과거 JSP 같은 SSR만으론 UX와 개발 생산성에 한계가 있었고,
2. AJAX → CSR이 등장해 인터랙티브한 UI를 가능케 했으나 성능·SEO 이슈를 야기,
3. Next.js는 React 생태계에서 SSR/SSG/ISR/CSR을 상황에 맞게 추상화하여
4. **빠른 초기 로드, SEO, 코드 분할, 풀스택 기능**을 모두 제공한다는 점이 핵심입니다.