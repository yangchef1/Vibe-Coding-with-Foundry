# 문제 해결

[이전: 로컬 MARGIN 확인](04-local-run.md) | [처음으로](../README.md)

## 빠른 진단 순서

```powershell
npm run typecheck
npm test
az account show --output table
Get-Content .env
```

`.env`의 전체 내용은 채팅이나 지원 요청에 붙여 넣지 않습니다. 변수 이름과 오류 메시지만 공유합니다.

## 설정 필요가 표시됨

다음을 확인합니다.

- `.env`가 저장소 루트에 있습니다.
- `FOUNDRY_PROJECT_ENDPOINT`와 `FOUNDRY_AGENT_NAME`이 비어 있지 않습니다.
- 환경 변수를 바꾼 뒤 `npm run dev:full`을 다시 시작했습니다.

```dotenv
VITE_WORKSHOP_MODE=live
FOUNDRY_PROJECT_ENDPOINT=https://<resource>.services.ai.azure.com/api/projects/<project>
FOUNDRY_AGENT_NAME=Bnk-Agent
FOUNDRY_TOKEN_SCOPE=https://ai.azure.com/.default
API_PORT=8787
```

프로젝트 엔드포인트 끝에 `/openai/v1` 또는 `/openai/v1/responses`를 직접 추가하지 않습니다.

## API 서버 연결 실패

- `npm run dev:full` 터미널에 Vite와 Express 서버가 모두 실행 중인지 확인합니다.
- Vite가 `http://localhost:3000`, API 서버가 `http://127.0.0.1:8787`을 사용하는지 확인합니다.
- [http://localhost:3000/api/health](http://localhost:3000/api/health)이 HTTP 200인지 확인합니다.
- 포트가 이미 사용 중이면 기존 프로세스를 종료합니다. Vite가 다른 포트로 자동 변경되게 두지 않습니다.

## 401 Unauthorized

Azure CLI 로그인을 갱신하고 올바른 구독을 선택합니다.

```powershell
az login
az account set --subscription "구독 이름 또는 ID"
az account show --output table
az account get-access-token --scope https://ai.azure.com/.default --query expiresOn --output tsv
```

마지막 명령은 토큰의 만료 정보만 표시합니다. 액세스 토큰 자체를 출력하거나 공유하지 않습니다.

## 403 Forbidden

Azure CLI로 로그인한 사용자에게 Foundry 프로젝트 범위의 `Foundry User` 역할이 있는지 확인합니다.

1. Microsoft Foundry에서 **Operate** > **Admin**을 엽니다.
2. 프로젝트를 선택하고 사용자 목록을 확인합니다.
3. 현재 로그인한 계정이 없다면 프로젝트 관리자에게 추가를 요청합니다.
4. 역할을 받은 뒤 `az login`을 다시 실행하고 앱을 재시작합니다.

역할 이름 변경이 아직 반영되지 않은 화면에는 이전 이름인 `Azure AI User`가 보일 수 있습니다.

## 404 또는 Agent를 찾을 수 없음

- `FOUNDRY_PROJECT_ENDPOINT`가 프로젝트 **Home**에서 복사한 전체 값인지 확인합니다.
- `FOUNDRY_AGENT_NAME=Bnk-Agent`의 대소문자가 Foundry Agent 이름과 같은지 확인합니다.
- `Bnk-Agent`에 저장된 Agent 버전이 있는지 확인합니다.
- classic 연결 문자열이 아니라 New Foundry 프로젝트 엔드포인트를 사용합니다.

## 429 또는 할당량 오류

- Foundry **Build** > **Models**에서 모델 배포 상태를 확인합니다.
- 해당 모델의 토큰 할당량을 확인합니다.
- 공유 모델이 포화 상태라면 강사가 준비한 대체 모델을 사용합니다.
- 모델을 바꿨다면 `Bnk-Agent`의 모델 연결을 다시 저장합니다.

## 잘못된 JSON 또는 INVALID_AGENT_CONTRACT

서버가 Foundry 응답을 JSON으로 해석하지 못하거나 Zod 계약 검증에 실패한 경우입니다. UI에는 결과가 표시되지 않아야 합니다.

- [Bnk-Agent 지침](../prompts/agent-instructions.md)을 다시 붙여 넣고 새 버전을 저장합니다.
- File Search가 연결되어 있고 실제 지식 문서 인덱싱이 완료됐는지 확인합니다.
- Agent가 Markdown이나 JSON fence 없이 JSON 객체 하나만 반환하는지 플레이그라운드에서 확인합니다.
- `sources`, `branchOpinion`, `applicationFields`, `reviewPoints`의 최소 개수와 열거값을 확인합니다.

## 응답에 근거 없는 정책이나 수치가 보임

- 앱 요청 본문의 `agent.name`이 `Bnk-Agent`인지 확인합니다.
- 모델을 직접 호출하는 요청이 아니라 `agent: { type: "agent_reference", name: ... }`를 사용하는지 확인합니다.
- Agent Instructions에 연결 지식 외 정보 생성 금지가 포함됐는지 확인합니다.
- 근거가 없는 결과를 사용하지 말고 담당자에게 보고합니다.

## AI 분석 실행이 비활성화됨

- `/api/health`의 `configured`가 `true`인지 확인합니다.
- 고객정보 입력 후 **입력 완료**를 선택했는지 확인합니다.
- 필드 가까이에 표시된 한국어 오류를 수정합니다.
- 분석 중에는 중복 실행을 막기 위해 버튼이 비활성화되는 것이 정상입니다.

## 테스트 또는 빌드가 실패함

```powershell
npm install
npm run lint
npm run typecheck
npm test
npm run build
npm run test:e2e
```

실패한 명령과 오류 메시지를 같은 Copilot 세션에 전달하고 수정하도록 요청합니다. Foundry 자격 증명은 단위 테스트에 필요하지 않아야 합니다.

## 지원 요청에 포함할 정보

- 실패한 단계와 명령 이름
- HTTP 상태 코드, 안전한 오류 `code`와 한국어 `message`
- lint, typecheck, test, build와 E2E 통과 여부
- `/api/health`의 `mode`와 `configured`
- `az account show`에 올바른 구독이 표시되는지 여부

액세스 토큰, Authorization 헤더, API 키, 암호, 전체 고객 입력과 `.env` 내용은 공유하지 않습니다.