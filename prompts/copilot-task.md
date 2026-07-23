# MARGIN 바이브 코딩 프롬프트

아래 코드 블록 전체를 GitHub Copilot app의 **Autopilot** 세션에 붙여 넣습니다.

```text
현재 저장소에 실행 가능한 한국어 업무용 웹앱 "MARGIN"을 구현하라. 사용자가 직접 입력한 교육용 고객정보를 Microsoft Foundry Prompt Agent가 분석해 정책자금 추천, 근거와 문서 초안을 생성해야 한다.

화면 모양과 세부 컴포넌트 구조는 요구사항을 만족하는 범위에서 자유롭게 결정하라. 설명이나 의사 코드로 끝내지 말고 코드, 테스트와 실행 설정까지 완성하라.

# 1. 실행 규칙

1. package.json, TypeScript 설정, 기존 소스와 테스트를 먼저 확인한다. 기존 React + Vite 구조와 사용자 변경을 유지하고 필요한 부분만 수정한다.
2. React 19, TypeScript, Vite, Express, Zod, DefaultAzureCredential, Lucide React와 Vitest를 사용한다. Playwright가 이미 있으면 유지한다.
3. 공유 타입과 입력 스키마, browser service, server와 UI 책임을 분리한다.
4. 6개 이내의 짧은 계획을 제시한 뒤 승인 대기 없이 구현한다. TODO, 생략된 코드와 동작하지 않는 버튼을 남기지 않는다.
5. 기존 infra 폴더는 삭제하거나 변경하지 않는다.

# 2. 핵심 사용자 흐름

첫 화면은 랜딩 페이지가 아니라 비어 있는 업무 입력 화면이다.

1. 빈 고객정보 폼과 별도의 "교육용 예시 채우기" 버튼을 표시한다.
2. 사용자가 직접 고객정보와 상담 내용을 입력하거나, 예시 버튼으로 합성 고객정보를 채운다.
3. "입력 완료"에서 Zod로 검증하고 입력 요약을 표시한다.
4. 사용자가 "AI 분석 실행"을 선택했을 때만 /api/analyze를 호출한다.
5. 검증된 live 결과에 한해 추천, 업종 분석, 근거, 영업점 의견서와 차입신청서 초안을 표시한다.

모든 추천과 문서는 담당자 검토 전 초안이다. 자동 승인, 접수, 대출 실행과 지급 기능은 만들지 않는다.

# 3. 런타임 안전 경계

- 앱은 고객정보, 추천과 문서가 모두 없는 상태로 시작하며 예시 데이터를 자동으로 채우지 않는다.
- production 코드에는 고정 또는 랜덤 Agent 응답, mock Agent, mock API, setTimeout 분석과 실패 fallback 결과를 넣지 않는다. Agent 응답 fixture와 네트워크 stub은 테스트 파일 안에서만 허용한다.
- 명시적인 "교육용 예시 채우기" 버튼에 사용하는 단일 합성 고객 fixture만 production 코드에 둘 수 있다. 이 fixture는 폼 입력을 채우는 용도로만 사용하고 추천, 문서, Agent 응답이나 API 응답을 포함하지 않는다.
- 실제 개인정보, 계좌정보, 비밀번호, API 키, 토큰과 운영 상품 조건을 넣지 않는다. 입력과 응답을 브라우저 저장소 또는 로그에 남기지 않는다.
- 브라우저는 같은 origin의 /api/health와 /api/analyze만 호출한다. Foundry 설정, 토큰과 Entra 인증은 Node 서버에서만 다룬다.

# 4. 기술 스택과 구조

기존 저장소 구조를 우선하되 다음 책임을 분리한다.

- domain: 공유 타입과 고객 입력 Zod 스키마
- browser service: /api/health와 /api/analyze 호출
- server: API 계약, Entra 인증, Foundry 호출, 응답 구조 검증과 Express route
- UI: 화면, 상태와 사용자 상호작용

컴포넌트 분할, 상태 관리 방식과 세부 파일 구성은 자유롭게 결정한다.

# 5. 사용자 입력 모델

- BusinessCustomer: id, businessName(상호명), ownerName(대표자명), businessNumber(교육용 식별번호), region(부산, 울산, 경남, 기타), district, industryCode, industryName, operatingMonths, annualRevenue, employeeCount, requestedAmount, fundingPurpose.

고객 ID는 입력 완료 시 crypto.randomUUID()로 만든다. 숫자 폼은 빈 문자열을 허용하되 검증 성공 후 number로 변환하며 빈 문자열을 0으로 바꾸지 않는다. 오류는 해당 필드 가까이에 한국어로 표시하고 첫 오류 필드로 포커스를 이동한다.

"교육용 예시 채우기" 버튼은 type="button"인 보조 버튼으로 만들고 다음 단일 합성 고객 초안을 폼 전체에 채운다.

- businessName: 한결베이커리
- ownerName: 김교육
- businessNumber: EDU-2026-0001
- region: 부산
- district: 해운대구
- industryCode: C10712
- industryName: 빵류 제조업
- operatingMonths: 38
- annualRevenue: 840000000
- employeeCount: 6
- requestedAmount: 150000000
- fundingPurpose: 저탄소 생산설비 교체와 온라인 판매 채널 확대

예시 버튼은 입력 완료 처리, 고객 ID 생성, 유효성 검증이나 API 호출을 자동 실행하지 않는다. 사용자가 내용을 확인한 뒤 "입력 완료"와 "AI 분석 실행"을 각각 명시적으로 선택해야 한다.

입력은 React 메모리에만 두며 새로고침하면 초기화한다. 입력값이 바뀌면 이전 입력 요약과 Agent 결과를 즉시 폐기한다. 전체 초기화는 예시 데이터도 포함해 모든 폼을 빈 상태로 되돌린다. "교육용 정보만 입력하고 실제 고객정보는 입력하지 마세요."를 항상 표시한다.

# 6. 화면, 상태와 연결

은행 직원이 반복 사용할 수 있는 조용한 운영 화면으로 만든다. 레이아웃과 색상은 자유롭게 결정하되 다음은 제공한다.

- 고객정보, AI 분석, 서류 검토의 3단계 진행 상태와 현재 가능한 다음 실행 버튼
- 고객정보 폼 가까이에 직접 입력과 구분되는 "교육용 예시 채우기" 보조 버튼
- 고객 입력 요약
- 추천, 업종 분석과 출처, 영업점 의견서와 차입신청서 탭
- 담당자 검토 체크리스트와 진행률
- 실제 동작하는 초안 복사, 다시 분석과 전체 초기화
- loading, success와 한국어 error 상태

한국어 은행 용어를 간결하게 사용하고 색상 외에도 아이콘과 텍스트로 상태를 구분한다. 의미 있는 fieldset, label, button과 접근 가능한 tab, focus-visible, aria-live, role=alert, reduced motion을 지원한다. 데스크톱과 390px 모바일에서 입력부터 AI 실행까지 사용할 수 있어야 한다.

앱 로드 시 /api/health를 호출해 "확인 중", "연결 준비 완료", "설정 필요", "API 서버 연결 실패"를 구분한다. configured=false이면 AI 실행을 비활성화하고 설정 안내만 표시한다.

분석 중에는 버튼을 비활성화하고 진행 상태를 보여준다. Foundry가 실패하면 오류, "연결 상태 다시 확인"과 "다시 시도"만 제공하며 mock 결과를 표시하지 않는다. 서버 검증에 성공한 live 응답만 화면에 표시한다.

# 7. API와 Foundry 계약

.env.example에는 실제 값 없이 다음 키를 두고 .env는 gitignore에 포함한다.

VITE_WORKSHOP_MODE=live
FOUNDRY_PROJECT_ENDPOINT=
FOUNDRY_AGENT_NAME=
FOUNDRY_TOKEN_SCOPE=https://ai.azure.com/.default
API_PORT=8787

GET /api/health는 인증 없이 HTTP 200과 { "status": "ok", "mode": "live", "configured": boolean }을 반환한다. configured는 FOUNDRY_PROJECT_ENDPOINT와 FOUNDRY_AGENT_NAME이 모두 있을 때만 true다.

POST /api/analyze는 전체 고객 입력을 Zod로 검증하고 검증된 고객정보만 Agent에 전달한다.

Foundry 호출은 다음 규칙을 따른다.

- endpoint 끝의 /openai/v1 또는 /openai/v1/responses를 제거한 뒤 {endpoint}/openai/v1/responses로 POST한다.
- DefaultAzureCredential로 FOUNDRY_TOKEN_SCOPE의 토큰을 얻고 Authorization: Bearer와 Content-Type: application/json을 사용한다. API 키 방식은 만들지 않는다.
- body에 agent: { type: "agent_reference", name: FOUNDRY_AGENT_NAME }와 input의 input_text를 넣는다. input_text에는 "연결된 지식 문서만 근거로 사용하고 JSON 계약만 반환하라"는 지시와 검증된 고객정보 JSON을 포함한다.
- timeout은 60초다. output_text를 우선 읽고 없으면 output[].content[].text를 합친다. 선택적인 Markdown JSON fence를 제거한 뒤 JSON.parse와 Zod로 검증한다.

토큰, Authorization header와 전체 고객 입력을 로그로 출력하지 않는다. 401, 403, 404, 429, timeout, 잘못된 JSON, 잘못된 계약을 구분 가능한 code와 안전한 한국어 message로 반환한다.

# 8. Agent 응답 계약과 서버 검증

Agent 응답은 다음 Zod 계약을 만족해야 한다.

- recommendation: name, provider, amount, reason
- industry: signal(회복, 보합, 주의), headline, detail
- sources: 1개 이상의 title, section, type(정책, 규정, 업황)
- branchOpinion: 비어 있지 않은 문단 2개 이상
- applicationFields: label, value, basis 항목 3개 이상
- reviewPoints: 비어 있지 않은 문자열 1개 이상

서버는 Foundry 응답을 그대로 전달하지 않고 Zod로 구조와 열거값을 검증한다. 계약을 위반하면 502와 INVALID_AGENT_CONTRACT를 반환하고 UI에 결과를 표시하지 않는다. 성공 응답에만 mode: "live"를 붙인다.

Agent 지침 문서에는 JSON 계약, File Search 사용, 근거 없는 정책명과 수치 생성 금지, 담당자 검토 필수를 기록한다. 추천과 수치는 연결된 지식 문서에서 확인한 내용만 사용하고, 근거를 찾지 못하면 추천할 수 없다고 명시한다. 고정 응답이나 가짜 knowledge 문서는 만들지 말고 실제 knowledge는 사용자가 Foundry에서 연결해야 한다고 README에 안내한다.

# 9. 테스트 요구사항

테스트 값은 각 테스트 안에서만 만들고 최소한 다음을 검증한다.

- 고객 입력 스키마의 성공, 필수값 누락과 숫자 경계값
- 초기 폼은 비어 있고 "교육용 예시 채우기"를 누르면 정의된 합성 고객정보가 모든 필드에 채워짐
- 예시 버튼만 눌렀을 때 입력 완료나 /api/analyze 호출이 자동으로 실행되지 않음
- 고객정보 변경 시 이전 Agent 결과가 제거됨
- 빈 초기 화면, 입력 오류, configured=false와 정상 live 결과 UI
- 서버가 잘못된 JSON과 응답 계약 위반을 거부함
- 390px에서 핵심 흐름에 접근 가능함

Foundry 자격증명 없이 단위 테스트가 통과하도록 credential과 네트워크는 테스트 경계에서만 stub한다.

# 10. 개발 명령과 완료 기준

package.json에 dev, dev:api, dev:full, start, lint, typecheck, test, build와 test:e2e 스크립트가 없으면 추가한다. Vite 개발 서버는 host를 localhost, port를 3000, strictPort를 true로 설정하고 /api proxy는 http://127.0.0.1:8787을 향하게 한다. Playwright의 baseURL과 webServer URL도 http://localhost:3000을 사용한다.

완료 전에 다음을 모두 실행하고 오류를 수정한다.

npm run lint
npm run typecheck
npm test
npm run build
npm run test:e2e

모든 검증이 통과하면 별도의 지속 터미널에서 npm run dev:full을 실행하고 종료하지 않는다. 다음 두 주소를 직접 요청해 모두 HTTP 200인지 확인한다.

http://localhost:3000
http://localhost:3000/api/health

웹앱이 http://localhost:3000에서 실제로 열리고 React 화면이 렌더링되는지 확인한 뒤에만 완료로 보고한다. Vite가 다른 포트로 자동 변경되게 두지 않는다.

README에는 설치, 빈 .env 설정, az login, npm run dev:full, http://localhost:3000 접속과 /api/health 확인, 교육용 예시 채우기와 교육용 입력 원칙만 간결하게 기록한다.

# 11. Azure App Service 공통 배포 계약

이 부분은 결과물마다 동일해야 한다. Docker와 Container Registry는 사용하지 않는다.

- package.json의 engines.node는 ">=24 <25"이고 express, zod, @azure/identity 등 runtime 패키지는 dependencies에 둔다.
- npm run build는 브라우저를 app/dist에, app/server TypeScript를 dist-server에 빌드한다. 루트 tsconfig.server.build.json은 app/server를 rootDir, dist-server를 outDir로 사용한다.
- package.json의 start는 "node dist-server/index.js"다. Express는 app/dist를 정적으로 제공해 UI와 /api를 같은 origin에서 실행한다.
- 서버는 process.env.PORT와 process.env.HOST를 우선 사용하며 로컬 기본값은 8787과 127.0.0.1이다. production은 .env 없이 시작되어야 한다.
- DefaultAzureCredential이 Azure의 AZURE_CLIENT_ID 환경변수와 사용자 할당 관리형 ID를 사용하게 하고 클라이언트 ID를 코드에 넣지 않는다.
- package.json, package-lock.json과 TypeScript 및 Vite 설정은 ZIP 원격 빌드를 위해 저장소 루트에 둔다.
- npm ci, npm run build, npm start 후 /api/health가 HTTP 200을 반환해야 한다.

마지막 응답에는 변경 파일, 구현한 사용자 흐름, Foundry 연결 방법, 실행한 검증, 실행 중인 http://localhost:3000 주소와 남은 외부 준비사항만 간결하게 보고하라.
```