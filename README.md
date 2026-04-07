# Claude Code Resources

Claude Code에서 사용하는 커스텀 플러그인, 훅 및 리소스를 모아둔 저장소입니다.

## 프로젝트 구조

```
.
├── plugins/
│   └── devtools/
│       ├── .claude-plugin/
│       │   └── plugin.json
│       ├── hooks/
│       │   └── hooks.json
│       ├── scripts/
│       │   ├── notify.sh
│       │   └── pre-commit-check.sh
│       └── skills/
│           ├── commit/
│           │   └── SKILL.md
│           ├── debug-log/
│           │   └── SKILL.md
│           ├── explain/
│           │   └── SKILL.md
│           ├── pr/
│           │   └── SKILL.md
│           ├── issue/
│           │   └── SKILL.md
│           └── squash/
│               └── SKILL.md
└── README.md
```

## 플러그인

### devtools

개발 워크플로우를 위한 스킬과 훅 모음입니다.

#### 스킬

| 스킬 | 설명 |
|------|------|
| **commit** | staged 변경사항을 분석하여 Conventional Commits 규격의 커밋 메시지를 자동 생성 |
| **debug-log** | 디버깅을 위한 `[DEBUG]` 접두사 로그 구문을 코드에 삽입 |
| **explain** | staged 변경사항을 파일별로 분석하여 변경 유형, 내용, 의도, 영향 라인을 포함한 상세 설명 생성 |
| **issue** | GitHub 이슈를 자동 생성 (라벨, assignee, 이슈 템플릿 자동 감지) |
| **pr** | 브랜치의 커밋을 분석하여 Conventional Commits 타이틀로 PR 자동 생성 |
| **squash** | 현재 브랜치의 커밋들을 스쿼시하여 Conventional Commits 메시지를 자동 생성 |

##### pr 사용 예시

| 명령 | 설명 |
|------|------|
| `/pr` | 기본 브랜치 대상으로 PR 생성 |
| `/pr develop` | develop 브랜치 대상으로 PR 생성 |

##### issue 사용 예시

| 명령 | 설명 |
|------|------|
| `/issue 로그인 시 세션 만료 처리 안 됨` | 설명 기반으로 이슈 생성 |
| `/issue` | 대화형으로 이슈 내용 입력 |

##### squash 사용 예시

| 명령 | 설명 |
|------|------|
| `/squash` | base 브랜치 이후 모든 커밋을 1개로 스쿼시 |
| `/squash 5` | 최근 5개 커밋을 1개로 스쿼시 |
| `/squash --base develop` | develop 브랜치 이후 커밋을 스쿼시 |
| `/squash --groups 3` | base 브랜치 이후 커밋을 3개 논리적 그룹으로 분할 스쿼시 |
| `/squash 10 --groups 2` | 최근 10개 커밋을 2개 그룹으로 분할 스쿼시 |

#### 훅

| 이벤트 | 설명 |
|--------|------|
| **PreToolUse** | 커밋 전 staged 파일 안전 검사 (민감 정보, .env, 충돌 마커, 대용량 파일, `--no-verify` 차단) |
| **Stop** | 작업 완료 시 레포지토리명과 브랜치명을 포함한 시스템 알림 전송 |
| **Notification** | HITL(Human-in-the-Loop) 대기 시 응답 요청 알림 전송 |

알림은 macOS, Windows, Linux를 지원하며, 레포지토리명은 git remote URL에서 추출합니다.
PreToolUse 훅은 `git commit` 명령 실행 전에 자동으로 트리거되며, 검사 실패 시 커밋을 차단합니다.

## 사용 방법

### 플러그인 설치

프로젝트의 `.claude/settings.json`에 플러그인 경로를 추가합니다.

```json
{
  "plugins": [
    "/path/to/this/repo/plugins/devtools"
  ]
}
```

플러그인을 설치하면 스킬과 훅이 함께 등록됩니다.

- 스킬: Claude Code에서 `/commit`, `/explain`, `/issue`, `/pr`, `/squash` 명령으로 호출
- 훅: 작업 완료 및 HITL 대기 시 자동 실행
