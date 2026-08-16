name: 돌봄로봇바이어소스리서치
description: 시니어케어 로봇(모니터링·낙상감지·복약알림·이동보조·정서케어), 로봇펫(반려로봇·치매케어용 동물형 로봇), 로봇 도우미(안내·이동·리셉션)를 실제로 구매하거나 향후 구매할 가능성이 높은 요양시설 운영그룹/복지기기 유통사/리테일러/호텔·SI/정부 복지조달 기관을, 링크드인이 아니라 시니어케어 협회·정부 보조금(AAL 등)·공공조달 포털·경쟁제품 설치기반에서 먼저 찾아내는 스킬. "공식 소스 → 도입 증거(Adoption Signal) → 미래 구매 신호(파일럿/보조금) → 카테고리 적합성 → 유통 채널 구조 → 담당 조직 → 링크드인 담당자" 순서로 조사하며, 링크드인은 상위 리드에 대해서만 담당자를 찾는 마지막 단계로만 사용한다. "유틸리티바이어소스리서치"와 같은 계열(공식 소스 기반 리드 발굴)이지만, 이 시장은 아직 초기 시장이라 정식 구매보다 파일럿·보조금 같은 선행 신호의 비중을 더 높게 둔다.

# 돌봄로봇바이어소스리서치 스킬

## 0. 이 스킬과 다른 스킬·트랙의 관계

이 프로젝트에는 돌봄로봇 시장을 다루는 결과물이 두 갈래로 존재한다.

| | `skills/household-care-robot-finder` (OpenClaw 배포용) | 돌봄로봇바이어소스리서치 (이 스킬, Claude Code 대화형) |
|---|---|---|
| **실행 위치** | Hostinger VPS의 OpenClaw, Telegram으로 트리거 | Claude Code 세션에서 사람과 대화하며 직접 진행 |
| **방법론** | 프로젝트 공통 "4대 조사 카테고리"(엔드유저/파트너/디스트리뷰터/경쟁사) 고정 프레임 | "공식 도입 증거(Adoption Signal) 우선" 프레임 — 파일럿·보조금·조달·임상연구를 먼저 확인 |
| **강점** | 국가 로테이션으로 자동·정기 실행, Telegram으로 결과 수신 | 근거(공식 URL)가 확실한 리드부터 만들고, Buying Stage(파일럿~정식조달)까지 세밀하게 추적 |
| **사람 개입** | 최소 — 정기 자동 실행 목적 | 필요 — 카테고리 혼동(산업용 로봇팔 vs 케어로봇) 검증, PDF/리스트 판단 등 사람 확인이 필요한 무거운 워크플로우 |

즉 `skills/household-care-robot-finder`가 "정기적으로 넓게 훑는" 자동화 트랙이라면, 이 스킬은 "근거를 깊게 파는" 대화형 트랙이다. 두 트랙은 서로 대체하지 않고 병행한다 — 이 스킬에서 찾은 Lead Score 상위 조직을 `household-care-robot-finder`의 다음 국가 로테이션 우선순위에 반영하거나, 반대로 그쪽에서 나온 후보를 이 스킬로 더 깊이 검증하는 식으로 이어 쓸 수 있다.

또한 이 스킬은 "수출바이어파인더"와도 이어진다 — 이 스킬의 마지막 단계(11절)에서 상위 Lead에 대해 링크드인 검색을 돌려 PDF로 내보낸 뒤, "수출바이어파인더" 스킬로 인물 단위 등급 분류를 이어갈 수 있다.

---

## 1. 목적

이 스킬의 목적은 **시니어케어 로봇(Senior Care Robot), 로봇펫(Companion/Pet Robot), 로봇 도우미(Service/Helper Robot)** 를 실제로 구매하거나 향후 구매 가능성이 높은 해외 바이어를 공식 소스 기반으로 찾아내는 것이다.

여기서 "바이어"는 한 부류가 아니다. 세 가지 서로 다른 로봇 카테고리가 서로 다른 유통 구조와 의사결정권자를 갖고 있으므로, 반드시 **제품군 → 구매 채널 구조**를 먼저 구분한 뒤 조사한다.

| 로봇 카테고리 | 1차 구매 채널 | 구매 동기 |
|---|---|---|
| 시니어케어 로봇 (모니터링, 낙상감지, 복약알림, 이동보조, 정서케어) | 요양시설·재가복지·병원·정부 복지조달 | 인력난 대응, 낙상/응급 대응, 정부 보조금 |
| 로봇펫 (반려로봇, 치매케어용 동물형 로봇) | 유통·리테일·요양시설(치료용) | 정서 안정, 치매 비약물 치료, 소비재 판매 |
| 로봇 도우미 (안내·이동·리셉션·배송) | 호텔·상업시설·SI(시스템통합)·렌탈업체 | 인력 대체, 고객 경험, 운영 효율 |

조사 흐름은 다음과 같다.

`공식 소스 → 구매/도입 증거(Pilot·보조금·조달) → 미래 구매 신호 → 시설/제품 스펙 적합성 → 유통 채널 구조 → 담당 조직 → LinkedIn 담당자`

이 스킬은 특정 국가에 국한하지 않고 **해외 수출 바이어 전반**을 대상으로 한다(북미, 유럽, 일본, 중동, 동남아 등).

---

## 2. 핵심 원칙

Buyer 검색은 LinkedIn에서 시작하지 않는다.

돌봄로봇 시장은 아직 초기 시장(Early Adopter Market)이라, "구매(Purchase)"보다 "파일럿 도입(Pilot)"과 "보조금 지원 구매(Grant-funded Purchase)"가 먼저 나타나는 경우가 많다. 따라서 일반 산업재보다 **미래 구매 신호(정부 보조금, 파일럿 프로그램, 시범사업)의 비중을 더 높게** 둔다.

핵심 원칙:

- 회사명보다 **도입 신호(Adoption Signal)** 를 먼저 찾는다 — 파일럿, 보조금 수혜, 전시회 계약, 조달 공고 등.
- 마케팅 보도자료보다 **정부 복지/보건 기관, 요양시설 협회, 조달 포털, 학술/임상 연구** 문서를 우선한다.
- 같은 "로봇"이라도 **어느 카테고리(시니어케어/로봇펫/도우미)** 에 해당하는지, **어떤 기능(모니터링/이동보조/정서케어/안내)** 인지 반드시 구분한다.
- 시설 단위 구매자(요양원 개별)뿐 아니라 **체인/그룹 단위 구매자**(다점포 운영사, 유통 총판)를 우선한다 — 개별 요양원은 물량이 작다.
- 모든 Lead는 반드시 최소 1개의 공식 근거 URL을 가져야 한다.
- 경쟁 제품(PARO, ElliQ, Temi, Pepper, Lovot 등)의 실제 도입 사례를 역추적하는 것이 가장 효율적인 시작점이다.

---

## 3. 타겟 바이어 유형

### Tier 1 — 시니어케어 기관 및 운영 그룹 (Institutional Senior Care)

가장 우선적으로 조사한다. 특히 **다점포/체인 운영사**를 우선한다.

예시:

- Nursing Home / Care Home Chain
- Assisted Living Operator
- Continuing Care Retirement Community (CCRC)
- Home Care Agency / Domiciliary Care Provider
- Adult Day Care Center Network
- Hospital Geriatric / Rehabilitation Department
- Memory Care / Dementia Care Facility

특히 적합한 제품:

- 낙상감지·모니터링 로봇
- 복약알림 로봇
- 이동보조 로봇
- 정서케어·치매케어 로봇 (반려로봇 포함)
- 원격진료·텔레프레즌스 로봇

### Tier 2 — 복지기기·의료기기 유통 채널 (Assistive Tech / MedTech Distribution)

로봇 제조사가 직접 각 시설에 파는 경우는 드물고, 대부분 이 계층을 거친다. **수출 바이어로서 가장 현실적인 1차 타겟.**

예시:

- Durable Medical Equipment (DME) Distributor
- Assistive Technology Retailer / Reseller
- Medical Device Importer & Distributor
- Rehabilitation Equipment Supplier
- Mobility Aid / Hearing Aid 유통 체인 (인접 채널 — 이미 시니어 대상 판매 인프라 보유)

### Tier 3 — 리테일·이커머스 (소비자용 로봇펫·홈 도우미 로봇)

예시:

- 완구·가전 리테일러 (Consumer Electronics Retailer)
- 백화점 프리미엄 가젯 코너
- 이커머스 플랫폼 (종합 또는 시니어/펫 전문몰)
- AgeTech / Smart Home 전문 리테일러
- 펫용품 리테일러 (로봇펫 크로스오버)

특히 적합한 제품:

- 로봇펫, 반려로봇
- 소비자용 홈 도우미 로봇

### Tier 4 — 상업시설·서비스업 B2B (로봇 도우미)

예시:

- Hotel / Resort Group
- Senior Living Hospitality Operator (식당·컨시어지 서비스 겸영)
- Facility Management Company
- System Integrator — 특히 AAL(Ambient Assisted Living) 통합업체
- 의료기기 렌탈/리스 전문업체

특히 적합한 제품:

- 안내·리셉션 로봇
- 이동·배송 로봇
- 서비스 도우미 로봇

### Tier 5 — 정부·공공조달 및 R&D/파일럿 기관

로봇 시장 특유의 계층 — 실 구매보다 먼저 나타나는 **선행 지표**로서 중요.

예시:

- Ministry / Department of Health, Aging, Welfare (국가별)
- Public Tender Platform 운영 기관
- 대학병원·연구기관 임상 파일럿 프로그램
- 고령친화산업 지원기관, Aging-in-Place 관련 NGO
- 지자체 복지기술 시범사업 운영 부서

---

## 4. 공식 소스 계층 (Official Source Hierarchy)

공식 소스는 아래 순서대로 조사한다.

### Source Level 1 — 시니어케어 산업 협회·디렉토리

목적: 전체 Buyer 모집단(요양시설/운영 그룹) 확보

주요 소스:

- LeadingAge (미국 비영리 시니어케어 협회, 회원 디렉토리)
- Argentum (미국 assisted living 협회)
- Care Quality Commission (영국 요양시설 등록 디렉토리)
- Eldercare Locator (미국 정부 운영 시니어케어 디렉토리)
- European Ageing Network (EAN)
- International Federation on Ageing (IFA)
- AGE Platform Europe
- 국가별 노인복지시설 협회 (독일 BAGSO, 일본 전국개호사업자협회 등)

검색 예시:

```text
site:leadingage.org member directory
site:argentum.org community
site:cqc.org.uk care home
"aged care provider" directory [country]
```

수집 필드:

- Operator / Group Name
- Country / Region
- Facility Count (다점포 여부)
- Facility Type (Nursing/Assisted Living/Memory Care 등)
- Ownership Type (For-profit/Non-profit/Government)
- Website

---

### Source Level 2 — 복지기기·의료기기 유통사 디렉토리

목적: 실제 유통 채널(수출 시 1차 파트너 후보) 확보

검색 예시:

```text
"assistive technology distributor" [country]
"durable medical equipment" distributor [country]
"rehabilitation equipment supplier" [country]
"mobility aids" wholesale distributor
```

전시회 기반 탐색(전시회 참가업체 = 실제 유통상):

```text
Medtrade exhibitor list
RehaCare exhibitor list
OT World exhibitor list
Arab Health exhibitor list
```

수집 필드:

- Distributor Name
- Country / Coverage Region
- Product Categories Carried
- Existing Brand Portfolio (경쟁 제품 취급 여부)
- Import License / Medical Device Registration 보유 여부
- Website / Contact

---

### Source Level 3 — 공공 조달 포털 (Government Procurement)

목적: 실제 Tender, RFP, Award 확인 — 특히 병원/공공요양시설/보훈시설 대상

주요 포털:

- SAM.gov (미국 연방정부 — VA 보훈병원, HHS 산하기관 포함)
- TED — Tenders Electronic Daily (EU 전역)
- Find a Tender (영국)
- 국가별 조달청 사이트 (지역별 확장)

검색 키워드:

```text
"companion robot" tender
"elderly monitoring robot" RFP
"assistive robot" procurement
"care robot" solicitation
"social robot" nursing home bid
"telepresence robot" senior tender
site:sam.gov robot elderly care
site:ted.europa.eu "care robot"
```

수집 필드:

- Tender Title / Bid Number
- Issuing Agency
- Product / Quantity
- Estimated Value
- Award Date / Supplier (있는 경우)
- Official URL

중요: 국방/의료기기 조달 카테고리에서 "assistive device," "rehabilitation equipment," "telehealth device" 키워드로도 함께 검색 — 로봇이 별도 카테고리로 분류되지 않는 경우가 많다.

---

### Source Level 4 — 정부 보조금/시범사업 (가장 중요한 미래 구매 신호)

목적: **실 구매보다 선행하는 가장 강력한 신호** — 돌봄로봇 시장 특유의 소스

주요 프로그램:

- EU AAL Programme (Active and Assisted Living Programme) — 회원국 공동 R&D/실증 자금
- 일본 経済産業省(METI) / 개호로봇 개발·도입 보조금 (介護ロボット導入支援)
- 싱가포르 Agency for Integrated Care (AIC) — 로봇 도입 지원
- 각국 지자체 고령친화기술 시범사업 공고
- 미국 주정부 Medicaid Home & Community-Based Services (HCBS) waiver 프로그램 (assistive tech 포함 여부 확인)

검색 예시:

```text
site:aal-europe.eu care robot project
"介護ロボット" 導入支援 補助金
"AIC" robot pilot Singapore
"[country] government" "care robot" pilot program
"aging in place" grant robot
```

수집할 Buying Signal:

- Pilot Program 선정 시설명
- 보조금 수혜 업체/시설
- Co-funded R&D 프로젝트 참여기관
- 시범사업 이후 정식 조달 전환 여부

이 소스는 향후 12~36개월 내 정식 구매로 전환될 가능성이 있는 Buyer를 조기에 포착하는 데 사용한다.

---

### Source Level 5 — 전시회·컨퍼런스 (Trade Show)

목적: 실제 구매 담당자와의 접점, 도입 계약 발표 확인

주요 전시회:

- CES (Digital Health / AgeTech 트랙)
- iREX — International Robot Exhibition (일본)
- RehaCare / OT World (독일)
- Medtrade (미국)
- Care Show / Care England Conference (영국)
- Arab Health (중동)
- 로보월드, K-Health Industry Expo (한국 — 국내 경쟁사 동향 파악용)
- Aging2.0 Optimize Conference

검색 예시:

```text
"[전시회명]" exhibitor list care robot
"[전시회명]" award winner robot elderly
"[전시회명]" 2025 buyer program
```

전시회 수상작·바이어 프로그램 참가사는 Lead Score 가점 대상이다.

---

### Source Level 6 — 경쟁 제품 설치 기반 역추적 (Existing Vendor First)

목적: 이미 카테고리를 이해하고 구매 경험이 있는 Buyer 확보 — **가장 효율적인 소스 중 하나**

주요 경쟁/레퍼런스 제품:

```text
PARO (물개형 치료로봇)
ElliQ (Intuition Robotics)
Temi
Pepper / Zora Robotics (SoftBank 기반 케어 적용)
Qoobo
Lovot (GROOVE X)
Jibo
Mabu (Catalia Health)
Buddy (Blue Frog Robotics)
Stevie (Akara Robotics)
```

검색 예시:

```text
"PARO robot" nursing home
"ElliQ" care facility
"Temi robot" senior living
"Pepper robot" elderly care facility
"Lovot" dementia care
```

목적:

- 기존 도입 시설/유통망 확인
- 경쟁 브랜드 대비 Product Fit 차별화 포인트 파악
- Replacement/추가 도입 주기 추정

---

### Source Level 7 — 학술/임상 연구 및 산업 리포트

목적: 실제 시설 단위 Pilot·효과성 검증 사례 확인 (특히 치매케어·낙상방지 분야는 임상연구로 먼저 나타남)

검색 예시:

```text
"companion robot" nursing home clinical trial
"social robot" dementia care study facility
"robot pet" therapy elderly study
site:pubmed.ncbi.nlm.nih.gov care robot nursing home
International Federation of Robotics service robot report
```

임상연구 참여 시설은 이미 로봇 도입에 우호적인 조직문화를 가진 경우가 많아 Lead Score 가점 대상이다.

---

## 5. 검색 전략

### Strategy A — Company First

이미 운영 그룹/유통사 이름을 알고 있을 때 사용한다.

```text
"[Operator Name]" robot pilot
"[Operator Name]" companion robot
"[Operator Name]" innovation technology adoption
"[Operator Name]" assistive technology
```

### Strategy B — Product First

Buyer 이름을 모를 때 사용한다.

```text
"companion robot" "nursing home" -PARO -ElliQ
"elderly monitoring robot" distributor
"care robot" "request for proposal"
site:.gov "assistive robot" procurement
```

검색 결과에서 시설/유통사 이름을 역추출한다.

### Strategy C — Future Signal First

아직 정식 구매 전인 Buyer를 찾을 때 사용한다.

```text
"pilot program" companion robot senior
"AAL" project care robot
"grant" robot elderly care facility
"innovation fund" assistive robot
"[country] government" robot elderly pilot
```

### Strategy D — Existing Vendor First

Source Level 6과 연계 — 경쟁 제품을 이미 쓰는 조직을 찾는다.

```text
"PARO" installation nursing home
"Temi robot" hotel deployment
"Pepper robot" hospital reception
```

---

## 6. 제품 키워드 사전

### 시니어케어 로봇 (Senior Care Robot)

```text
elderly care robot
senior companion robot
social robot elderly
fall detection robot
medication reminder robot
telepresence robot senior
AAL robot
ambient assisted living robot
dementia care robot
aging in place robot
eldercare monitoring robot
rehabilitation robot elderly
```

### 로봇펫 (Robot Pet / Companion Robot)

```text
robot pet
companion pet robot
robotic seal therapy
animal-assisted therapy robot
robotic companion animal
therapeutic robot pet dementia
emotional support robot
```

### 로봇 도우미 (Service / Helper Robot)

```text
service helper robot
reception robot
guide robot facility
home assistant robot
hospitality robot
delivery robot hotel
concierge robot
autonomous mobile robot facility
```

---

## 7. Buying Signal 점수화

각 Buyer에 0~100점 Lead Score를 부여한다.

### 90–100 — Immediate Opportunity

다음 중 여러 개가 확인됨:

- 현재 Tender / RFP 진행 중이거나 유통 파트너 공개 모집 중
- 최근 12개월 내 파일럿/도입 발표
- 다점포 운영사이며 확장 계획 명시
- Budget / 보조금 확정 명시
- 담당자(Innovation/Procurement) 확인 가능

### 75–89 — Strong Opportunity

- 정부 시범사업 선정 시설/유통사
- 경쟁 제품(PARO, ElliQ 등) 기존 도입 확인 → 교체/확장 여지
- 임상연구 참여 이력
- 전시회 바이어 프로그램 참가 이력

### 55–74 — Qualified Buyer

- 다점포 요양 그룹 또는 규모 있는 의료기기 유통사
- 시니어/AgeTech 카테고리 취급 명시
- 협회 회원사로 확인됨

### 30–54 — General Target

- 요양시설/유통사라는 사실만 확인
- 구체적 도입 신호 없음

### 0–29 — Low Priority

- 카테고리 적합성 불분명 (예: 급성기 병원 중심, 로봇 무관 사업)
- 공식 근거 없음

---

## 8. 필수 출력 형식

Buyer 조사 결과는 아래 필드를 반드시 포함한다.

```markdown
## Buyer Lead

Company:
Country:
Region:
Buyer Type: (Care Facility Operator / Distributor / Retailer / Government / SI / Rental)
Facility Count / Coverage:
Website:

Target Product: (시니어케어 로봇 / 로봇펫 / 로봇 도우미)
Product Fit:

Adoption Signal: (Pilot / Grant / Tender / Trade Show / Clinical Trial / Existing Competitor Installation)
Signal Date:
Buying Stage:

Evidence Summary:
Official Source 1:
Official Source 2:

Facility Type / Care Setting:
Quantity / Scale:
Budget / Contract Value:
Current Vendor (경쟁 제품):

Innovation / Technology Department Contact:
Procurement Department Contact:

LinkedIn Search Keywords:

Lead Score:
Confidence:
Next Action:
```

---

## 9. Buying Stage 분류

각 Lead를 반드시 아래 단계 중 하나로 분류한다.

```text
1. Awareness (카테고리 인지 단계)
2. Pilot Planning
3. Pilot / Trial Funded
4. Pilot Completed / Evaluation
5. Budget Approved
6. Vendor Qualification
7. RFI / RFQ
8. RFP / Tender
9. Awarded
10. Deployed
11. Expansion / Renewal
```

영업적으로는 `Pilot Planning ~ Vendor Qualification` 단계가 특히 중요하다. 이 시장은 정식 Tender 이전에 파일럿 단계에서 벤더가 사실상 결정되는 경우가 많다.

---

## 10. LinkedIn 리서치 — 마지막 단계에서만

공식 Adoption Signal을 확보한 뒤에만 LinkedIn 담당자를 찾는다.

### Technical / Innovation Decision Makers

```text
Director of Innovation
Head of Technology / Digital Transformation
Clinical Innovation Lead
Director of Nursing
Quality & Care Improvement Manager
Assistive Technology Lead
```

### Commercial / Procurement Buyers

```text
Procurement Manager
Purchasing Director
Category Manager (Medical/Assistive)
Business Development Manager (유통사 측)
Import Manager
Distribution Partnerships Lead
```

### LinkedIn Query Example

```text
"[Operator/Distributor Name]" "Director of Innovation"
"[Operator/Distributor Name]" "Procurement Manager"
"[Operator/Distributor Name]" "Assistive Technology"
"[Operator/Distributor Name]" "Business Development"
```

가능하면 한 회사에서 최소 2명을 확보한다.

- Innovation / Clinical Decision Maker 1명
- Procurement / Commercial Buyer 1명

---

## 11. 검증 규칙

### Rule 1

보도자료·마케팅 블로그는 Adoption Evidence의 1차 근거로 사용하지 않는다. 단, 정부/협회/학술 소스가 없을 경우 보조 근거로만 사용한다.

### Rule 2

우선순위:

```text
Government Health/Welfare Agency Document
Public Procurement Portal
Academic / Clinical Study
Industry Association (LeadingAge, IFR 등)
Official Trade Show Program
Official Company/Facility Press Release
News / Secondary Source
```

### Rule 3

Pilot 또는 Grant 관련 문서가 발견되면 반드시 확인:

- 지원/발행 기관
- 발행 날짜
- 참여 시설/유통사 이름
- 로봇 카테고리 (시니어케어/로봇펫/도우미 중 어디인지)
- 예산/수량/기간
- 정식 도입 전환 여부

### Rule 4

2년 이상 지난 파일럿 문서는 현재 구매 근거가 아니라 `Historical Evidence`로 표시하고, 후속 소식(정식 도입/중단 여부)을 별도로 검색해 확인한다.

### Rule 5

로봇 카테고리 혼동에 주의한다. "care robot"이 산업용/제조업 로봇 팔을 지칭하는 경우가 흔하므로, 반드시 "elderly," "senior," "dementia," "nursing home," "companion" 등 케어 맥락 키워드와 함께 확인한다.

### Rule 6

공식 URL이 없는 Lead는 High Priority Lead로 분류하지 않는다.

---

## 12. 리서치 워크플로우

에이전트는 아래 순서로 조사한다.

```text
STEP 1
로봇 카테고리(시니어케어/로봇펫/로봇도우미)와 대상 지역을 정의한다.

STEP 2
LeadingAge / Argentum / CQC 등에서 다점포 운영 그룹 후보를 수집한다 (Tier 1).

STEP 3
경쟁 제품(PARO, ElliQ, Temi, Pepper, Lovot 등) 도입 사례를 역추적한다 (Source Level 6).

STEP 4
AAL Programme / 개호로봇 보조금 / AIC 등 정부 보조금·시범사업 정보를 검색한다 (Source Level 4).

STEP 5
SAM.gov / TED / Find a Tender에서 조달 공고·Award를 검색한다 (Source Level 3).

STEP 6
DME/Assistive Technology 유통사 디렉토리 및 전시회 참가사 리스트를 수집한다 (Tier 2, Source Level 2/5).

STEP 7
학술/임상 연구에서 파일럿 참여 시설을 확인한다 (Source Level 7).

STEP 8
Adoption Signal과 Product Fit(카테고리 적합성)을 추출한다.

STEP 9
Lead Score와 Buying Stage를 계산한다.

STEP 10
상위 Lead에 대해서만 LinkedIn 담당자를 찾는다.
```

---

## 13. 예시 검색 쿼리 세트

제품: 시니어케어 모니터링·정서케어 로봇
지역: 전체 (Global)

```text
"companion robot" "nursing home" pilot
"elderly monitoring robot" distributor -PARO
site:aal-europe.eu care robot
site:sam.gov "assistive robot" OR "care robot"
site:ted.europa.eu "social robot" elderly
"PARO robot" nursing home installation
"ElliQ" care facility deployment
LeadingAge member innovation robot
"assistive technology distributor" [country]
"介護ロボット" 導入 補助金
```

Operator/유통사가 확인된 후:

```text
"[Operator Name]" robot pilot
"[Operator Name]" innovation technology
"[Operator Name]" assistive technology adoption
"[Distributor Name]" product portfolio robot
"[Distributor Name]" import medical device
```

---

## 14. 에이전트 행동 원칙

이 스킬을 사용하는 에이전트는 다음 행동 원칙을 따른다.

1. LinkedIn 검색부터 시작하지 않는다.
2. 로봇 카테고리(시니어케어/로봇펫/로봇도우미)를 먼저 구분하고, 이후 조사 전 과정에서 이 구분을 유지한다.
3. 공식 Adoption Signal(파일럿/보조금/조달/임상연구)을 먼저 찾는다.
4. 한 개의 검색결과만 보고 Buyer라고 판단하지 않는다.
5. 반드시 공식 URL을 저장한다.
6. 개별 시설보다 다점포 운영 그룹·유통사를 우선한다.
7. 경쟁 제품의 기존 설치 기반을 적극적으로 역추적한다.
8. 오래된 파일럿 문서는 Historical Evidence로 표시하고 후속 상태를 재확인한다.
9. 회사마다 Buying Stage를 지정한다.
10. Lead Score를 산출하고, 상위 Buyer만 담당자 탐색 단계로 넘긴다.

---

## 15. 최종 목표

최종 목적은 단순한 회사 리스트가 아니다.

최종 결과물은 다음 질문에 답할 수 있어야 한다.

- 어느 카테고리(시니어케어/로봇펫/도우미)의 로봇을 필요로 하는가?
- 누가 이미 파일럿·시범사업을 진행했는가?
- 누가 정부 보조금을 받아 도입을 준비 중인가?
- 누가 경쟁 제품을 이미 쓰고 있어 교체·확장 여지가 있는가?
- 어떤 유통사가 이미 관련 카테고리를 취급하는가?
- 언제, 어떤 단계에서 접근해야 하는가?
- Innovation/Clinical 담당자는 누구인가?
- Procurement/유통 담당자는 누구인가?

최종 산출물의 핵심은 다음 구조다.

`Buyer(운영그룹/유통사) → Adoption Evidence → Buying Stage → Category Fit → Decision Maker → Next Action`
