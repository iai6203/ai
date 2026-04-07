# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 프로젝트 개요

Claude Code CLI용 플러그인/스킬 저장소. 개발 자동화 도구(커밋 생성, 변경사항 설명, 디버그 로그 삽입, 시스템 알림)를 제공한다.

빌드 시스템이나 테스트 프레임워크 없이, Markdown 스킬 정의 + Bash 스크립트 + JSON 설정으로 구성된 순수 설정 기반 프로젝트다.

## 아키텍처

```
plugins/
  └── devtools/                    # 메인 플러그인
      ├── .claude-plugin/
      │   └── plugin.json          # 플러그인 매니페스트 (이름, 버전, 작성자)
      ├── hooks/
      │   └── hooks.json           # 이벤트 핸들러 (PreToolUse, Stop, Notification)
      ├── scripts/
      │   ├── notify.sh            # 크로스 플랫폼 시스템 알림
      │   └── pre-commit-check.sh  # 커밋 전 안전 검사
      └── skills/
          ├── commit/SKILL.md      # /commit 스킬 정의
          ├── debug-log/SKILL.md   # /debug-log 스킬 정의
          ├── explain/SKILL.md     # /explain 스킬 정의
          ├── issue/SKILL.md       # /issue 스킬 정의
          ├── pr/SKILL.md          # /pr 스킬 정의
          └── squash/SKILL.md      # /squash 스킬 정의
```

**플러그인 등록:** 사용자의 `.claude/settings.json`에 플러그인 디렉토리 경로를 추가하여 설치한다.

**스킬 구조:** 각 스킬은 `SKILL.md` 파일에 동작 규칙, 출력 형식, `allowed-tools`를 정의한다. Claude Code가 이 마크다운을 직접 해석하여 실행한다.

**훅 구조:** `hooks.json`에 이벤트별 스크립트를 매핑한다. `notify.sh`는 git remote URL에서 레포명을 추출하고, macOS/Windows/Linux 알림을 지원한다. `pre-commit-check.sh`는 `PreToolUse` 이벤트로 `git commit` 명령을 가로채어 민감 정보, .env 파일, 충돌 마커, 대용량 파일을 검사하고 `--no-verify` 사용을 차단한다.

## 커밋 컨벤션

- Conventional Commits 형식: `<type>(<scope>): <subject>`
- type과 scope는 영문, subject와 body는 한국어
- 지원 type: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`
- scope 예시: `plugin`, `skill`, `hook`, `config`, `notify`
- `--no-verify` 플래그 사용 금지
- Co-authored-by 트레일러 추가하지 않음

## 스킬 작성 규칙

- `SKILL.md` 파일은 전체를 영어로 작성한다 (Claude Code가 해석하는 명세이므로)
- `allowed-tools`에 사용할 도구를 명시한다
- 사용자 확인이 필요한 스킬(commit, pr, squash)은 실행 전 반드시 확인 단계를 포함한다
- `--no-verify` 플래그, Co-authored-by 트레일러, 자동 생성 attribution 텍스트를 금지한다

## 언어 규칙

- 코드, 타입, 식별자: 영문
- 사용자 대면 메시지, 커밋 메시지 본문, 문서: 한국어
- SKILL.md 파일: 영문
