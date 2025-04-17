# 📱 Expo 기반 React Native 코드 컨벤션

## ✅ 폴더 구조

```
project-root/
├── assets/            # 이미지, 폰트, 사운드 등 정적 자원
├── components/        # 재사용 가능한 컴포넌트
├── screens/           # 페이지 컴포넌트
├── navigation/        # 네비게이션 설정
├── hooks/             # 커스텀 훅
├── stores/            # 상태 관리 (Zustand 등)
├── apis/              # API 요청 모듈
├── utils/             # 유틸리티 함수
├── constants/         # 상수 정의 (색상, 폰트 등)
├── App.js             # 앱 진입점
└── app.json           # Expo 설정 파일
```

---

## ✅ 코드 스타일

- 세미콜론 사용 ❌
- 탭 간격: 2 spaces
- 문자열: " " 사용
- 파일명: `PascalCase.jsx` 또는 `.js`
- 스타일은 `StyleSheet.create()` 사용
- 공통 스타일은 `styles/common.js` 등에 분리

```js
const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 16,
  },
})
```

---

## ✅ 컴포넌트 작성 규칙

- 함수형 컴포넌트 사용
- `useEffect`, `useState`는 최소화하고 커스텀 훅으로 분리
- props는 구조 분해 할당으로 받기

```jsx
function LoginButton({ onPress }) {
  return (
    <TouchableOpacity onPress={onPress}>
      <Text>로그인</Text>
    </TouchableOpacity>
  )
}
```

---

## ✅ 네이밍 규칙

| 타입       | 규칙          | 예시               |
|------------|---------------|--------------------|
| 폴더/파일  | kebab-case    | login-button.js    |
| 컴포넌트   | PascalCase    | LoginButton        |
| 함수/변수  | camelCase     | handleLoginPress   |
| 상수       | UPPER_SNAKE   | BASE_URL           |

---

## ✅ API 연동

- `apis/axiosInstance.js`로 axios 인스턴스 생성
- 각 기능별 API 모듈 분리 (`apis/auth.js`, `apis/user.js` 등)

```js
// apis/auth.js
import axios from './axiosInstance'

export const login = (data) => axios.post('/auth/login', data)
```

---

## ✅ 필수 설정 파일 예시

```jsonc
// .prettierrc
{
  "singleQuote": true,
  "semi": false,
  "tabWidth": 2,
  "trailingComma": "es5"
}
```

```js
// .eslintrc.js
module.exports = {
  extends: ["expo", "plugin:prettier/recommended"],
}
```

---

## ✅ 테스트 (선택 사항)

- `jest`, `react-native-testing-library` 사용
- 사용자 시나리오 중심 테스트 권장
- 파일명: `컴포넌트명.test.js`
