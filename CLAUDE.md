# 전역 규칙

## 🚨 Git 명령어 실행 규칙 (매우 중요!)

**Claude는 사용자가 직접 명령하기 전까지 절대로 git 명령어를 자동으로 실행하지 않습니다.**

### 금지된 자동 실행
- ❌ `git add` 자동 실행 금지
- ❌ `git commit` 자동 실행 금지
- ❌ `git push` 자동 실행 금지
- ❌ 기타 모든 git 관련 명령어 자동 실행 금지

### 올바른 동작
- ✅ 코드 변경 후 변경 사항을 설명할 뿐
- ✅ `git status` 또는 `git diff` 등 조회 명령은 필요시 실행 가능
- ✅ **사용자가 `add commit push` 또는 명시적으로 git 명령을 내릴 때만 실행**
- ✅ 실행 전에 항상 사용자의 명확한 지시를 기다림

### 예시

**잘못된 동작:**
```bash
# ❌ 사용자가 요청하지 않았는데 자동 실행됨
$ git add src/components/MyComponent.svelte
$ git commit -m "feat: MyComponent 추가"
$ git push
```

**올바른 동작:**
코드를 수정한 후 사용자에게 변경 내용을 설명하고, 사용자가 다음과 같이 명령할 때만 실행:
```bash
사용자: "add commit push"
Claude: git add → git commit → git push 실행
```

## Git 커밋 메시지 규칙

- **언어**: 모든 커밋 메시지는 한국어로 작성해야 합니다
- **형식**: 적절한 경우 conventional commit 형식을 사용합니다 (`feat:`, `fix:`, `refactor:` 등)
- **내용**: 무엇이 변경되었고 왜 변경되었는지에 집중하여 개발자를 위해 작성합니다

### ⚠️ 중요: AI 관련 내용 금지

- **절대 포함하지 말 것**: Claude/AI 관련 attribution이나 생성 내용 알림
- **금지 예시**:
  - `🤖 Generated with [Claude Code](https://claude.ai/code)`
  - `Co-Authored-By: Claude <noreply@anthropic.com>`
  - 기타 모든 AI 도구 관련 언급
- **이유**: 커밋 메시지는 개발자를 위한 것이며, 도구나 AI 사용 여부는 불필요한 정보입니다

## 프로젝트별 규칙 적용

이 파일이 있는 디렉토리(`~/.claude/`)의 `rules/` 폴더에서 프로젝트명별 규칙을 관리합니다.

**적용 방법**:
1. Claude Code 실행 시 작업 중인 프로젝트의 경로 또는 프로젝트명을 확인합니다
2. `~/.claude/rules/[프로젝트명].md` 파일이 있으면 그 규칙을 추가로 적용합니다
3. 프로젝트별 규칙이 전역 규칙과 충돌하면 프로젝트별 규칙을 우선합니다

**예시**:
- 작업 중인 프로젝트: `vivuscardio-connect-training-simulator`
- 확인할 파일: `~/.claude/rules/vivuscardio-connect-training-simulator.md`
- 해당 파일이 있으면 그 안의 규칙을 함께 따릅니다
