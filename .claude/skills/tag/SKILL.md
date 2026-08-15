---
name: tag
description: 03-semantic·04-procedural 노트의 파일 위치와 본문은 건드리지 않고 tags와 relationships(관계망) 메타데이터만 보강한다. "/tag" 요청 시 사용.
---

# /tag — 스마트 라벨링 및 관계망 형성

파일을 옮기거나 본문 내용을 고치지 않는다. **frontmatter만** 수정한다.

## 대상

`03-semantic/`, `04-procedural/` 아래 노트. (`02-episodic/`까지 포함할지는 사용자에게 확인 후 진행)
`01-working-memory/`, `00-MOC/`, `_templates/`는 대상에서 제외한다.

## 절차

1. **맥락 파악**: 노트 본문을 읽고 실제 주제/성격을 판단한다 (기존 `tags`에만 의존하지 않는다).
2. **태그 보강**: 누락되었거나 더 정확한 분류 태그를 `tags`에 추가한다. 기존 태그는 지우지 않는다 — 명백히 틀린 경우만 근거를 들어 사용자에게 제거를 제안한다.
3. **관계망 형성**: 단순 `related` 링크를 넘어, 타입이 있는 관계를 `relationships` 필드에 명시한다.

   ```yaml
   relationships:
     - type: depends_on
       target: "[[Redis]]"
     - type: owner
       target: "[[박서준]]"
     - type: implements
       target: "[[전략 패턴]]"
   ```

   본문을 읽으며 "A가 B에 의존한다", "A 담당자는 B다", "A는 B를 구현/대체/모순한다" 같은 관계를 찾아 이 형식으로 남긴다. 관계 타입은 자유롭게 쓰되 기존에 쓰인 타입(`depends_on`, `owner`, `implements`, `supersedes`, `contradicts` 등)을 우선 재사용해 일관성을 유지한다.
4. **양방향은 강제하지 않는다**: A→B 관계를 적었다고 B에도 자동으로 역관계를 추가하지 않는다. 필요해 보이면 제안만 한다.

## 완료 후 보고

- 태그를 추가한 노트와 추가된 태그
- 새로 형성한 relationships 목록과 근거(본문의 어떤 문장에서 추론했는지 한 줄)
- 관계 타입이 애매하거나 본문 근거가 약했던 항목은 사용자에게 확인 요청
