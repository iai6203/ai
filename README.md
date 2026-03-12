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
│       │   └── notify.sh
│       └── skills/
│           ├── commit/
│           │   └── SKILL.md
│           └── explain/
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
| **explain** | staged 변경사항을 파일별로 분석하여 변경 유형, 내용, 의도, 영향 라인을 포함한 상세 설명 생성 |

#### 훅

| 이벤트 | 설명 |
|--------|------|
| **Stop** | 작업 완료 시 레포지토리명과 브랜치명을 포함한 시스템 알림 전송 |
| **Notification** | HITL(Human-in-the-Loop) 대기 시 응답 요청 알림 전송 |

알림은 macOS, Windows, Linux를 지원하며, 레포지토리명은 git remote URL에서 추출합니다.

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

- 스킬: Claude Code에서 `/commit`, `/explain` 명령으로 호출
- 훅: 작업 완료 및 HITL 대기 시 자동 실행
