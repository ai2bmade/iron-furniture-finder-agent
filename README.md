# ai2bmade — Finder Agent 허브

해외 B2B 시장조사·잠재고객(엔드유저/파트너/디스트리뷰터/경쟁사) 발굴 자동화 시스템.
버티컬(핸드메이드 가구, 하우스홀드 로봇, 화장품, 전기전자, 발전소·변전소, 교육 콘텐츠,
헬스케어 하드웨어, AI 에이전트 등)마다 하나의 "Finder Agent"(OpenClaw 스킬)를 만들고,
이를 Telegram으로 트리거·보고받는 구조입니다.

## 2026-08-16부터: 마켓마다 별도 GitHub 레포 + 별도 Telegram 봇

원래는 모든 버티컬의 `skills/`가 이 레포 하나에 같이 있었지만, **마켓마다
운영·배포를 독립시키기 위해 배포용 레포와 Telegram 봇을 마켓별로 분리**하기로
했습니다.

| 마켓 | 배포 레포 | Telegram 봇 |
|---|---|---|
| 철제 감성 가구 | (이 레포, `iron-furniture-finder-agent`) `skills/handmade-metal-furniture-decor-finder/` | 미정 |
| 돌봄/펫/서비스 로봇 | [`ai2bmade/robot_market`](https://github.com/ai2bmade/robot_market) | [@RobotMarketFinderBot](https://t.me/RobotMarketFinderBot) |

**이 레포(`iron-furniture-finder-agent`)는 두 가지 역할을 겸합니다**:
1. 모든 마켓이 공유하는 허브 — `finder-agent-factory`(메타 에이전트),
   고정 방법론 문서, `.claude/skills`(대화형 전용 스킬), 설계 히스토리(`CLAUDE.md`)
2. 철제 가구 버티컬 자체의 배포 레포 (아직 별도 레포로 분리 안 함)

새 버티컬을 만들 때는:
- `finder-agent-factory`로 SKILL.md를 생성/수정하는 작업은 계속 이 레포에서 한다
- 하지만 **배포용 `skills/<slug>/` 폴더는 그 마켓 전용의 새 GitHub 레포로
  옮겨서 push**하고, 그 레포를 VPS에서 별도로 clone/pull해 별도 Telegram
  봇에 연결한다 (아래 로드맵 참고)

```
agent-factory/ (허브, 이 레포)
├── finder-agent-factory/      # 에이전트를 만드는 에이전트 (메타 에이전트)
│   ├── generate-finder-agent.js
│   ├── templates/
│   │   └── finder-agent-methodology.md   # 4대 카테고리 + 뉴스검색 등 고정 방법론
│   ├── package.json
│   └── .env.example
├── skills/                     # 철제 가구 버티컬만 (이 레포 자체가 그 배포 레포)
│   └── handmade-metal-furniture-decor-finder/
│       └── SKILL.md
└── .claude/skills/             # Claude Code 대화형 전용 스킬 (어느 VPS로도 배포 안 함)

robot_market/ (별도 레포, 별도 봇)
└── skills/household-care-robot-finder/SKILL.md
```

## 전체 워크플로우

```
[로컬: G드라이브 작업 폴더]
   Claude Code로 finder-agent-factory 실행 (또는 대화로 직접 SKILL.md 수정)
        │  (버티컬별로 몇 가지 질문에 답 / 실증 검증)
        ▼
   완성된 SKILL.md를 그 마켓 전용 레포의 skills/<vertical>-finder/ 로 배치
        │
        ▼
   git add / commit / push  →  그 마켓 전용 GitHub 레포
        │
        ▼
[Hostinger VPS — 마켓별로 별도 OpenClaw workspace 또는 별도 컨테이너]
   해당 마켓 레포만 git pull (또는 최초 1회 git clone)
        │
        ▼
   OpenClaw가 그 workspace의 skills/ 를 스캔·인식
        │
        ▼
   그 마켓 전용 Telegram 봇으로 "/finder <slug>" 또는 자연어 요청
        │
        ▼
   Finder Agent 실행 → 조사 → 결과 요약을 그 봇으로 전송
        (원본 데이터는 스프레드시트에 누적)
```

## 1. 로컬 개발 (Claude Code)

```bash
cd finder-agent-factory
npm install
cp .env.example .env
# .env에 ANTHROPIC_API_KEY 입력

npm start
```

버티컬 이름을 입력하면 몇 가지 질문(제품 정의, 타겟 국가, 업계 데이터 소스,
키워드, 제약사항, **뉴스/보도자료 소스**, **짧은 호출 트리거 단어**) 후
`../skills/<slug>-finder/SKILL.md`가 자동 생성됩니다. 고정 방법론(4대
조사 카테고리, 뉴스 검색 최우선, 짧은 호출 문법 등)은 모든 버티컬에
고정으로 적용되므로 이 부분은 다시 묻지 않습니다. 생성 후에는 실제
웹검색으로 결과 품질을 검증하고 다듬는 걸 권장합니다 (하우스홀드 로봇
버티컬에서 이렇게 검증하며 방법론 자체가 여러 번 개선됨 — 상세는
`CLAUDE.md` 참고).

## 2. 그 마켓 전용 레포에 배치하고 커밋

```bash
# 아직 그 마켓 전용 레포가 없다면 먼저 GitHub에 빈 레포 생성
# (예: ai2bmade/<market-name>)

git clone https://github.com/ai2bmade/<market-name>.git
cp -r skills/<slug>-finder <market-name>/skills/
cd <market-name>
git add skills/
git commit -m "add <vertical> finder agent"
git push origin main
```

## 3. Hostinger VPS(OpenClaw)에 반영

최초 1회 (마켓별 레포마다 반복):
```bash
git clone https://github.com/ai2bmade/<market-name>.git
```
이후에는:
```bash
cd <market-name>
git pull
```

OpenClaw는 workspace의 `skills/`를 가장 높은 우선순위의 skill root로
스캔합니다. **마켓마다 별도 봇으로 운영하므로, 마켓별로 별도 OpenClaw
workspace(또는 별도 인스턴스)를 두고 그 마켓 레포의 `skills/`만
심볼릭 링크**하세요.

```bash
ln -s ~/<market-name>/skills ~/.openclaw/workspaces/<market-name>/skills
```

> Hostinger의 OpenClaw 사전 설치 템플릿은 workspace 기본 경로가 다를 수 있습니다.
> `openclaw` 대시보드 또는 CLI에서 정확한 workspace 경로를 먼저 확인하세요.

## 4. Telegram 연결

마켓마다 BotFather로 **별도 봇**을 만들고, OpenClaw 대시보드(또는
`openclaw` CLI)에서 그 마켓의 workspace에 그 봇 토큰을 연결하세요. 허용
사용자(본인)를 등록하면 그 봇으로만 그 마켓의 Finder Agent를 트리거하고
결과를 받습니다. 정확한 명령은 사용 중인 OpenClaw 버전의 공식 문서
(Telegram 연동 섹션)를 확인하세요.

## 5. 새 버티컬 추가하기

로드맵 순서 (테스트 우선순위):

1. 핸드메이드 가구·철제 소품 ✅ (이 레포에서 직접 배포)
2. 하우스홀드 로봇 (돌보미/펫/서비스 로봇) ✅ → [`ai2bmade/robot_market`](https://github.com/ai2bmade/robot_market) 분리 완료
3. 화장품
4. 전기전자
5. 발전소·변전소
6. 교육 콘텐츠
7. 헬스케어 하드웨어
8. AI 에이전트 시장

새 버티컬을 추가할 때마다:
1. 이 레포에서 `finder-agent-factory`를 실행해 SKILL.md를 만든다
2. 그 마켓 전용 새 GitHub 레포를 만들고(비어있는 레포), 위 SKILL.md를 그
   레포의 `skills/<slug>/`에 배치해 push한다
3. VPS에 그 마켓 레포를 별도로 clone하고, 별도 Telegram 봇에 연결한다

4대 조사 카테고리 + 뉴스 검색 + 짧은 호출 문법 등 고정 방법론은
`finder-agent-factory/templates/finder-agent-methodology.md`에
문서화되어 있으니, 팩토리 프롬프트를 수정할 때 이 문서도 함께 갱신하세요.
