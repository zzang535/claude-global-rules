# CLAUDE.md

이 파일은 Claude Code (claude.ai/code)가 이 저장소에서 작업할 때 참조하는 가이드라인입니다.

## 프로젝트 개요

VivusCardio Connect는 SvelteKit으로 구축된 프론트엔드 애플리케이션입니다. CPR/AED 인증 및 준수를 위한 교육 관리 시스템입니다.

## 개발 명령어

### 서버 시작

- `npm run dev` - 개발 환경에 연결된 개발 서버 시작
- `npm run dev-token` - 디자인 토큰 생성 후 개발 서버 시작
- `npm run beta` - 베타 서버에 연결 (VITE_BASE_URL 환경 변수 필요)

### 빌드 및 배포

- `npm run build` - 프로덕션 애플리케이션 빌드
- `npm run preview` - 프로덕션 빌드 미리보기
- `npm run release` - package.json의 버전 업데이트 및 버전 업데이트 트리거

### 디자인 시스템

- `npm run generate:tokens` - Style Dictionary에서 디자인 토큰 생성
- `npm run generate:typography` - 디자인 토큰에서 타이포그래피 CSS 생성

### 테스트

- `npm test` - Playwright 통합 테스트 실행
- `npm run test:ui` - UI 모드로 Playwright 테스트 실행

애플리케이션은 `src/tests/integration/`에 위치한 Playwright를 사용하여 End-to-End 테스트를 수행합니다.

## 아키텍처 개요

### 프레임워크 및 기술 스택

- **프론트엔드**: SvelteKit with Vite
- **스타일링**: TailwindCSS + SCSS, 레거시 컴포넌트용 Bootstrap
- **차트**: 데이터 시각화를 위한 Chart.js 플러그인
- **상태 관리**: `src/lib/store/`의 Svelte 스토어
- **UI 컴포넌트**: `tokens/`의 토큰을 사용한 커스텀 디자인 시스템
- **인증**: Firebase + Google OAuth
- **테스트**: End-to-End 테스트용 Playwright

### 주요 디렉토리 구조

- `src/api/` - 도메인별로 구성된 API 통합 레이어
- `src/components/` - 기능별로 구성된 재사용 가능한 UI 컴포넌트
- `src/lib/` - 유틸리티, 스토어, 상수 및 공유 서비스
- `src/routes/` - 파일 기반 라우팅을 사용하는 SvelteKit 페이지
- `src/tests/` - Playwright 통합 테스트
- `tokens/` - 디자인 시스템 토큰 및 생성 스크립트

### 컴포넌트 구성

컴포넌트는 기능 도메인별로 구성되어 있습니다:

- `certification/` - 인증서 생성 및 관리
- `course-builder/` - 교육 과정 생성 및 편집
- `training-records/` - CPR 성능 지표 및 점수
- `organization/` - 라이선스 관리 및 조직 계층
- `ui/` - 재사용 가능한 UI 기본 요소 (버튼, 모달, 테이블 등)

### 데이터 플로우 아키텍처

- API 호출은 `src/api/` 모듈에서 도메인별로 중앙화
- Svelte 스토어가 반응형 업데이트로 애플리케이션 상태 관리
- 컴포넌트는 공유 상태에 스토어를, 로컬 데이터에 props 사용
- Chart.js 컴포넌트가 CPR 지표의 복잡한 데이터 시각화 처리

### 디자인 시스템

프로젝트는 토큰 기반 디자인 시스템을 사용합니다:

- `tokens/design-tokens.json`에 정의된 디자인 토큰
- Style Dictionary를 통해 생성된 CSS 변수
- `src/lib/ds/_variables.scss`의 SCSS 변수
- 유틸리티 클래스용 TailwindCSS

### 환경 설정

- **Dev**: 별도 데이터베이스를 사용하는 개발 환경
- **Beta**: 프로덕션 데이터베이스를 공유하는 베타 환경
- **Prod**: 프로덕션 환경
- `VITE_BASE_URL` 환경 변수로 환경 전환 처리

### 주요 기능

- CPR/AED 교육 과정 관리
- 실시간 성능 지표 및 점수
- 커스터마이징 가능한 템플릿을 사용한 인증서 생성
- 멀티 테넌트 조직 및 라이선스 관리
- SDL (Self-Directed Learning) 과정 플로우
- 상세 분석 및 차트가 포함된 교육 기록

### 경로 별칭

`svelte.config.js`에 구성된 별칭들:

- `$api` → `./src/api`
- `$routes` → `./src/routes`
- `$components` → `./src/components`
- `$utils` → `./src/utils`
- `$store` → `./src/lib/store`
- `$lib` → `./src/lib`
- `$tests` → `./src/tests`

<!-- 여기부터는 개발시 지켜야할 규칙이나 가이드라인 입니다. -->

## 테스트 콘솔 가이드라인

테스트 작성 시 명확성을 위해 테스트당 약 5개의 콘솔 로그를 포함하세요:

1. 테스트 초기화
2. 주요 동작 (네비게이션, 클릭)
3. 상태 변화 (요소 대기)
4. 검증
5. 테스트 완료

이는 더 나은 디버깅과 테스트 투명성을 위한 `.cursor/rules/test-logging-guidelines.mdc`의 가이드라인을 따릅니다.

## 🚨 테스트 Selector 규칙 (중요!)

- **필수**: 모든 테스트에서는 `data-testid` 속성을 사용하여 요소를 선택합니다
- **금지**: CSS 클래스, 태그명, 텍스트 내용, role 속성 등 범용 selector 사용 금지
- **이유**: 스타일링이나 텍스트 변경에 영향받지 않는 안정적인 테스트를 위함

**올바른 예시:**

```javascript
// ✅ data-testid 사용
const editButton = page.getByTestId("org-info-edit-button");
const manageButton = page.getByTestId("org-info-manage-button");
const modalTitle = page.getByTestId("modal-title");
const modalContainer = page.getByTestId("license-manage-modal");
```

**잘못된 예시:**

```javascript
// ❌ CSS 클래스 사용
const editButton = page.locator('svg[class*="cursor-pointer"]');
// ❌ 텍스트 내용으로 선택
const manageButton = page.locator('button:has-text("Manage")');
// ❌ 범용 클래스 사용
const usedLabel = page.locator('[class*="text-[20px]"]');
// ❌ role 속성이나 태그명 사용
const modalTitle = page.locator('[role="dialog"] h2, [class*="modal"] h2, h2');
```

## Import 사용 가이드라인

- **필수**: 모든 import 구문은 절대 경로(별칭) 사용을 원칙으로 합니다
- **금지**: 상대 경로(`../../`, `../`) 사용 금지
- **이유**: 파일 이동 시에도 import 경로 수정이 불필요하고 가독성이 향상됩니다

### Import 순서 가이드라인

**모든 import 구문은 다음 순서로 정렬하여 가독성을 향상시킵니다:**

1. **외부 모듈** (npm 패키지)
2. **Svelte import** (`svelte` 관련)
3. **Svelte 내장 모듈 import** (`$app`, `$env` 등)
4. **API** (`$api`)
5. **Store** (`$store`)
6. **Lib** (`$lib`)
7. **Utils** (`$utils`)
8. **Component** (`$components`)

**올바른 예시:**

```javascript
// 1. 외부 모듈
import Chart from "chart.js/auto";
import { toast } from "svelte-sonner";

// 2. Svelte import
import { onMount } from "svelte";

// 3. Svelte 내장 모듈
import { page } from "$app/stores";
import { env } from "$env/dynamic/public";

// 4. API
import { organizationApi } from "$api/organization";
import { userApi } from "$api/user";

// 5. Store
import { UserStore } from "$store/userStore";
import { OrganizationStore } from "$store/organizationStore";

// 6. Lib
import { dateFormat } from "$lib/utils/date";
import { validation } from "$lib/validation";

// 7. Utils
import { debounce } from "$utils/debounce";
import { formatCurrency } from "$utils/format";

// 8. Component
import Button from "$components/ui/Button.svelte";
import Modal from "$components/ui/Modal.svelte";
```

**⚠️ Playwright 테스트 파일 예외**

Playwright 테스트 파일(`*.test.js`)에서는 SvelteKit의 경로 별칭을 인식하지 못하므로 **상대 경로를 사용해야 합니다**.

**일반 소스 코드 (올바른 예시):**

```javascript
// ✅ 절대 경로 별칭 사용
import { login } from "$tests/helper/testPageHelpers.js";
import { UserStore } from "$store/userStore.js";
import { Button } from "$components/ui/Button.svelte";
```

**Playwright 테스트 파일 (올바른 예시):**

```javascript
// ✅ Playwright 테스트에서는 상대 경로 사용
import { login } from "../../helper/testPageHelpers.js";
import { seedNotEnoughLicenseScenario } from "../../setup/serviceSeed.js";
```

**잘못된 예시:**

```javascript
// ❌ 일반 소스 코드에서 상대 경로 사용
import { loginCustom } from "../../helper/testPageHelpers.js";
import { UserStore } from "../../../lib/store/userStore.js";
import { Button } from "../../../../components/ui/Button.svelte";

// ❌ Playwright 테스트에서 별칭 사용 (인식 불가)
import { login } from "$tests/helper/testPageHelpers.js";
```

## Deprecated 컴포넌트 처리 가이드라인

### 기본 원칙

더 이상 사용하지 않는 컴포넌트나 파일은 삭제하지 말고 deprecated 처리하여 보관합니다.

### 처리 절차

#### 1. 폴더 구조

- 현재 경로 내에 `deprecated` 폴더가 있으면 해당 폴더로 이동
- `deprecated` 폴더가 없으면 새로 생성 후 이동

#### 2. 파일명 변경 규칙

- 기존 파일명에 현재 package.json의 버전을 추가
- 형식: `{원본파일명}_{버전}.{확장자}`
- 버전 형식: `v{major}_{minor}_{patch}` (점을 언더스코어로 변경)

**예시:**

- `OrganizationDetail.svelte` → `OrganizationDetail_v1_2_2.svelte`
- `organization.js` → `organization_v1_2_2.js`

#### 3. Import 경로 수정

- 기존 파일을 import하는 모든 곳에서 새로운 deprecated 경로로 수정
- deprecated된 파일 내부의 import도 필요시 수정

#### 4. 폴더 생성 예시

```bash
mkdir -p {현재_경로}/deprecated
```

#### 5. 파일 이동 예시

```bash
mv {원본파일} {현재_경로}/deprecated/{원본파일명}_v{major}_{minor}_{patch}.{확장자}
```

### 주의사항

- deprecated 처리 전에 해당 파일을 사용하는 모든 곳을 확인
- import 경로 수정 후 빌드 오류가 없는지 확인
- deprecated된 파일들은 향후 참조나 롤백 목적으로 보관

## 피그마 디자인 시안 해석 규칙

### 기본 원칙

피그마 시안을 Svelte 컴포넌트로 구현할 때 다음 규칙을 따릅니다.

### HTML 태그 선택 가이드라인

#### 텍스트 요소

- **제목이나 독립적인 텍스트**: `<span>` 사용 (기본 마진 방지)
- **단순 텍스트나 인라인 요소**: `<span>` 사용

#### 높이와 정렬

- **피그마에서 명시된 높이**: 정확히 해당 높이(예: `h-[24px]`) 적용
- **중앙 정렬**: `flex items-center` 또는 `leading-[높이값]` 사용
- **라인 높이**: 피그마 시안의 높이와 동일하게 설정 (예: `leading-[24px]`)

#### 레이아웃

- **컨테이너 높이**: 피그마 시안의 정확한 높이값 사용
- **패딩과 마진**: 피그마 시안의 간격을 정확히 측정하여 적용
- **플렉스 정렬**: 시안의 정렬 방식에 따라 `items-center`, `justify-between` 등 사용

### 예시

**올바른 구현:**

```html
<!-- 24px 높이 영역에 텍스트 중앙 정렬 -->
<div class="h-[64px] flex items-center justify-between">
  <span
    class="text-[20px] font-semibold leading-[24px] h-[24px] flex items-center"
  >
    Title Text
  </span>
  <span class="text-[14px] font-medium leading-[24px]"> Button Text </span>
</div>
```

**피해야 할 구현:**

```html
<!-- 기본 마진이 들어가는 p 태그 사용 -->
<div class="flex items-center justify-between">
  <p class="text-[20px] font-semibold">Title Text</p>
  <p class="text-[14px] font-medium">Button Text</p>
</div>
```
