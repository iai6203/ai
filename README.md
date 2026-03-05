# Claude Code Resources

Claude Code에서 사용하는 커스텀 스킬 및 리소스를 모아둔 저장소입니다.

## 프로젝트 구조

```
.
├── skills/
│   └── common/
│       ├── commit/
│       │   └── SKILL.md
│       └── impact/
│           └── SKILL.md
└── README.md
```

## 스킬 목록

| 스킬 | 설명 |
|------|------|
| **commit** | staged 변경사항을 분석하여 Conventional Commits 규격의 커밋 메시지를 자동 생성 |
| **impact** | staged 변경사항의 범위, 리스크, 영향 영역, 브레이킹 체인지를 분석하여 영향도 평가 리포트 생성 |

## 사용 방법

이 저장소의 스킬을 프로젝트에서 사용하려면 `.claude/skills/` 디렉토리에 심링크를 생성합니다.

```bash
# 프로젝트 루트에서 .claude/skills 디렉토리 생성
mkdir -p .claude/skills

# 원하는 스킬을 심링크로 연결
ln -s /path/to/this/repo/skills/common/commit .claude/skills/commit
ln -s /path/to/this/repo/skills/common/impact .claude/skills/impact
```

스킬은 Claude Code에서 `/commit`, `/impact` 명령으로 호출할 수 있습니다.
