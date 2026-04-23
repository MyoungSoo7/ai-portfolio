🔁 AI를 개발 워크플로에 통합한 사례

4단계 오케스트레이션 파이프라인: 하나의 요구사항을 15개 이상의 전문 에이전트가 순차적으로 처리하도록 설계


1. 문제 상황
개인 사이드 프로젝트를 혼자 진행하다 보면 다음과 같은 반복적인 비효율이 발생합니다.

요구사항 → 구현 사이의 누락
머릿속에 있는 "대충 이런 걸 만들자"를 바로 코드로 옮기면, 비기능 요구사항(인증·권한·에러 처리·테스트)이 체계 없이 누락됩니다.
단일 프롬프트의 한계
Claude에 "이거 만들어줘" 식으로 한 번에 요청하면, 맥락이 섞여서 아키텍처·보안·테스트 관점이 균등하게 반영되지 않습니다.
컨텍스트 리셋 비용
새 채팅마다 프로젝트 규칙·스타일·스택을 매번 재설명해야 해서 실질 작업 시간보다 맥락 주입 시간이 더 길어집니다.
검증되지 않은 코드가 그대로 커밋되는 리스크
AI가 생성한 코드를 개발자 1인이 혼자 리뷰하면, 환각(hallucination)이나 누락된 엣지 케이스를 놓치기 쉽습니다.

2. 해결 방안: 4단계 에이전트 오케스트레이션
Claude Code의 .claude/commands/ 와 커스텀 에이전트 기능을 사용해,
하나의 요구사항이 4단계를 거치며 15개 이상의 전문 에이전트에 의해 검토되는 파이프라인을 구성했습니다.
#mermaid-rfr{font-family:inherit;font-size:16px;fill:#191919;}@keyframes edge-animation-frame{from{stroke-dashoffset:0;}}@keyframes dash{to{stroke-dashoffset:0;}}#mermaid-rfr .edge-animation-slow{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 50s linear infinite;stroke-linecap:round;}#mermaid-rfr .edge-animation-fast{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 20s linear infinite;stroke-linecap:round;}#mermaid-rfr .error-icon{fill:#CC785C;}#mermaid-rfr .error-text{fill:#3387a3;stroke:#3387a3;}#mermaid-rfr .edge-thickness-normal{stroke-width:1px;}#mermaid-rfr .edge-thickness-thick{stroke-width:3.5px;}#mermaid-rfr .edge-pattern-solid{stroke-dasharray:0;}#mermaid-rfr .edge-thickness-invisible{stroke-width:0;fill:none;}#mermaid-rfr .edge-pattern-dashed{stroke-dasharray:3;}#mermaid-rfr .edge-pattern-dotted{stroke-dasharray:2;}#mermaid-rfr .marker{fill:#91918D;stroke:#91918D;}#mermaid-rfr .marker.cross{stroke:#91918D;}#mermaid-rfr svg{font-family:inherit;font-size:16px;}#mermaid-rfr p{margin:0;}#mermaid-rfr .label{font-family:inherit;color:#191919;}#mermaid-rfr .cluster-label text{fill:#3387a3;}#mermaid-rfr .cluster-label span{color:#3387a3;}#mermaid-rfr .cluster-label span p{background-color:transparent;}#mermaid-rfr .label text,#mermaid-rfr span{fill:#191919;color:#191919;}#mermaid-rfr .node rect,#mermaid-rfr .node circle,#mermaid-rfr .node ellipse,#mermaid-rfr .node polygon,#mermaid-rfr .node path{fill:#F0F0EB;stroke:#D9D8D5;stroke-width:1px;}#mermaid-rfr .rough-node .label text,#mermaid-rfr .node .label text,#mermaid-rfr .image-shape .label,#mermaid-rfr .icon-shape .label{text-anchor:middle;}#mermaid-rfr .node .katex path{fill:#000;stroke:#000;stroke-width:1px;}#mermaid-rfr .rough-node .label,#mermaid-rfr .node .label,#mermaid-rfr .image-shape .label,#mermaid-rfr .icon-shape .label{text-align:center;}#mermaid-rfr .node.clickable{cursor:pointer;}#mermaid-rfr .root .anchor path{fill:#91918D!important;stroke-width:0;stroke:#91918D;}#mermaid-rfr .arrowheadPath{fill:#0b0b0b;}#mermaid-rfr .edgePath .path{stroke:#91918D;stroke-width:2.0px;}#mermaid-rfr .flowchart-link{stroke:#91918D;fill:none;}#mermaid-rfr .edgeLabel{background-color:#F5E6D8;text-align:center;}#mermaid-rfr .edgeLabel p{background-color:#F5E6D8;}#mermaid-rfr .edgeLabel rect{opacity:0.5;background-color:#F5E6D8;fill:#F5E6D8;}#mermaid-rfr .labelBkg{background-color:rgba(245, 230, 216, 0.5);}#mermaid-rfr .cluster rect{fill:#CC785C;stroke:hsl(15, 12.3364485981%, 48.0392156863%);stroke-width:1px;}#mermaid-rfr .cluster text{fill:#3387a3;}#mermaid-rfr .cluster span{color:#3387a3;}#mermaid-rfr div.mermaidTooltip{position:absolute;text-align:center;max-width:200px;padding:2px;font-family:inherit;font-size:12px;background:#CC785C;border:1px solid hsl(15, 12.3364485981%, 48.0392156863%);border-radius:2px;pointer-events:none;z-index:100;}#mermaid-rfr .flowchartTitleText{text-anchor:middle;font-size:18px;fill:#191919;}#mermaid-rfr rect.text{fill:none;stroke-width:0;}#mermaid-rfr .icon-shape,#mermaid-rfr .image-shape{background-color:#F5E6D8;text-align:center;}#mermaid-rfr .icon-shape p,#mermaid-rfr .image-shape p{background-color:#F5E6D8;padding:2px;}#mermaid-rfr .icon-shape rect,#mermaid-rfr .image-shape rect{opacity:0.5;background-color:#F5E6D8;fill:#F5E6D8;}#mermaid-rfr .label-icon{display:inline-block;height:1em;overflow:visible;vertical-align:-0.125em;}#mermaid-rfr .node .label-icon path{fill:currentColor;stroke:revert;stroke-width:revert;}#mermaid-rfr :root{--mermaid-font-family:inherit;}Stage 4: VerifyReviewer AgentQA AgentStage 3: BuildBackend SpecialistFrontend SpecialistTest SpecialistSecurity SpecialistDocs SpecialistStage 2: DesignArchitect AgentDB DesignerAPI DesignerStage 1: DiscoverPM AgentBA Agent요구사항자연어PR + 문서 + 테스트
단계별 역할
Stage에이전트책임산출물1. DiscoverPM / BA요구사항 구조화, 유저스토리 분해, Acceptance Criteria 정의requirements.md2. DesignArchitect / DB / API아키텍처 결정(ADR), ERD, OpenAPI 스펙design/*.md, openapi.yaml3. BuildBackend / Frontend / Test / Security / Docs실제 구현, 테스트 작성, 보안 점검, 문서화코드 · 테스트 · README4. VerifyReviewer / QA코드 리뷰, Acceptance Criteria 재검증, 회귀 테스트리뷰 코멘트, QA 리포트

3. 실제 실행 예시
입력 (자연어 요구사항)

"블로그 글 임베딩을 주기적으로 업데이트하는 배치 기능을 추가하고 싶어.
마지막 임베딩 이후 변경된 글만 찾아서 재계산해야 해."

Stage 1 출력 (PM + BA 에이전트)
markdown## User Story
- As a 블로그 운영자
- I want 변경된 글만 임베딩을 재계산하는 배치를
- So that 불필요한 API 비용 없이 검색 결과를 최신으로 유지할 수 있다.

## Acceptance Criteria
1. Given 블로그 글 100개가 있고 그중 3개가 어제 이후 수정됨
   When 배치를 실행하면
   Then 3개에 대해서만 임베딩이 재계산되어야 한다.
2. 배치 실패 시 실패한 글 단위로 재시도 가능해야 한다.
3. 실행 로그는 최소 30일 보관되어야 한다.
Stage 2 출력 (Architect 에이전트)

ADR-014: "Outbox 테이블 대신 updated_at 컬럼 비교 방식 채택 (이유: 단일 인스턴스 환경)"
인터페이스: EmbeddingBatchPort (헥사고날 아키텍처)
OpenAPI: POST /admin/embeddings/sync 스펙 자동 생성

Stage 3 출력 (Specialists)

Backend: FastAPI 배치 엔드포인트 + Celery 태스크 구현
Test: pytest 기반 9개 테스트 케이스 (정상/실패/재시도/대량)
Security: 관리자 토큰 검증 로직 추가, 민감정보 로그 마스킹

Stage 4 출력 (Reviewer)
✅ ADR-014의 updated_at 기반 방식, 요구사항과 일치
⚠️  test_retry_on_partial_failure 에서 재시도 횟수 제한이 없음 → 무한 루프 위험
⚠️  로그 레벨이 INFO인데 개인정보 포함 가능 → DEBUG 이하 권장
✅ Acceptance Criteria 1, 2 통과 / 3은 로그 보관 정책 미확인
→ 르무엘(본인)이 이 리뷰 코멘트를 받아 최종 판단을 내리고 머지합니다.

4. 디렉토리 구조
.claude/
└── commands/
    ├── 01-discover/
    │   ├── pm.md                    # PM 에이전트 (요구사항 구조화)
    │   └── ba.md                    # BA 에이전트 (유저스토리 분해)
    ├── 02-design/
    │   ├── architect.md             # 아키텍처 결정
    │   ├── db-designer.md           # ERD / 스키마
    │   └── api-designer.md          # OpenAPI 스펙
    ├── 03-build/
    │   ├── backend-specialist.md
    │   ├── frontend-specialist.md
    │   ├── test-specialist.md
    │   ├── security-specialist.md
    │   └── docs-specialist.md
    ├── 04-verify/
    │   ├── reviewer.md              # 코드 리뷰어
    │   └── qa.md                    # Acceptance Criteria 재검증
    └── _shared/
        ├── project-context.md       # 공통 프로젝트 맥락
        ├── coding-style.md          # 코딩 스타일 규칙
        └── tech-stack.md            # 승인된 스택 목록
각 .md 파일은 시스템 프롬프트 + 입력/출력 계약 + 실패 조건을 포함합니다.
(실제 프롬프트 구조는 03_prompt_templates.md 참조)

5. 본인 기여도 명시
영역기여 비율설명파이프라인 아키텍처 설계100%4단계 구조, 에이전트 분할 기준에이전트 프롬프트 작성80%초안은 Claude 생성, 최종 튜닝/제약조건은 본인공통 컨텍스트 문서(_shared/)100%프로젝트 규칙·스타일 본인 정의실제 실행·리뷰·머지 판단100%AI 산출물은 항상 본인이 최종 검증트러블슈팅 / 파이프라인 개선100%사용하면서 발견한 문제를 반복적으로 개선

6. 효과 (정량)
지표Before (단일 프롬프트)After (4단계 파이프라인)개선요구사항 → 머지 가능한 PR 평균 소요3~4시간1시간 내외약 70% 단축테스트 누락률 (기능 구현 대비)약 40%약 5%87% 감소리뷰 반려 / 재작업 횟수 (PR당)평균 2.1회평균 0.4회80% 감소환각(hallucination) 발견 건수세션당 2~3건세션당 0~1건Reviewer 단계에서 대부분 차단
수치는 2025.08 ~ 2026.03 개인 프로젝트 기준 자체 측정치.

7. 트러블슈팅 경험
🐛 문제 1: Stage 2의 Architect 에이전트가 이전 결정을 무시하고 매번 다른 구조 제안
원인: 각 에이전트가 독립 세션으로 실행되어 과거 ADR을 참조하지 못함.
해결:

docs/adr/ 디렉토리를 모든 Stage 2 에이전트의 컨텍스트에 주입
프롬프트에 "새 ADR 작성 전 기존 ADR과의 충돌 여부를 먼저 검증할 것" 명시
결과: 이전 결정과 충돌하는 설계 제안 빈도 주 5회 → 0회

🐛 문제 2: Reviewer가 "LGTM" 만 남기고 실제 검증을 건너뜀
원인: 프롬프트에 리뷰 기준이 추상적이어서 AI가 안일한 응답을 반환.
해결:

체크리스트 형태로 강제: Acceptance Criteria 1번: ✅/❌/⚠️ + 근거 라인 번호
모든 항목에 대해 반드시 근거 코드 라인을 인용하도록 포맷 강제
결과: 리뷰 코멘트 평균 2줄 → 15줄, 실제 버그 발견율 상승

🐛 문제 3: 15개 에이전트 프롬프트의 중복·버전 불일치
원인: 공통 규칙(코딩 스타일 등)이 각 에이전트 파일에 복붙되어 수정 시 누락 발생.
해결:

_shared/ 디렉토리에 공통 규칙을 단일화하고, 각 에이전트가 @import 스타일로 참조
공통 규칙 변경 시 한 파일만 수정하면 전체 파이프라인에 반영
결과: 스타일 불일치 이슈 월 8건 → 0건


8. 배운 점 & 한계
배운 점

AI는 "한 번에 잘 쓰는 법"보다 "실패해도 다음 단계에서 잡히도록 설계하는 법" 이 훨씬 중요하다.
프롬프트는 코드처럼 버전 관리되고, 중복 제거되고, 테스트되어야 한다.
에이전트 분리는 결국 관심사의 분리(SoC) 와 같은 원리다. 모놀리식 프롬프트 → 마이크로 에이전트.

한계

Claude Code Max 20x 플랜의 요청 한도 안에서만 동작 (팀 단위 확장 시 비용 재설계 필요)
Stage 간 상태 전달이 파일 시스템 기반 → 대규모 프로젝트에선 별도 orchestrator 필요
에이전트 품질은 프롬프트 품질에 선형으로 의존 → 초기 세팅 비용이 큼


9. 관련 블로그 포스팅

하네스 엔지니어링: Agent = Model + Harness (Tistory)
폴리글랏 MSA 환경에서의 AI 오케스트레이션 (Velog)

10. 링크

GitHub Repository: [공개 예정 / 제출 시 링크 업데이트]
데모 영상: [파이프라인 실행 화면 GIF 또는 Loom]
