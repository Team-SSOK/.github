## 1. 파일 및 폴더 구조, 그리고 Import 순서

### 폴더 구조

```bash
├── api/                       # Axios 인스턴스 및 API 전역 관리
│   └── ApiInstance.ts
├── app/                       # App Router 기반 페이지 구성 (Next.js style)
│   ├── __tests__/             # 테스트 파일 모음
│   ├── (tabs)/                # 탭 네비게이션 구성
│   ├── account/               # account 관련 라우팅
│   ├── auth/                  # auth 관련 라우팅
│   ├── transfer/              # transfer 관련 라우팅
│   ├── _layout.tsx            # App 레이아웃 정의
│   └── index.tsx              # 루트 페이지 엔트리
├── assets/                    # 이미지, 폰트 등 공통 정적 자산
├── components/                # 공통 UI 컴포넌트 모음
├── constants/                 # 상수 정의 (color.ts)
├── docs/                      # API 명세나 설계 문서
├── hooks/                     # 전역에서 재사용하는 커스텀 훅
├── mocks/                     # 목 데이터 (테스트용)
├── modules/                   # 기능 단위별 모듈
│   ├── account/
│   │   ├── api/              # account 전용 API 함수들
│   │   ├── components/       # account 전용 컴포넌트
│   │   ├── hooks/            # account 전용 훅
│   │   ├── store/            # account 전용 Zustand 상태
│   │   ├── utils/            # account 관련 유틸
│   │   ├── auth_api_spec.md  # API 문서
│   │   └── index.ts
│   ├── auth/
│   ├── bluetooth/
│   ├── onboarding/
│   ├── settings/
│   └── transfer/
├── node_modules/
├── stores/                    # 전역 Zustand store (slice 기반 분리 가능)
├── styles/                    # 전역 스타일
├── theme/                     # 색상, 폰트 등 테마 토큰
├── types/                     # 전역 TypeScript 타입 정의
├── utils/                     # 전역 유틸 함수 모음
├── .eslintrc.js               # ESLint 설정
├── .gitignore
├── .prettierrc                # Prettier 설정
├── app.json                   # Expo 설정
├── eas.json                   # Expo Application Services 설정
├── expo-env.d.ts              # 환경 타입 정의
├── package.json
├── package-lock.json
├── README.md
└── tsconfig.json              # TypeScript 설정


```

### 명명 규칙

- 폴더: 소문자, 복수형
- 파일: `kebab-case.ts(x)`
- 컴포넌트는 PascalCase export
- 테스트: `.test.tsx`

### 절대경로 및 Import 순서

- `@/components/...` 등으로 alias 사용 (`tsconfig.json`)
- 외부 라이브러리 → alias 내부 모듈 → 로컬 모듈 순
- `import/order` 린트 적용 권장

### export

- `index.ts` 배럴 파일 사용
- 순환참조 방지, export 범위 관리

## 2. TypeScript 타입 및 인터페이스 컨벤션

### 네이밍

- `UserProfile`, `PaymentMethod` 등 PascalCase
- `I`, `T` 접두사 지양

### interface vs type

- `interface`: 객체, API 구조
- `type`: 유니언, 함수 시그니처

### 타입 선언

- 함수, Props 등 명시
- 명백한 추론은 생략 가능
- `any` 대신 `unknown`, 타입 단언

## 3. 코드 구성 원칙 (Atomic Design, 모듈화, Co-location)

### Atomic Design

- Atoms: 버튼, 텍스트 등
- Molecules: 검색창 등
- Organisms: 패널, 내비게이션
- Templates/Pages: 레이아웃/라우트 구성

### 모듈화 & Co-location

- `modules/feature` 단위
- 내부에 컴포넌트, 훅, 상태 등 포함
- 공용은 상위 폴더로 올림

### 단일 책임(SRP) & 재사용

- 더 완벽한 단일 책임 원칙 적용을 위해서는 이들을 개별 파일로 분리합니다.
- UI/로직 분리 (컴포넌트/커스텀 훅)
- DRY 원칙, 과도한 추상화 지양
  
---

### 주석 & 문서화

- 필요한 곳에만 Why 중심 주석
- JSDoc 사용 권장

4. Axios 기반 API 구성 및 관리 Best Practices

### ✅ API 모듈 구조 설계

- `axios.create()`로 중앙 Axios 인스턴스를 생성
- `api/` 또는 `services/` 디렉토리에 API 함수 정리
- 환경 변수(`.env`)로 baseURL, API 키 분리 관리

### ✅ 모듈별 API 함수 분리

- `userService.ts`, `productService.ts` 등 도메인 기반 분리
- 도메인별 함수: `getUser()`, `updateUser()` 등으로 구성
- 코드 캡슐화 및 재사용성 향상

### ✅ 공통 인터셉터 처리

- 요청 시 인증 토큰 자동 추가
- 응답 에러 처리, 로깅 등 전역 처리
- 공통 로직은 인터셉터에서 통합 관리

### ✅ 오류 핸들링 전략

- 인터셉터에서 401, 500 등 공통 처리
- 개별 API에서는 필요할 때만 `try-catch`
- 공통 에러 메시지/재인증/로깅 처리 일원화

---

## Zustand 상태 관리 Best Practices

### ✅ 전역 vs 모듈별 상태 관리

- 전역 상태: `store/global.ts` 등
- 모듈별 상태: `modules/<feature>/store.ts`로 로컬 slice 구성
- slice 단위로 분리하여 가독성과 유지보수 향상

### ✅ Zustand 디렉토리 구조

- 전역 상태는 `store/`
- 기능별 상태는 각 `modules/` 내부에 co-location

### ✅ Middleware 활용 전략

- `selector`: 필요한 상태만 구독 → 렌더 최적화
- `persist`: AsyncStorage에 저장 필요 상태 보존
- `devtools`, `immer`: 디버깅 및 불변성 유지에 유용

### ✅ TypeScript 기반 타입 안전성 확보

- `interface State { ... }` + `create<State>()` 패턴 사용
- slice별 타입 분리 및 병합 가능
- 커스텀 훅으로 필요한 상태만 추출해서 제공

---
