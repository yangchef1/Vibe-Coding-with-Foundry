# Repository instructions

- 참가자에게 보이는 문서와 UI는 한국어로 작성합니다.
- 가이드에 구간, 분량, 소요 시간과 같은 시간 정보를 추가하지 않습니다.
- 이 저장소는 독립형 GitHub Copilot app 실습용입니다. IDE 확장 전용 절차를 추가하지 않습니다.
- MARGIN은 React 19, TypeScript, Vite, Express, Zod, Lucide React, Vitest와 `@azure/identity`를 사용합니다.
- Node.js 서버에서만 Azure 자격 증명을 사용하고 브라우저 코드에 토큰이나 비밀값을 노출하지 않습니다.
- Foundry 호출은 프로젝트 엔드포인트의 `/openai/v1/responses`와 `agent: { type: "agent_reference", name }`를 사용합니다.
- 로컬 인증은 Azure CLI 로그인과 `DefaultAzureCredential`을 사용합니다. API 키를 소스에 저장하지 않습니다.
- `FOUNDRY_PROJECT_ENDPOINT` 또는 `FOUNDRY_AGENT_NAME`이 없으면 설정 방법을 안내하고, 완료 확인에서는 실제 `Bnk-Agent` live 응답을 검증합니다.
- production 코드에 mock Agent, 고정 응답, 실패 fallback 결과와 분석 지연 시뮬레이션을 넣지 않습니다.
- 고객 입력과 Agent 응답을 브라우저 저장소나 로그에 남기지 않습니다.
- 생성하는 UI 색상은 `public/index.html`에 `--cp-*` 변수로 정의하고 그 변수만 사용합니다.
- 앱을 생성한 뒤 lint, typecheck, test, build와 E2E를 모두 실행하고 실패하면 원인을 수정합니다.
- 관련 없는 리팩터링이나 의존성 추가는 피합니다.