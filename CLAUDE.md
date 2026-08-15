# CLAUDE.md — inho_llmwiki

이 저장소는 Obsidian 기반 개인 지식 창고(LLM Wiki)다. [Andrej Karpathy의 LLM Wiki 아이디어](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)와 [agentmemory](https://github.com/rohitg00/agentmemory)에서 얻은 확장 패턴("LLM Wiki v2")을 1인용 Obsidian 볼트 규모에 맞게 축소 적용한다.

핵심 원칙: **다시 유도하지 말고, 축적하라.** 같은 걸 매번 새로 설명하지 않도록, 유용한 결론은 반드시 위키에 파일링한다.

## 운영자 프로필

- 소프트웨어 엔지니어. 주 업무: 코드 작성/리뷰, CS 학습.
- 이 위키에서 원하는 산출물: 학습 노트/정리, 재사용 가능한 코드·기술 자산.
- 따라서 이 위키는 업무 보고서나 블로그 마케팅 톤이 아니라, **재사용을 전제로 한 학습 노트/기술 레퍼런스 톤**을 기본으로 한다.

## 메모리 계층 (Memory Lifecycle)

지식은 4단계를 거쳐 숙성된다. 폴더 = 숙성 단계.

| 계층 | 폴더 | 성격 | 승격 기준 |
|---|---|---|---|
| Working memory | `01-working-memory/` | 원본 캡처, 미처리 | 정리되면 episodic 또는 semantic으로 |
| Episodic memory | `02-episodic/` | 날짜별 세션/학습 로그 | 같은 내용이 2회 이상 반복되면 semantic 후보 |
| Semantic memory | `03-semantic/` | 검증된 지식 (위키 본문) | — |
| Procedural memory | `04-procedural/` | 반복돼서 굳어진 워크플로 | semantic/episodic에서 같은 절차가 3회+ 반복되면 승격 |

각 폴더 안 `_index.md`에 해당 계층의 세부 규칙이 있다.

## 폴더 구조

```
00-MOC/              탐색 허브 (Map of Content)
01-working-memory/   원본 캡처, 미처리
02-episodic/         날짜별 세션/학습 로그
03-semantic/         검증된 지식 — 위키 본문
  CS/                CS 개념 노트
  코드-자산/          재사용 가능한 코드/기술 자산
04-procedural/       워크플로/체크리스트/패턴
_templates/          새 노트 작성 시 복사해서 쓰는 템플릿
index.md             사람이 훑어보는 카탈로그
CLAUDE.md            이 문서
```

## Frontmatter 스키마

모든 semantic/procedural 노트는 아래 필드를 채운다 (episodic/working-memory는 생략 가능).

| 필드 | 값 | 설명 |
|---|---|---|
| `title` | string | |
| `type` | concept / code-asset / workflow / daily / moc / meta | 노트 종류 |
| `status` | draft / active / stale | draft: 아직 미검증. active: 현재 유효. stale: 오래돼서 재검토 필요 |
| `confidence` | 0.0–1.0 | 처음 작성 시 0.5. 다른 세션에서 재확인되면 +0.1~0.2, 모순 발견 시 -0.2 (이유를 본문에 기록) |
| `tags` | list | |
| `created` / `updated` | YYYY-MM-DD | `updated`는 내용을 바꿀 때마다 갱신 |
| `sources` | list | 참고한 자료 (URL, 책, 노트 등) |
| `related` | list of `[[wikilink]]` | 관련 노트. 양방향 연결을 지향 |

## 워크플로우

### Ingest — 새 자료/대화 결론을 반영할 때
1. 사소한 캡처는 바로 `03-semantic/`에, 아직 검증 안 된 원본은 `01-working-memory/`에 둔다.
2. 핵심만 추출해 적절한 tier에 노트를 만들거나 **기존 노트를 업데이트**한다 (겹치는 내용이면 새로 만들지 말 것 — `updated` 갱신).
3. 새 semantic/procedural 노트는 관련 MOC에 반드시 링크를 추가한다. MOC에 안 걸린 노트는 orphan이다.
4. `related`에 관련 노트를 링크해 양방향 연결을 만든다.

### Query — 질문에 답할 때 탐색 순서
1. `00-MOC/`에서 해당 주제의 MOC를 먼저 확인한다.
2. MOC에서 링크된 semantic 노트를 확인한다.
3. 부족하면 `index.md` 또는 폴더 탐색/grep으로 보충한다.
4. 위키에 없는 새 정보를 찾았다면, 답변 후 위키에 정리해둘지 제안한다 (Crystallization).

### Lint — 사용자가 "정리해줘"라고 요청하거나, 노트가 꽤 쌓였을 때 먼저 제안
- MOC/related 어디에도 안 걸린 semantic 노트(orphan) 찾아서 연결하거나 flag
- `status: active`인데 `updated`가 6개월 이상 지난 노트 → `stale`로 표시 제안 (삭제 아님)
- 깨진 wikilink 확인
- `01-working-memory/`에 2주 이상 방치된 캡처 → 정리하거나 semantic 승격 제안

### Crystallization — 세션/탐구 결과를 위키로 편입
대화에서 의미 있는 결론(디버깅 해결, 개념 정리, 코드 패턴)이 나오면 세션 끝에 아래 형태로 요약해 파일링을 제안한다:
- 질문/문제 → 발견/결론 → 관련 파일·노트 → 위키에 남길 semantic 후보

## MOC 사용 규칙

- MOC는 탐색 허브 역할만 한다. 본문 내용은 쓰지 않고 링크만 정리한다.
- 새 semantic/procedural 노트를 만들면 관련 MOC에 한 줄 링크를 반드시 추가한다.
- MOC가 늘어나면 `MOC-Home`에서 트리 구조로 계속 연결한다.

## 품질 기준

- frontmatter 필수 필드를 채운다.
- 최소 1개의 `related` 링크 또는 소속 MOC 연결이 있어야 한다.
- 코드 자산은 실행 가능한 형태로, 사용 맥락을 한 줄 이상 명시한다.
- **노트 하나 = 개념/자산 하나** (atomic). 여러 개념을 한 노트에 뭉치지 않는다.

## 지금은 하지 않는 것 (범위 밖)

LLM Wiki v2가 제안하는 것 중, 1인 사용자·Obsidian 마크다운 규모에는 아직 과한 것들:
- 임베딩/벡터 검색, BM25 하이브리드 서치 인프라
- 엔티티 추출 + 타입드 지식 그래프 (지금은 wikilink로 충분)
- 자동 훅/이벤트 기반 자동화 (ingest/lint는 지금은 요청 기반 수동)
- 멀티에이전트 mesh sync, 팀 협업 스코핑
- 정형화된 감사 로그/거버넌스 시스템

노트가 대략 150~200개를 넘어가거나 탐색이 느려지기 시작하면 이 섹션을 다시 열어 확장을 검토한다.

## 참고
- 원안: [Andrej Karpathy's LLM Wiki idea](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
- 확장판: [agentmemory](https://github.com/rohitg00/agentmemory)
