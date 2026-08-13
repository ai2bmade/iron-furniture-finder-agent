# ai2bmade

해외 B2B 시장조사·잠재고객(엔드유저/파트너/디스트리뷰터/경쟁사) 발굴 자동화 시스템.
버티컬(핸드메이드 가구, 하우스홀드 로봇, 화장품, 전기전자, 발전소·변전소, 교육 콘텐츠,
헬스케어 하드웨어, AI 에이전트 등)마다 하나의 "Finder Agent"(OpenClaw 스킬)를 만들고,
이를 Telegram으로 트리거·보고받는 구조입니다.

```
ai2bmade/
├── finder-agent-factory/      # 에이전트를 만드는 에이전트 (메타 에이전트)
│   ├── generate-finder-agent.js
│   ├── templates/
│   │   └── finder-agent-methodology.md   # 4대 카테고리 고정 방법론 문서
│   ├── package.json
│   └── .env.example
└── skills/                    # OpenClaw workspace skill root
    └── handmade-furniture-finder/
        └── SKILL.md           # 1번째 테스트 버티컬 (팩토리로 생성한 예시)
```

## 전체 워크플로우

```
[로컬: G드라이브 작업 폴더]
   Claude Code로 finder-agent-factory 실행
        │  (버티컬별로 몇 가지 질문에 답)
        ▼
   skills/<vertical>-finder/SKILL.md 생성
        │
        ▼
   git add / commit / push  →  GitHub "ai2bmade" 저장소
        │
        ▼
[Hostinger VPS — OpenClaw 사전 설치됨]
   git pull (또는 최초 1회 git clone)
        │
        ▼
   OpenClaw가 skills/ 를 workspace skill root로 자동 스캔·인식
        │
        ▼
   Telegram으로 "/finder handmade-furniture" 또는 자연어 요청
        │
        ▼
   Finder Agent 실행 → 4대 카테고리 조사 → 결과 요약을 Telegram으로 전송
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

버티컬 이름을 입력하면 몇 가지 질문(제품 정의, 타겟 국가, 업계 데이터 소스, 키워드,
제약사항) 후 `../skills/<slug>-finder/SKILL.md`가 자동 생성됩니다.
4대 조사 카테고리(엔드유저/파트너/디스트리뷰터/경쟁사) 프레임워크는 모든 버티컬에
고정으로 적용되므로 이 부분은 다시 묻지 않습니다.

## 2. GitHub(ai2bmade)에 커밋

```bash
git add skills/ finder-agent-factory/
git commit -m "add <vertical> finder agent"
git push origin main
```

## 3. Hostinger VPS(OpenClaw)에 반영

최초 1회:
```bash
# VPS에서, OpenClaw workspace 디렉토리 기준
git clone https://github.com/<your-account>/ai2bmade.git
```
이후에는:
```bash
cd ai2bmade
git pull
```

OpenClaw는 `<workspace>/skills/` 를 가장 높은 우선순위의 skill root로 스캔합니다.
이 저장소 자체를 OpenClaw workspace로 사용하거나, `skills/` 폴더를 실제 workspace의
`skills/` 경로에 심볼릭 링크로 연결하세요.

```bash
ln -s ~/ai2bmade/skills ~/.openclaw/workspace/skills
```

> Hostinger의 OpenClaw 사전 설치 템플릿은 workspace 기본 경로가 다를 수 있습니다.
> `openclaw` 대시보드 또는 CLI에서 정확한 workspace 경로를 먼저 확인하세요.

## 4. Telegram 연결

OpenClaw 대시보드(또는 `openclaw` CLI)에서 Telegram 채널을 연결하고 (BotFather로
발급받은 토큰 등록), 허용 사용자(본인)를 등록하면 Telegram 채팅으로 Finder Agent를
트리거하고 결과를 받을 수 있습니다. 정확한 명령은 사용 중인 OpenClaw 버전의
공식 문서(Telegram 연동 섹션)를 확인하세요.

## 5. 새 버티컬 추가하기

로드맵 순서 (테스트 우선순위):

1. 핸드메이드 가구·철제 소품 ✅ (예시 생성 완료)
2. 하우스홀드 로봇 (돌보미/펫 로봇)
3. 화장품
4. 전기전자
5. 발전소·변전소
6. 교육 콘텐츠
7. 헬스케어 하드웨어
8. AI 에이전트 시장

새 버티컬을 추가할 때마다 `finder-agent-factory`를 다시 실행하면 됩니다.
4대 조사 카테고리 방법론은 `finder-agent-factory/templates/finder-agent-methodology.md`
에 문서화되어 있으니, 팩토리 프롬프트를 수정할 때 이 문서도 함께 갱신하세요.
