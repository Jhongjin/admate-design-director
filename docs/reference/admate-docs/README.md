# AdMate Docs / Strategy HQ

이 폴더는 AdMate 프로젝트의 중앙 장기기억이자 총괄 문서 보관소다.

AdMate는 나스미디어 데이터분석팀이 구축하는 AI Agent 기반 광고 운영 자동화 플랫폼이다.

제품군:

- AdMate Compass: 광고 정책/가이드 RAG
- AdMate Sentinel: 캠페인 사전 검수 + 실시간 모니터링
- AdMate Lens: 광고 캡처/증빙 자동화
- AdMate Foresight: 미디어 플래닝/성과 예측
- AdMate Agent Core: Openclaw + Hermes 기반 실행/학습/자동화 엔진

## 폴더 구조

admate-docs/
- AGENTS.md
- README.md
- INDEX.md
- strategy/
- design/
- handoff/
- prompts/
- repo-guides/
- working-notes/

## 사용 원칙

새로운 LLM, Codex, Paperclip, 개발 Agent는 먼저 아래 순서로 읽는다.

1. AGENTS.md
2. handoff/AdMate_Project_Director_Handoff_AGENTS_v1.md
3. strategy/01_AdMate_Unified_Agent_Architecture_v1_1.md
4. strategy/05_AdMate_Product_Map_v1.md
5. strategy/13_AdMate_Homepage_IA_Brand_Copy_v1.md
6. strategy/14_AdMate_Repo_Codex_Integration_Guide_v1.md
7. design/openclaw-theme-reference.md

## 주의

- 이 폴더는 코드 수정 작업용이 아니라 전략/문서/프롬프트 관리용이다.
- 각 제품 repo 작업 전 이 폴더의 문서를 source of truth로 본다.
- secret, API key, token, credential은 절대 저장하지 않는다.
