# 🔍 다관점 문서 리뷰 에이전트 모음

> 하나의 문서를 여러 전문가 관점에서 리뷰하고, 점수 기반으로 자동 반복 개선하는 Claude Code 플러그인입니다.

## 왜 만들었나?

AI로 문서를 작성하면, 한 가지 관점에서만 정리된 결과물이 나옵니다.  
GPT에 올려보고, Claude에 올려보고, 다시 GPT에 올리면 — 서로 다른 관점의 피드백이 나오면서 문서가 점점 좋아집니다.

**이 과정을 Claude 하나로 자동화한 것**이 이 프로젝트입니다.

같은 Claude라도 역할(페르소나)을 다르게 부여하면 서로 다른 관점에서 리뷰합니다.  
트집잡기가 아닌, **놓친 부분을 건설적으로 보완**하는 방식입니다.

---

## 🚀 설치 방법

### 방법 1: 마켓플레이스 설치 (권장)

Claude Code에서 아래 두 줄만 입력하면 됩니다:

```
/plugin marketplace add TTtaTTe/About-Us
/plugin install report-reviewer
```

끝! 이후에는 `보고서 리뷰해줘`라고 말하면 자동으로 실행됩니다.

업데이트가 나오면 자동으로 반영됩니다:
```
/plugin marketplace update about-us
```

### 방법 2: 수동 설치

`plugins/report-reviewer/agents/report-reviewer.md` 파일을 다운받아서  
아래 폴더에 복사합니다:

| OS | 경로 |
|----|------|
| Windows | `C:\Users\내이름\.claude\agents\` |
| Mac | `~/.claude/agents/` |

---

## 📁 프로젝트 구조

```
About-Us/
├── .claude-plugin/
│   └── marketplace.json                ← 마켓플레이스 카탈로그
├── plugins/
│   └── report-reviewer/                ← 보고서 리뷰 플러그인
│       ├── .claude-plugin/
│       │   └── plugin.json             ← 플러그인 매니페스트
│       └── agents/
│           └── report-reviewer.md      ← 에이전트 본체 v2.0
├── guides/
│   └── 설치가이드.md
└── README.md
```

---

## 📋 플러그인 목록

| 플러그인 | 용도 | 버전 | 상태 |
|----------|------|------|------|
| `report-reviewer` | 보고서/분석 자료 다관점 리뷰 | v2.0 | ✅ 사용 가능 |
| `proposal-reviewer` | 기획서/제안서 리뷰 | — | 🔜 예정 |
| `tech-doc-reviewer` | 기술 문서 리뷰 | — | 🔜 예정 |
| `marketing-reviewer` | 마케팅/홍보 자료 리뷰 | — | 🔜 예정 |

---

## 🔄 작동 원리

```
📄 문서 입력
    │
    ▼
📎 PHASE 0: 맥락 수집
   (문서 유형, 목적, 독자, 분야 파악)
    │
    ▼
┌─────────────────────────────────────┐
│  🔷 전략가 리뷰 → 체크리스트 + 점수  │
│  🔷 분석가 리뷰 → 체크리스트 + 점수  │  라운드 1
│  🔷 독자대변인 리뷰 → 체크리스트 + 점수│
└─────────────────────────────────────┘
    │
    ▼
📊 리뷰 결과 보고서 제출
   (수정 계획, 충돌 의견, 점수 요약)
    │
    ▼
👤 사용자 결정
   ├─ ✅ 수정 적용 → 새 파일 생성
   ├─ ✏️ 부분 수정 → 선택 항목만 적용
   ├─ 🔄 재리뷰 → 기준 변경 후 다시
   └─ 💬 논의 → 특정 항목 상세 설명
    │
    ▼
📊 전원 8.0점 이상? ─── Yes ──→ ✅ 완료
    │ No
    ▼
🔄 다음 라운드 (최대 3라운드)
    │
    ▼
📄 최종 문서 + 리뷰 보고서 저장
```

### v2.0 주요 특징

| 특징 | 설명 |
|------|------|
| 🧠 깊은 페르소나 | 각 리뷰어에 경력, 전문성, 커뮤니케이션 스타일까지 설정 |
| 📎 맥락 수집 | 리뷰 전 문서 유형·목적·독자·분야를 먼저 파악 |
| 📊 결과 보고서 선제출 | 수정 전에 반드시 보고서를 먼저 보여주고 승인 후 적용 |
| 🎯 우선순위 체계 | P1(긴급)/P2(권장)/P3(선택)으로 피드백 분류 |
| 📐 정량적 채점 | 소수점 허용 (예: 8.7), 채점 공식 투명 공개 |
| 🛡️ 예외 처리 | 대형 문서, 인식 불가, 의견 충돌, 특수 분야 대응 |
| 💾 학습 메모리 | 반복 리뷰에서 발견한 패턴을 축적하여 점점 정확해짐 |
| 🔒 원본 보존 | 원본 파일은 절대 수정하지 않음 |

---

## ⚙️ 커스터마이징

```
보고서 리뷰해줘                        ← 기본 설정으로 실행
보고서 리뷰해줘 통과점수 9점으로          ← 통과 기준 상향
보고서 리뷰 5라운드까지 해줘             ← 라운드 수 변경
전략가 관점만 리뷰해줘                   ← 특정 리뷰어만
100점 만점으로 채점해줘                  ← 점수 체계 변경
P1만 적용해줘                          ← 긴급 항목만 수정
```

---

## 🔗 참고한 오픈소스

- [VoltAgent/awesome-claude-code-subagents](https://github.com/VoltAgent/awesome-claude-code-subagents) — 127개 이상의 Claude Code subagent 컬렉션
- [Anthropic 공식 문서 - Custom Subagents](https://code.claude.com/docs/en/sub-agents) — subagent 제작 가이드
- [Anthropic 공식 문서 - Plugin Marketplace](https://code.claude.com/docs/en/plugin-marketplaces) — 마켓플레이스 구축 가이드
- [Anthropic Agent SDK Demos](https://github.com/anthropics/claude-agent-sdk-demos) — 공식 데모 모음

---

## 📌 요구사항

- [Claude Code](https://claude.ai) (데스크탑 또는 터미널)
- Claude Pro 이상 구독
- Opus 모델 (1M 컨텍스트 윈도우 활용)

---

## 🤝 기여

새로운 리뷰어 플러그인을 추가하거나 기존 에이전트를 개선하고 싶다면:
1. 이 레포를 Fork
2. `plugins/` 아래에 새 플러그인 폴더 생성
3. Pull Request 제출

---

## 📝 라이선스

MIT License — 자유롭게 사용, 수정, 배포할 수 있습니다.
