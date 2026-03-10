# Claude Code Resources

Claude Code에서 사용하는 커스텀 플러그인, 훅 및 리소스를 모아둔 저장소입니다.

## 프로젝트 구조

```
.
├── hooks/
│   └── notify.sh
├── plugins/
│   └── devtools/
│       ├── .claude-plugin/
│       │   └── plugin.json
│       └── skills/
│           ├── commit/
│           │   └── SKILL.md
│           └── explain/
│               └── SKILL.md
└── README.md
```

## 플러그인

### devtools

개발 워크플로우를 위한 스킬 모음입니다.

| 스킬 | 설명 |
|------|------|
| **commit** | staged 변경사항을 분석하여 Conventional Commits 규격의 커밋 메시지를 자동 생성 |
| **explain** | staged 변경사항을 파일별로 분석하여 변경 유형, 내용, 의도, 영향 라인을 포함한 상세 설명 생성 |

## 훅

| 훅 | 설명 |
|----|------|
| **notify** | 작업 완료 시 디렉토리명과 git branch명을 포함한 시스템 알림 전송 (macOS, Windows, Linux 지원) |

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

스킬은 Claude Code에서 `/commit`, `/explain` 명령으로 호출할 수 있습니다.

### 훅 등록

Claude Code의 `/hooks` 메뉴에서 원하는 이벤트에 훅 스크립트를 등록합니다.

```
"$CLAUDE_PROJECT_DIR"/hooks/notify.sh
```
