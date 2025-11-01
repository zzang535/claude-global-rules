# 전역 규칙

## Git 명령어 실행 규칙
- add, commit, push 등 git 관련 명령어는 사용자가 직접 명령하기 전까지 자동으로 실행하지 않습니다.

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
- 현재 디렉토리를 확인합니다.
- `rules/[현재실행디렉토리명].md` 가 있는 경우 이 파일의 규칙을 추가로 적용합니다.
