🔍 LLM 기반 기능 개발 경험

블로그 RAG 검색 시스템: 개인 블로그(Tistory / Velog)에 게시한 300여 개의 기술 포스트를
임베딩 기반 의미 검색으로 질의할 수 있는 개인 지식 검색 시스템


1. 배경
9년간 작성한 기술 블로그 포스트가 Tistory와 Velog 양쪽에 흩어져 있습니다.
하지만 몇 달 뒤 같은 주제를 다시 찾으려고 하면 키워드 검색으로는 한계가 있었습니다.

"Kafka 컨슈머 리밸런싱 이슈 해결했던 글"  → 정확한 키워드가 기억나지 않으면 못 찾음
"예전에 작성한 헥사고날 아키텍처 정리" → 제목과 본문 용어가 달라 검색 누락
두 플랫폼을 각각 검색해야 하는 불편함

→ 내 기술 자산을 의미 기반으로 검색할 수 있는 개인 RAG 시스템을 직접 구축하기로 했습니다.

2. 기술 스택
레이어기술선정 이유LLMAnthropic Claude (claude-haiku-4-5)비용 대비 한국어 품질, Anthropic API 사용 경험 확장Embeddingtext-embedding-3-small (OpenAI)한국어 지원, 1536차원 대비 저렴Vector StoreChromaDB (self-hosted)로컬 실행, Docker 배포 용이, 무료BackendFastAPI + Pydantic타입 안전성, OpenAPI 자동 생성SchedulerAPScheduler블로그 크롤링 배치InfraDocker + k3s (홈 서버) + Cloudflare Zero Trust외부 접근은 Zero Trust로만 허용Domainlemuel.co.kr개인 도메인
선정 이유 상세

Pinecone/Weaviate 대신 ChromaDB 선택 → 포스트 수백 개 규모에선 성능 오버엔지니어링이고, 무료/로컬이 우선
LangChain 전체 의존 대신 필요한 부분만 직접 구현 → 의존성 관리가 단순하고 디버깅이 쉬움


3. 아키텍처
#mermaid-rgi{font-family:inherit;font-size:16px;fill:#191919;}@keyframes edge-animation-frame{from{stroke-dashoffset:0;}}@keyframes dash{to{stroke-dashoffset:0;}}#mermaid-rgi .edge-animation-slow{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 50s linear infinite;stroke-linecap:round;}#mermaid-rgi .edge-animation-fast{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 20s linear infinite;stroke-linecap:round;}#mermaid-rgi .error-icon{fill:#CC785C;}#mermaid-rgi .error-text{fill:#3387a3;stroke:#3387a3;}#mermaid-rgi .edge-thickness-normal{stroke-width:1px;}#mermaid-rgi .edge-thickness-thick{stroke-width:3.5px;}#mermaid-rgi .edge-pattern-solid{stroke-dasharray:0;}#mermaid-rgi .edge-thickness-invisible{stroke-width:0;fill:none;}#mermaid-rgi .edge-pattern-dashed{stroke-dasharray:3;}#mermaid-rgi .edge-pattern-dotted{stroke-dasharray:2;}#mermaid-rgi .marker{fill:#91918D;stroke:#91918D;}#mermaid-rgi .marker.cross{stroke:#91918D;}#mermaid-rgi svg{font-family:inherit;font-size:16px;}#mermaid-rgi p{margin:0;}#mermaid-rgi .label{font-family:inherit;color:#191919;}#mermaid-rgi .cluster-label text{fill:#3387a3;}#mermaid-rgi .cluster-label span{color:#3387a3;}#mermaid-rgi .cluster-label span p{background-color:transparent;}#mermaid-rgi .label text,#mermaid-rgi span{fill:#191919;color:#191919;}#mermaid-rgi .node rect,#mermaid-rgi .node circle,#mermaid-rgi .node ellipse,#mermaid-rgi .node polygon,#mermaid-rgi .node path{fill:#F0F0EB;stroke:#D9D8D5;stroke-width:1px;}#mermaid-rgi .rough-node .label text,#mermaid-rgi .node .label text,#mermaid-rgi .image-shape .label,#mermaid-rgi .icon-shape .label{text-anchor:middle;}#mermaid-rgi .node .katex path{fill:#000;stroke:#000;stroke-width:1px;}#mermaid-rgi .rough-node .label,#mermaid-rgi .node .label,#mermaid-rgi .image-shape .label,#mermaid-rgi .icon-shape .label{text-align:center;}#mermaid-rgi .node.clickable{cursor:pointer;}#mermaid-rgi .root .anchor path{fill:#91918D!important;stroke-width:0;stroke:#91918D;}#mermaid-rgi .arrowheadPath{fill:#0b0b0b;}#mermaid-rgi .edgePath .path{stroke:#91918D;stroke-width:2.0px;}#mermaid-rgi .flowchart-link{stroke:#91918D;fill:none;}#mermaid-rgi .edgeLabel{background-color:#F5E6D8;text-align:center;}#mermaid-rgi .edgeLabel p{background-color:#F5E6D8;}#mermaid-rgi .edgeLabel rect{opacity:0.5;background-color:#F5E6D8;fill:#F5E6D8;}#mermaid-rgi .labelBkg{background-color:rgba(245, 230, 216, 0.5);}#mermaid-rgi .cluster rect{fill:#CC785C;stroke:hsl(15, 12.3364485981%, 48.0392156863%);stroke-width:1px;}#mermaid-rgi .cluster text{fill:#3387a3;}#mermaid-rgi .cluster span{color:#3387a3;}#mermaid-rgi div.mermaidTooltip{position:absolute;text-align:center;max-width:200px;padding:2px;font-family:inherit;font-size:12px;background:#CC785C;border:1px solid hsl(15, 12.3364485981%, 48.0392156863%);border-radius:2px;pointer-events:none;z-index:100;}#mermaid-rgi .flowchartTitleText{text-anchor:middle;font-size:18px;fill:#191919;}#mermaid-rgi rect.text{fill:none;stroke-width:0;}#mermaid-rgi .icon-shape,#mermaid-rgi .image-shape{background-color:#F5E6D8;text-align:center;}#mermaid-rgi .icon-shape p,#mermaid-rgi .image-shape p{background-color:#F5E6D8;padding:2px;}#mermaid-rgi .icon-shape rect,#mermaid-rgi .image-shape rect{opacity:0.5;background-color:#F5E6D8;fill:#F5E6D8;}#mermaid-rgi .label-icon{display:inline-block;height:1em;overflow:visible;vertical-align:-0.125em;}#mermaid-rgi .node .label-icon path{fill:currentColor;stroke:revert;stroke-width:revert;}#mermaid-rgi :root{--mermaid-font-family:inherit;}🔎 검색 파이프라인 - 유저 요청 시📥 수집 파이프라인 - 일 1회 배치Tistory RSS/APICrawlerVelog GraphQLChunker512 tokensEmbedderOpenAI APIChromaDBUser QueryFastAPI/searchQuery EmbedderSimilarity Searchtop-k=5RerankeroptionalPrompt BuilderClaude API답변 + 출처 링크

4. 주요 구현 사항
4.1 수집 & 임베딩 파이프라인
python# ingest/embedder.py (발췌 · 민감정보 제거)

from typing import Iterable
import tiktoken
from openai import OpenAI

CHUNK_TOKENS = 512
CHUNK_OVERLAP = 64

class BlogEmbedder:
    def __init__(self, client: OpenAI, model: str = "text-embedding-3-small"):
        self._client = client
        self._model = model
        self._enc = tiktoken.get_encoding("cl100k_base")

    def chunk(self, text: str) -> Iterable[str]:
        """오버랩 있는 슬라이딩 윈도우로 청크 생성 (문맥 연속성 확보)"""
        tokens = self._enc.encode(text)
        step = CHUNK_TOKENS - CHUNK_OVERLAP
        for i in range(0, len(tokens), step):
            yield self._enc.decode(tokens[i : i + CHUNK_TOKENS])

    def embed(self, chunks: list[str]) -> list[list[float]]:
        """배치 임베딩 (API 호출 횟수 절감)"""
        resp = self._client.embeddings.create(input=chunks, model=self._model)
        return [d.embedding for d in resp.data]
핵심 설계 포인트

오버랩 청킹: 문단 경계에서 맥락이 잘리는 문제 방지 (64 토큰 오버랩)
배치 임베딩: 포스트 1개당 API 1회 호출로 비용 최소화
증분 업데이트: updated_at 비교로 변경된 포스트만 재임베딩

4.2 검색 & 답변 생성
python# api/search.py (발췌)

from anthropic import Anthropic

SYSTEM_PROMPT = """당신은 르무엘의 기술 블로그 지식을 검색해서 답하는 어시스턴트입니다.
반드시 아래 규칙을 지키세요:
1. 제공된 [컨텍스트]에 없는 내용은 추측하지 말고 "해당 내용은 블로그에 없습니다"라고 답하세요.
2. 답변 마지막에 근거 포스트의 URL을 반드시 출처로 포함하세요.
3. 한국어로 답하되, 기술 용어는 원문 그대로 사용하세요."""

async def search_and_answer(query: str, top_k: int = 5) -> dict:
    # 1. 쿼리 임베딩
    q_vec = embedder.embed([query])[0]

    # 2. 벡터 검색
    hits = chroma.query(query_embeddings=[q_vec], n_results=top_k)

    # 3. 프롬프트 조립 (토큰 제한 고려)
    context = build_context(hits, max_tokens=3000)

    # 4. 답변 생성
    resp = anthropic.messages.create(
        model="claude-haiku-4-5",
        max_tokens=500,
        system=SYSTEM_PROMPT,
        messages=[{
            "role": "user",
            "content": f"[컨텍스트]\n{context}\n\n[질문]\n{query}",
        }],
    )

    return {
        "answer": resp.content[0].text,
        "sources": [h["metadata"]["url"] for h in hits["metadatas"][0]],
        "usage": resp.usage.model_dump(),
    }
4.3 토큰 제어 전략
구간제한전략입력 컨텍스트3,000 토큰유사도 상위 5개 청크만 선택, 초과 시 뒷부분 절단시스템 프롬프트150 토큰고정 (캐싱 대상)응답500 토큰max_tokens=500 강제세션당 총 합4,000 토큰초과 쿼리는 앞단에서 reject
4.4 프롬프트 체인
[사용자 질의]
    ↓
[Query Rewriter]   ← 짧은 질의 → 검색용으로 확장 ("카프카 리밸런싱" → "Kafka consumer rebalancing 이슈 원인 해결")
    ↓
[Vector Search]    ← ChromaDB top-5
    ↓
[Context Builder]  ← 토큰 한도 내 포스트 본문 조립
    ↓
[Answer Generator] ← Claude API
    ↓
[Citation Check]   ← 답변에 출처 URL이 포함됐는지 검증, 없으면 후처리로 추가
    ↓
[최종 응답]

5. 비용 통제
5.1 월별 예산 및 실제 집행
항목예산실제 (월 평균)비고OpenAI 임베딩$5$1.2증분 업데이트 덕분에 초기 1회 이후 급감Anthropic Claude API$20$7~12Haiku 모델 사용홈 서버 전기료 환산-~$324시간 가동합계~$25~$11~16예산 대비 약 40% 절감
5.2 절감 기법

증분 임베딩: 블로그 전체 재임베딩 ❌ → 변경된 포스트만 ✅
응답 캐싱: 동일 쿼리는 Redis(24시간)로 히트 → API 호출 약 30% 감소
모델 티어링: 단순 검색은 Haiku, 요약 생성은 Sonnet으로 분리 (현재는 Haiku만 사용 중)
Prompt Caching: Anthropic의 프롬프트 캐싱 기능으로 시스템 프롬프트 재사용 비용 90% 절감


6. 본인 기여도 명시
영역기여 비율설명시스템 아키텍처 설계100%전체 구조, 스택 선정, 배포 설계수집 파이프라인 구현90%청킹/임베딩 로직 본인 설계, 보일러플레이트는 AI 보조FastAPI 검색 API80%엔드포인트 설계 본인, 구현은 Claude Code 협업프롬프트 엔지니어링100%시스템 프롬프트, 체인 설계 본인비용/토큰 통제 로직100%배수 제한, 캐싱, 증분 업데이트 모두 본인 설계배포 (k3s + Cloudflare)100%홈 서버 k3s 클러스터, Zero Trust 보호 본인 구성

7. 성과
지표값측정 방법포스트 검색 시간 (체감)수 분 → 5초 이내동일 질문 10건 비교검색 만족도 (본인 평가)4.6 / 5.025건 질의 Top-5 내 원하는 포스트 포함 여부평균 응답 레이턴시1.2초p50 기준 (임베딩 + 검색 + 생성)월 운영 비용약 $11~16상세는 섹션 5환각 발생률< 3%시스템 프롬프트의 "없으면 없다고 답하라" 강제로 억제

8. 트러블슈팅 경험
🐛 문제 1: 한국어 문서에서 토큰 수가 영어보다 2배 이상
증상: cl100k_base 토크나이저가 한글을 1글자당 2~3토큰으로 계산해 예상보다 API 비용이 빠르게 증가.
해결:

청크 크기를 512 → 384로 축소해 API 배치 수 증가, 대신 오버랩 비율 상향
긴 코드 블록은 별도 청크로 분리 (자연어와 섞이면 검색 품질 저하)
결과: 동일 데이터셋 대비 임베딩 비용 약 22% 감소

🐛 문제 2: 검색 품질이 불안정 (같은 질의에 다른 결과)
원인: 청크 경계에서 맥락이 잘려, 핵심 키워드가 다른 청크로 분리됨.
해결:

오버랩(64 토큰) 도입
제목/태그/본문을 각각 임베딩하는 대신 제목+태그를 본문 앞에 prepend한 뒤 통합 임베딩
결과: Top-5 hit rate 68% → 91%

🐛 문제 3: Claude가 존재하지 않는 블로그 글 URL을 만들어냄 (환각)
증상: "이 내용은 https://iamipro.tistory.com/999 에 있습니다" 식으로 실제 존재하지 않는 URL 생성.
해결:

시스템 프롬프트에 "출처 URL은 반드시 [컨텍스트]에 명시된 것만 사용하라" 추가
응답 후처리에서 URL 정규식 추출 → 실제 메타데이터와 대조, 불일치 시 경고 표시
결과: 환각 URL 주 3~5건 → 0건


9. RAG 품질 측정 방법
자체 평가셋 30개 질문을 작성해 정기적으로 품질을 측정합니다.
평가 항목측정 방식현재 값Retrieval 정확도Top-5에 정답 포스트 포함 비율91%Faithfulness답변이 컨텍스트에 근거하는 비율96%Answer Relevance질문과 답변의 의미 유사도0.82 (cosine)출처 정확도인용 URL이 실제 관련 있는 비율100%

10. 향후 개선 계획

 Hybrid Search: BM25 + Vector 결합으로 고유명사 검색 보강
 Reranker: Cohere Rerank 또는 자체 모델로 Top-5 재정렬
 대화 맥락 유지: 연속 질문 지원 (세션 히스토리 주입)
 공개 API 버전: 외부 방문자도 질의 가능한 엔드포인트 (rate limit + 캡차 적용)


11. 링크

GitHub Repository: [공개 예정 / 제출 시 링크 업데이트]
데모: https://lemuel.co.kr [라이브 데모 예정]
관련 블로그 포스팅:

ChromaDB + FastAPI로 만든 개인 블로그 검색기
Anthropic Prompt Caching으로 비용 90% 절감하기
