name: 유틸리티바이어소스리서치
description: 전력기기(개폐기/Switchgear, 리클로저/Recloser, 차단기/Circuit Breaker, RMU, 변압기 등)를 실제로 구매하거나 향후 구매할 가능성이 높은 Utility/Power Company/Municipal Utility/Electric Cooperative/Grid Operator/Power Plant 등 대형 전력 수요처를, 링크드인이 아니라 정부·유틸리티·규제기관의 공식 문서(입찰, 낙찰, 자본투자계획, 승인벤더목록, 엔지니어링 표준)에서 먼저 찾아내는 스킬. "공식 소스 → 구매 증거(Buying Signal) → 미래 구매 신호 → 기술 사양 → Vendor 정보 → 담당 조직 → 링크드인 담당자" 순서로 조사하며, 링크드인은 상위 리드(Lead Score 상위)에 대해서만 담당자를 찾는 마지막 단계로만 사용한다. "수출바이어파인더" 스킬이 이미 확보된 링크드인 PDF를 분류하는 스킬이라면, 이 스킬은 그 PDF 자체가 없을 때 "누구를 링크드인에서 검색해야 하는지"를 증거 기반으로 먼저 찾아내는 전 단계 스킬이다.

# 유틸리티바이어소스리서치 스킬

## 0. 이 스킬과 "수출바이어파인더"의 관계

이 프로젝트에는 바이어를 찾는 스킬이 두 개 있고, 서로 다른 지점에서 출발한다.

| | 수출바이어파인더 | 유틸리티바이어소스리서치 (이 스킬) |
|---|---|---|
| **출발점** | 이미 확보된 링크드인 검색결과 PDF | 제품/시장만 정해져 있고 PDF는 없음 |
| **방법** | PDF를 파싱해 인물 단위로 등급 분류 | 정부·유틸리티 공식 문서에서 "실제 구매 증거"를 먼저 찾음 |
| **강점** | 이미 모인 대량의 인물을 빠르게 등급화 | 근거가 확실한(공식 URL이 있는) 리드를 처음부터 만들어냄, 담당자뿐 아니라 "언제 접근해야 하는지(Buying Stage)"까지 알 수 있음 |
| **약점** | 링크드인 검색 알고리즘이 걸러준 사람들에 의존, 구매 증거 없이 조직·직함만으로 등급 매김 | 시간이 더 걸림, 미국 외 지역은 공식 소스 정비 수준에 따라 편차가 큼 |
| **산업 적합도** | 소비재(뷰티 등)와 산업재(전기장비 등) 모두 적용 가능 | **입찰·조달 기록이 공개되는 산업**(공공/규제 유틸리티, 정부조달, 발전·송배전 등)에 특히 강함 |

**두 스킬은 서로 이어진다**: 이 스킬의 마지막 단계(11절)가 "상위 Lead에 대해서만 링크드인 담당자를 찾는다"이므로, 여기서 나온 Lead Score 상위 유틸리티들을 대상으로 링크드인 검색을 돌려 PDF로 내보낸 뒤 **"수출바이어파인더" 스킬로 인물 단위 분류를 이어갈 수 있다.** 반대로 이미 "수출바이어파인더"로 만든 리스트에서 특정 조직(예: A/B등급 최종수요자)의 실제 구매 증거·구매 시점을 더 깊이 확인하고 싶을 때, 이 스킬의 5절(공식 소스 계층) 이하를 그 조직 하나에 대해서만 수행할 수도 있다.

## 1. 기본 정보
- 스킬 이름: 유틸리티바이어소스리서치 (Utility & Power Equipment Buyer Source Research)
- 한 줄 요약: 전력기기 제품군과 시장(국가)을 입력받아, 정부·유틸리티·규제기관의 공식 문서에서 실제 구매 증거(입찰/낙찰/자본계획/승인벤더목록)를 단계별로 찾아 리드 스코어(0~100점)와 구매단계(Buying Stage)를 매기고, 상위 리드에 한해서만 마지막 단계로 링크드인에서 기술/구매 담당자를 찾는다.
- 카테고리: 해외영업 / 바이어(Buyer) 발굴 / 산업재·전력기기 조달 리서치
- 버전: v1.0
- 출처: 사용자가 제공한 `utility_buyer_source_research_skill.md`(Electric_equipment 폴더)를 이 프로젝트의 스킬 형식에 맞게 재구성하고, "수출바이어파인더" 스킬과의 연계 지점을 추가함.

## 2. 호출 방식
- 직접 호출 명령어: /유틸리티바이어소스리서치
- 자동 인식 키워드
  1. "이 전력기기(개폐기/리클로저/차단기/RMU/변압기) 살 만한 유틸리티 찾아줘"
  2. "공식 문서 기반으로 바이어 찾아줘" / "입찰·낙찰 기록으로 리드 만들어줘"
  3. "이 회사가 진짜 구매할지 증거부터 확인해줘"
  4. "Lead Score 매겨줘" / "Buying Stage 확인해줘"
- 이 스킬을 쓰면 안 되는 상황
  - 이미 링크드인 PDF가 있고 단순히 그 인물들을 분류만 하면 되는 경우 → "수출바이어파인더" 스킬을 바로 사용한다(이 스킬을 거칠 필요 없음)
  - 대상 시장이 공식 조달 문서를 거의 공개하지 않는 국가/산업인 경우 → 5절의 공식 소스 상당수(SAM.gov, PUC 등)가 미국 특화이므로, 다른 국가에 적용할 때는 그 나라의 상응 기관(예: 규제위원회, 공기업 조달포털)을 먼저 찾아 대체해야 하며, 대체 소스를 찾지 못하면 이 스킬의 신뢰도가 크게 떨어진다는 점을 사용자에게 미리 알린다
  - 소비재(뷰티 등) 리서치인 경우 → 이 스킬은 "공식 입찰·자본계획 문서"가 존재하는 산업재/공공조달형 산업에 특화되어 있어 소비재에는 적합하지 않음("수출바이어파인더"를 사용한다)

## 3. 입력 파라미터
- 필수 입력값
  - 대상 제품군 (예: DC Switchgear, Circuit Recloser, Circuit Breaker, Solid Dielectric Switchgear, RMU, Transformer — 7절의 키워드 사전에서 선택하거나 새로 정의)
  - 대상 시장/국가 (예: 미국, 특정 주(state); 미국 외 지역은 상응하는 공식 소스를 먼저 파악해야 함)
- 선택 입력값
  - 우선 조사할 Buyer Tier (4절의 Tier 1~4 중 특정 티어만 좁혀서 조사할 수 있음, 기본값: Tier 1부터 순서대로)
  - Lead Score 하한선 (기본값: 55점 이상만 최종 리포트에 포함, 30~54점은 "일반 타겟"으로 별도 목록화)
  - 링크드인 담당자 탐색까지 진행할지 여부 (기본값: Lead Score 75점 이상만 진행)
- 입력값이 여러 개일 때 우선순위 처리 규칙
  1. 제품군이 여러 개면 4절의 "특히 적합한 제품" 매핑에 따라 Buyer Tier를 나눠서 조사한다(예: Recloser는 Tier 1 배전유틸리티 우선, 초고압 Circuit Breaker는 Tier 2 송전망운영사 우선).
  2. 공식 소스 검색 결과가 여러 개면 12절 Rule 2의 우선순위(공식 유틸리티 문서 > 정부조달 > 규제기관 제출자료 > 정부DB > 엔지니어링표준 > 보도자료 > 업계협회 > 뉴스/2차소스)를 따른다.
  3. 오래된 문서와 최신 문서가 충돌하면 12절 Rule 4에 따라 오래된 것은 "Historical Evidence"로만 표시하고 현재 구매 근거로 쓰지 않는다.

## 4. 대상 바이어 유형 (Target Buyer Types)

### Tier 1 — 배전 유틸리티 (가장 우선 조사)
Electric Utility / Electric Distribution Company / Distribution Utility / Municipal Utility / Public Power Utility / Electric Cooperative / Distribution System Operator / Distribution Network Operator
- 특히 적합한 제품: Circuit Recloser, RMU, Solid Dielectric Switchgear, Medium Voltage Circuit Breaker, Distribution Automation Equipment

### Tier 2 — 송전/계통운영사
Transmission Utility / Transmission System Operator / Grid Operator / Power Transmission Company
- 특히 적합한 제품: High/Medium Voltage Circuit Breaker, Switchgear, Substation Equipment, Protection Equipment

### Tier 3 — 발전사
Power Generation Company / Independent Power Producer / Thermal·Hydroelectric·Nuclear Power Plant / Renewable Energy Operator
- 특히 적합한 제품: Circuit Breaker, Switchgear, DC Switchgear, Substation Equipment

### Tier 4 — 대형 전력소비처
Data Center / BESS Operator / Solar·Wind Operator / Railway·Metro / Mining / Steel / Petrochemical / Large Industrial Facility

## 5. 공식 소스 계층 (Official Source Hierarchy) — 이 순서대로 조사한다

### Level 1 — Utility Master Lists (전체 Buyer 모집단 확보)
미국: EIA(U.S. Energy Information Administration), APPA(American Public Power Association), NRECA(National Rural Electric Cooperative Association), 주별 Public Utility Commission/Public Service Commission
```text
site:eia.gov electric utility [state]
site:publicpower.org utility [state]
site:electric.coop cooperative [state]
```
수집 필드: Utility Name / State / Utility Type / Customer Count / Service Territory / Ownership Type / Website

### Level 2 — Utility/City Procurement Portals (실제 Tender/Bid/Award/Contract 확인)
```text
"[Utility Name]" procurement / bids / solicitations / contracts / purchasing / vendor
"[Utility Name]" recloser bid / switchgear bid / circuit breaker procurement / RMU solicitation / transformer contract
```
Municipal Utility는 Utility 사이트뿐 아니라 City Procurement Portal / City Finance·Purchasing Department / County Procurement Portal도 반드시 확인한다.
수집 필드: Solicitation Title / Bid Number / Issue Date / Closing Date / Award Date / Product / Quantity / Estimated Value / Award Amount / Supplier / Buyer(Procurement Contact) / 공식 URL

### Level 3 — SAM.gov (미국 연방정부·군·연방시설)
문서 유형: Presolicitation / Solicitation / Invitation for Bid / Request for Proposal / Award Notice / Sole Source Notice
검색 키워드: recloser, circuit recloser, automatic recloser, medium voltage switchgear, MV switchgear, solid dielectric switchgear, ring main unit, RMU, circuit breaker, vacuum circuit breaker, distribution switchgear, substation equipment
**중요**: 현재 Tender뿐 아니라 과거 Award도 확인한다 — 실제 Buyer/Supplier/Contract Value/반복구매 여부/사용 제품군을 알 수 있다.

### Level 4 — State/City/County Procurement
```text
site:.gov "recloser" "bid" / "invitation for bid"
site:.gov "switchgear" "solicitation"
site:.gov "circuit breaker" "bid"
site:.gov "ring main unit" / "solid dielectric switchgear"
```
함께 검색: IFB, RFP, RFQ, Solicitation, Bid, Award, Contract, Purchase Order, Vendor

### Level 5 — Public Utility Commission/Public Service Commission (현재 구매보다 더 중요한 "미래 구매 신호")
미국 IOU는 대규모 설비투자 계획을 주 규제기관에 제출하는 경우가 많다.
```text
site:cpuc.ca.gov recloser PG&E
site:cpuc.ca.gov switchgear PG&E
site:cpuc.ca.gov circuit breaker SCE
site:cpuc.ca.gov distribution upgrade/investment project
```
중요 문서명: General Rate Case, Capital Investment Plan, Distribution System/Resource Plan, Integrated Distribution Plan, Grid Modernization Plan, Reliability Improvement Plan, Infrastructure Replacement Plan, Asset Management Plan, Wildfire Mitigation Plan
수집할 Buying Signal: Recloser replacement, Feeder automation, Substation upgrade, Breaker replacement, Switchgear modernization, Distribution automation, Grid hardening, Wildfire mitigation, Underground distribution, Asset replacement program

### Level 6 — U.S. DOE/Grid Modernization Sources (현대화에 적극적인 Utility 확인)
기관: U.S. Department of Energy, Office of Electricity, NETL, Grid Deployment Office
```text
site:energy.gov recloser/switchgear utility
site:energy.gov "distribution automation" / "automatic recloser"
site:energy.gov "grid modernization" / "smart grid" utility
```
이 소스는 직접 입찰보다 "현대화 적극성 / DA투자여부 / 스마트그리드 경험 / 연방보조금 수혜여부 / 향후 추가투자 가능성" 판단에 사용한다.

### Level 7 — Utility Engineering Standards/Technical Documents (실제 사양·승인 브랜드 확인)
```text
"[Utility Name]" recloser/switchgear specification / standard / approved recloser / approved manufacturers
"[Utility Name]" material/technical/distribution/engineering standard
```
반드시 찾을 문서: Approved Manufacturer List, Approved Vendor List, Qualified Products List, Preferred Equipment List, Material/Technical Specification, Equipment Standard, Distribution Construction Standard, Engineering Standard, Design/Construction Manual
추출 필드: Voltage Class, Current Rating, Interrupting Rating, Insulation Type, Control Type, Communication Protocol, Required Standard(예: IEEE C37.60), Approved Manufacturer/Model, Required Certification(예: 15/27/38 kV Recloser, Vacuum Interruption, Solid Dielectric Insulation, SCADA Compatible)

## 6. 검색 전략 (Search Strategy)

- **전략 A — Company First**: 이미 Utility 이름을 알 때. 예) `"Austin Energy" recloser bid`, `"Austin Energy" recloser specification`, `"Austin Energy" distribution automation`, `"Austin Energy" switchgear contract`
- **전략 B — Product First**: Buyer 이름을 모를 때. 예) `site:.gov "recloser" "bid"`, `site:.gov "medium voltage switchgear" procurement`, `site:.gov "solid dielectric switchgear"` — 검색결과에서 Utility 이름을 역추출한다.
- **전략 C — Future Signal First**: 아직 입찰 전인 Buyer를 찾을 때. 예) `"recloser replacement" utility`, `"grid modernization" recloser`, `"capital improvement plan" switchgear`, `"wildfire mitigation" recloser`
- **전략 D — Existing Vendor First**: 경쟁사 제품을 쓰는 Utility를 찾을 때(기존 설치기반·경쟁브랜드·Replacement Cycle 추정). 예) `"Eaton recloser" utility`, `"S&C recloser" utility`, `"G&W recloser" utility`, `"ABB switchgear" utility`, `"Schneider RMU" utility`

## 7. 제품 키워드 사전

- **Recloser**: recloser, circuit recloser, automatic recloser, electronic recloser, pole mounted recloser, three/single phase recloser, substation recloser, distribution recloser
- **Switchgear**: switchgear, medium voltage switchgear, MV switchgear, solid dielectric/insulated switchgear, metal clad switchgear, pad mounted switchgear
- **RMU**: ring main unit, RMU, solid insulated RMU, gas insulated RMU, medium voltage ring main unit
- **Circuit Breaker**: circuit breaker, medium voltage circuit breaker, MV circuit breaker, vacuum circuit breaker, power circuit breaker, substation circuit breaker
- **DC Switchgear**: DC switchgear, DC circuit breaker, battery switchgear, BESS switchgear, solar DC switchgear, rail DC switchgear

## 8. 작업 절차 (Research Workflow)

1. 시장/국가/제품을 정의한다.
2. EIA/APPA/NRECA 또는 해당 국가 Utility Directory에서 Buyer 후보를 수집한다(Level 1).
3. 각 Buyer의 공식 Procurement Portal을 검색한다(Level 2).
4. 제품명 + Bid/Contract/Award를 검색한다(Level 2~3, 미국 연방시설은 SAM.gov 포함).
5. PUC/PSC에서 Capital Plan/Grid Modernization Plan을 검색한다(Level 5).
6. DOE/정부 Grid Modernization 자료를 검색한다(Level 6).
7. Engineering Standard/Approved Vendor 문서를 검색한다(Level 7).
8. Buying Signal과 Product Fit을 추출한다.
9. Lead Score(9절)를 계산하고 Buying Stage(10절)를 지정한다.
10. **상위 Lead에 대해서만** 링크드인 담당자를 찾는다(11절) — 이 단계에서 나온 검색결과를 PDF로 내보내면 "수출바이어파인더" 스킬로 넘겨 인물 단위 분류를 이어갈 수 있다.

## 9. Lead Score 산정 (0~100점)

- **90–100 (Immediate Opportunity)**: 현재 Tender/RFP 진행중 + 최근 12개월 내 Award + 제품 Quantity 명시 + Budget 명시 + Procurement Contact 확인 중 다수 충족
- **75–89 (Strong Opportunity)**: Capital Plan에 제품 명시, Grid Modernization 프로젝트 존재, Recloser/Switchgear 교체 프로그램 존재, Engineering Specification 공개, 반복적 과거구매 확인
- **55–74 (Qualified Buyer)**: Utility 규모 큼, 해당 제품 사용 확인, Approved Equipment List 존재, Distribution Automation 투자 이력
- **30–54 (General Target)**: Utility라는 사실만 확인, 구체적 구매 신호 없음
- **0–29 (Low Priority)**: 제품 적합성 불분명, 공식 근거 없음

## 10. Buying Stage 분류 (반드시 하나로 지정)

```text
1. Planning
2. Budget Approved
3. Engineering / Specification
4. Vendor Qualification
5. RFI / RFQ
6. RFP / IFB
7. Bid Open
8. Awarded
9. Installation
10. Replacement / Renewal
```
영업적으로는 **Planning ~ Vendor Qualification** 단계가 특히 중요하다 — Tender가 공개된 뒤에는 신규 Vendor가 참여하기 어려울 수 있기 때문이다.

## 11. 링크드인 리서치 — 반드시 마지막 단계에서만

공식 Buying Signal을 확보한 뒤에만 링크드인 담당자를 찾는다.

- **기술 의사결정자**: Distribution Engineer, Distribution Engineering Manager, Protection Engineer, Protection & Control Engineer, Substation Engineer, Grid Engineer, Asset Manager, Asset Management Director, Director of Distribution, Director of Engineering, Grid Modernization Manager
- **구매 담당자**: Procurement Manager/Director, Purchasing Manager, Strategic Sourcing Manager, Category Manager, Supply Chain Manager, Vendor Manager
- 검색 예: `"[Utility Name]" "Distribution Engineer"`, `"[Utility Name]" "Procurement Manager"`, `"[Utility Name]" "Strategic Sourcing"`
- 가능하면 한 회사에서 최소 2명(기술 의사결정자 1명 + 구매/커머셜 담당자 1명)을 확보한다.
- 여기서 찾은 인물들을 링크드인 검색결과 PDF로 저장하면, **"수출바이어파인더" 스킬**의 파싱·분류 절차를 그대로 이어서 쓸 수 있다.

## 12. 출력 형식

Buyer 조사 결과는 아래 필드를 반드시 포함한다.

```markdown
## Buyer Lead

Company:
Country:
State / Region:
Buyer Type:
Website:

Target Product:
Product Fit:

Buying Signal:
Buying Signal Date:
Buying Stage:

Evidence Summary:
Official Source 1:
Official Source 2:

Voltage / Specification:
Quantity:
Budget / Contract Value:
Current Supplier:
Approved Manufacturer:

Technical Department:
Procurement Department:

LinkedIn Search Keywords:

Lead Score:
Confidence:
Next Action:
```

여러 Buyer를 한 번에 조사했다면 위 카드들을 모은 뒤, "수출바이어파인더"의 엑셀 산출물과 형식을 맞추기 위해 표(No/Company/Country/Buyer Type/Lead Score/Buying Stage/Evidence URL/Next Action)로도 함께 정리하는 것을 권장한다.

## 13. 자체 검증 절차 (결과를 내놓기 전에 반드시 수행 — 원문 12절 Verification Rules)

### 검증 1: 근거 소스 등급 확인
블로그, 뉴스, Distributor 페이지는 Buyer Evidence의 1차 근거로 사용하지 않는다. 우선순위는 다음과 같다: Official Utility Document > Government Procurement > Regulatory Filing > Government Database > Utility Engineering Standard > Official Press Release > Industry Association > News/Secondary Source. 이 순서를 어긴 리드가 있으면 근거를 상위 소스로 교체하거나 Lead Score를 낮춘다.

### 검증 2: 문서 메타데이터 확인
PDF가 검색되면 반드시 발행 기관/발행 날짜/프로젝트 이름/제품명/Utility 이름/예산·수량·일정을 확인했는지 점검한다. 누락된 항목이 있으면 "정보 불완전"으로 표시하고 Lead Score를 보수적으로 매긴다.

### 검증 3: 시의성 확인
오래된 문서는 현재 구매 근거가 아니라 "Historical Evidence"로 표시했는지 확인한다. Tender 종료 여부(마감일 경과 여부)를 반드시 재확인한다.

### 검증 4: 최소 2개 독립 소스 교차검증 (가능한 경우)
핵심 원칙(3절)에 따라 가능하면 최소 2개의 독립된 공식 소스로 교차 검증했는지 확인한다. 소스가 1개뿐인 리드는 "Confidence: Low"로 명시한다.

### 검증 5: URL 존재 확인
공식 URL이 없는 Lead를 High Priority(Lead Score 75점 이상)로 분류하지 않았는지 최종 점검한다.

## 14. 예외 처리 규칙

- 공식 소스가 거의 없는 국가/시장: 5절의 미국 특화 소스(SAM.gov, PUC 등)를 그대로 쓰지 말고, 그 나라의 상응 기관(전력 규제위원회, 국영 유틸리티 공시 시스템, 정부 조달포털)을 먼저 식별한다. 상응 기관을 찾지 못하면 Lead Score 상한을 55점(Qualified Buyer)으로 제한하고 그 이유를 명시한다.
- 문서는 찾았지만 최신 여부가 불확실할 때: 발행일을 반드시 표기하고, 12개월 이상 지난 자료는 "Historical Evidence"로 구분해 Buying Stage 판단에서 제외한다.
- Buying Signal과 Technical Fit이 상충할 때(예: 전압 규격이 자사 제품과 안 맞음): Lead Score를 매기기 전에 Product Fit을 먼저 재확인하고, 맞지 않으면 조사 대상에서 제외하거나 별도로 "규격 불일치"로 표시한다.
- 사용자가 "빨리 링크드인부터 찾아달라"고 요청해도: 3절의 핵심 원칙(링크드인은 마지막 단계)을 임의로 생략하지 않는다. 다만 이미 공식 소스 조사가 끝난 상위 Lead가 있다면 그 범위 내에서는 곧바로 11절로 진행할 수 있다.

## 15. 연계 가능한 다른 스킬
- **수출바이어파인더**: 이 스킬의 11절(링크드인 최종 단계) 결과물을 PDF로 저장하면 그 스킬로 이어서 인물 단위 등급 분류를 할 수 있다. 반대로 "수출바이어파인더"로 이미 만든 A/B등급(최종수요자) 리스트 중 특정 조직의 구매 증거·구매 시점을 더 깊이 확인하고 싶을 때 이 스킬의 5절 이하를 그 조직 하나에 적용할 수 있다.
- **리테일바잉시그널모니터링**: 이 스킬과 원리(증거 우선, 링크드인은 최종단계)는 동일하지만, 대상 산업이 공개 입찰이 없는 소비재/리테일(뷰티 등)일 때는 이 스킬 대신 그 스킬을 쓴다. 5절의 "공식 소스 계층"이 입찰·자본계획 대신 수입통관데이터·벤더포털·채용공고로 대체된 버전이다.
- 향후 미구현 스킬(사용자 로드맵): 상세 연락처 조사, 컨택 메시지 작성(이 스킬의 Buying Stage·Evidence Summary를 메시지에 인용하면 설득력이 높아짐), 관계 유지·네트워크 관리.

## 16. 최종 목표

최종 결과물은 단순한 회사 리스트가 아니라 아래 질문에 답할 수 있어야 한다: 누가 이 제품을 실제로 사용하는가? 누가 최근 구매했는가? 누가 앞으로 구매할 가능성이 높은가? 어떤 Specification을 요구하는가? 기존 Vendor는 누구인가? Vendor Qualification이 필요한가? 언제 접근해야 하는가? 기술 담당자와 구매 담당자는 각각 누구인가?

최종 산출물의 핵심 구조: `Buyer → Buying Evidence → Buying Stage → Technical Fit → Decision Maker → Next Action`

## 17. 예시

### 예시 입력/절차 (제품: Circuit Recloser, 시장: 미국)
- Level 1: EIA/APPA/NRECA에서 미국 전역 배전유틸리티 후보 수집
- Level 2~3: 각 후보의 Procurement Portal + SAM.gov에서 `"recloser" "bid"/"award"` 검색
- Level 5: `site:cpuc.ca.gov recloser PG&E` 등으로 대형 IOU의 Capital Investment Plan에서 recloser replacement 신호 탐색
- Level 7: `"[Utility Name]" recloser specification` / `approved manufacturers` 로 승인벤더목록·전압규격(예: 15/27/38 kV) 확인
- Lead Score 산정 후 75점 이상 리드에 한해 11절대로 링크드인에서 "Distribution Engineer"·"Procurement Manager" 검색 → PDF로 저장 → "수출바이어파인더" 스킬로 인계

## 18. 변경 이력
- v1.0 (2026-08-11): 사용자가 제공한 `utility_buyer_source_research_skill.md`(Electric_equipment 폴더)의 내용을 이 프로젝트의 스킬 형식(기본정보/호출방식/입력파라미터/작업절차/출력형식/자체검증/예외처리/연계스킬/예시/변경이력)에 맞게 재구성해 최초 작성함. 원문의 공식 소스 계층(Level 1~7), Buyer Tier(4단계), 검색 전략(A~D), 제품 키워드 사전, Lead Score(0~100), Buying Stage(1~10), 출력 형식, Verification Rules, Research Workflow, Agent Behavior 원칙을 모두 보존했고, "0. 이 스킬과 수출바이어파인더의 관계" 절을 새로 추가해 두 스킬이 어떻게 이어지는지(공식소스 리서치 → 링크드인 최종단계 → PDF 인계 → 인물단위 분류) 명시함.
