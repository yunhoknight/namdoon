# React + TypeScript + Vite

This template provides a minimal setup to get React working in Vite with HMR and some ESLint rules.

Currently, two official plugins are available:

- [@vitejs/plugin-react](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react) uses [Oxc](https://oxc.rs)
- [@vitejs/plugin-react-swc](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react-swc) uses [SWC](https://swc.rs/)

## React Compiler

The React Compiler is not enabled on this template because of its impact on dev & build performances. To add it, see [this documentation](https://react.dev/learn/react-compiler/installation).

## Expanding the ESLint configuration

If you are developing a production application, we recommend updating the configuration to enable type-aware lint rules:

```js
export default defineConfig([
  globalIgnores(['dist']),
  {
    files: ['**/*.{ts,tsx}'],
    extends: [
      // Other configs...

      // Remove tseslint.configs.recommended and replace with this
      tseslint.configs.recommendedTypeChecked,
      // Alternatively, use this for stricter rules
      tseslint.configs.strictTypeChecked,
      // Optionally, add this for stylistic rules
      tseslint.configs.stylisticTypeChecked,

      // Other configs...
    ],
    languageOptions: {
      parserOptions: {
        project: ['./tsconfig.node.json', './tsconfig.app.json'],
        tsconfigRootDir: import.meta.dirname,
      },
      // other options...
    },
  },
])

```

You can also install [eslint-plugin-react-x](https://npmx.dev/package/eslint-plugin-react-x) and [eslint-plugin-react-dom](https://npmx.dev/package/eslint-plugin-react-dom) for React-specific lint rules:

```js
// eslint.config.js
import reactX from 'eslint-plugin-react-x'
import reactDom from 'eslint-plugin-react-dom'

export default defineConfig([
  globalIgnores(['dist']),
  {
    files: ['**/*.{ts,tsx}'],
    extends: [
      // Other configs...
      // Enable lint rules for React
      reactX.configs['recommended-typescript'],
      // Enable lint rules for React DOM
      reactDom.configs.recommended,
    ],
    languageOptions: {
      parserOptions: {
        project: ['./tsconfig.node.json', './tsconfig.app.json'],
        tsconfigRootDir: import.meta.dirname,
      },
      // other options...
    },
  },
])

```
# Namdo-On Frontend Module

React, TypeScript, Vite를 기반으로 구축된 `namdo-on` 서비스의 웹 프론트엔드 모듈입니다.

---

## 🛠 기술 스택 (Tech Stack)
* **Core**: React 18, TypeScript
* **Build Tool**: Vite
* **Linter**: ESLint
* **Package Manager**: npm

---

## 🚀 시작하기 (Quick Start)

### 1. 패키지 설치
```bash
cd frontend
npm install
```

### 2. 개발 서버 실행
```bash
npm run dev
```
* 기본 접속 주소: `http://localhost:5173`

### 3. 주요 스크립트
| 명령어 | 설명 |
| :--- | :--- |
| `npm run dev` | 로컬 HMR 개발 서버를 실행합니다. |
| `npm run build` | TypeScript 검사 후 `dist/` 폴더에 프로덕션 빌드합니다. |
| `npm run lint` | ESLint로 정적 코드 검사를 실행합니다. |
| `npm run preview` | 빌드된 정적 결과물을 로컬에서 미리 확인합니다. |

---

## 📂 폴더 구조
```text
frontend/
├── public/           # 파비콘, 정적 아이콘 등
├── src/
│   ├── assets/       # static 이미지 및 미디어 파일
│   ├── App.tsx       # 메인 애플리케이션 컴포넌트
│   ├── main.tsx      # Entry point (DOM 렌더링)
│   └── index.css     # 전역(Global) 스타일
├── index.html        # HTML 템플릿
├── eslint.config.js  # ESLint 설정
├── package.json      # 의존성 패키지 및 실행 스크립트
└── vite.config.ts    # Vite 번들러 설정
```

---

## ⚙️ 환경 변수 (`.env`)
`frontend` 폴더 루트 위치에 `.env` 파일을 작성하여 API 서버 주소를 연결합니다.

```env
VITE_API_BASE_URL=http://localhost:8080
VITE_AI_BASE_URL=http://localhost:8000
```