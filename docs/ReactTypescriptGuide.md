
# React + TypeScript 코드 품질 규칙 (.mdc)

이 문서는 React + TypeScript 기반 프로젝트에서 Cursor 룰 정의 및 팀 협업을 위한 실전 코드 품질 규칙입니다. 
다음 기준을 바탕으로 작성되었습니다:

- @Frontend Fundamentals
- Cursor 및 Windsurf 기반 코드 컨벤션
- 실무 적용 예시 기반 가이드
- React, TypeScript, Zustand, ESLint, Prettier 등 통합

## 목차

1. 명명 규칙
2. 조건문 처리
3. 상태 관리
4. 들여쓰기 및 코드 스타일
5. 가드 조건 / 분기 최소화
6. 함수 분리 / 중첩 제한
7. 컴포넌트 크기 및 책임 제한
8. Props Drilling 방지
9. 추상화 기준
10. React 훅 사용 가이드
11. 전역 유틸 / 상태 관리 분리 기준
12. 디렉토리 구조 및 모듈화

---

## 1. 명명 규칙

- **폴더:** 소문자 + 복수형 (`components`, `hooks`, `utils`)
- **파일:** `kebab-case.ts(x)`
- **React 컴포넌트:** 파일은 kebab-case, 컴포넌트 이름은 PascalCase
- **변수/함수:** camelCase
- **상수:** 대문자 스네이크_CASE
- **일관된 용어 사용:** `fetchUser`, `loadUser` 등 혼용 금지
- **외부 함수와 충돌 피하기:** 라이브러리 함수명과 중복 피함

```ts
// ❌ http 이름이 중복되어 충돌
import { http } from "@some-lib/http";
export function http() { return http.get("/api"); }

// ✅ 이름 구체화
import { http as httpLib } from "@some-lib/http";
export const httpService = {
  async getWithAuth(url: string) {
    const token = await getToken();
    return httpLib.get(url, { headers: { Authorization: `Bearer ${token}` } });
  }
}
```

---

## 2. 조건문 처리

- **복잡한 조건은 분해 후 명명**
- **삼항 연산자 중첩 금지**
- **논리 연산자는 의도 명확히**

```ts
// ❌ 복잡한 조건
const result = a && b ? "AB" : a ? "A" : b ? "B" : "None";

// ✅ 가독성 개선
function getResult() {
  if (a && b) return "AB";
  if (a) return "A";
  if (b) return "B";
  return "None";
}
```

---

## 3. 상태 관리

- **불변성 유지:** 객체/배열 직접 수정 금지
- **상태 최소화:** 파생값은 필요 시 계산
- **useReducer 사용 시 상태 응집화**
- **Immer는 상황에 따라 선택**

```ts
// ✅ 상태 불변성 유지 예시
setUser(prev => ({ ...prev, age: prev.age + 1 }));
```

---

## 4. 들여쓰기 및 코드 스타일

- **Prettier, ESLint 통합**
- **2-space indent**
- **최대 줄 길이 80~100자**
- **불필요한 중첩 줄이기**

---

## 5. 가드 조건 / 분기 최소화

- **early return 적극 활용**
- **불필요한 if/else 중첩 제거**
- **역할별 컴포넌트 분리**

```tsx
// ✅ 권한별 분리 예시
return isViewer ? <ViewerButton /> : <AdminButton />;
```

---

## 6. 함수 분리 / 중첩 제한

- **50줄 이상이면 리팩토링 고려**
- **useEffect 과다한 로직 분리**
- **복잡한 로직 → 훅/헬퍼 함수로 분리**

---

## 7. 컴포넌트 크기 및 책임 제한

- **한 파일에 하나의 책임**
- **Container / Presentational 분리**
- **하위 UI는 컴포넌트로 추출**

---

## 8. Props Drilling 방지

- **중간 컴포넌트를 거치지 말고 구조 변경**
- **Context API 또는 Composition 활용**

```tsx
// ✅ Context API 사용
const ctx = useContext(UserContext);
```

---

## 9. 추상화 기준

- **세 번 이상 반복 시 추상화 고려**
- **지나친 DRY는 오히려 해악**
- **단순 공통 로직은 유틸 함수로 분리**

---

## 10. React 훅 사용 가이드

### useEffect
- 사이드 이펙트 처리 전용
- 의존성 배열 철저히 관리
- cleanup 함수 필수

### useCallback
- 리렌더 최적화 목적
- props로 콜백 넘길 때만 사용
- 남용 지양

### useMemo
- 계산량 큰 연산만 메모이제이션
- 간단한 연산에는 사용 금지

### useRef
- DOM 참조 또는 렌더에 영향 없는 값 저장

---

## 11. 전역 유틸 / 상태 관리 분리 기준

- **utils/**: 공통 유틸 함수
- **constants/**: 전역 상수 (API, 권한 등)
- **stores/**: Context, Zustand 등 상태 모듈

```ts
// utils/format.ts
export const formatPrice = (price: number) =>
  new Intl.NumberFormat("ko-KR").format(price) + "원";
```

---

## 12. 디렉토리 구조 및 모듈화

```bash
src/
├── components/       # 공용 UI
├── hooks/            # 공용 훅
├── utils/            # 공용 유틸 함수
├── constants/        # 상수
├── stores/           # 전역 상태
├── modules/
│   ├── auth/
│   │   ├── components/
│   │   ├── hooks/
│   │   ├── utils/
│   │   ├── stores/
│   │   └── index.ts
│   └── onboarding/
│       ├── components/
│       ├── utils/
│       └── ...
├── app/ or pages/
└── App.tsx / main.tsx
```

---

## Import 정렬 순서 (권장)

1. 외부 라이브러리
2. `@/` alias
3. 상대 경로

```ts
import React from "react";
import { Button } from "@/components";
import { useAuth } from "@/modules/auth/hooks";
import styles from "./LoginForm.module.css";
```

---

📚 이 문서는 Cursor 룰 기반으로 구성되었으며, 프로젝트 코드 스타일, 디렉토리 구조, 상태 관리, 훅 사용 기준 등을 포함한 실무 중심 규칙집입니다.
