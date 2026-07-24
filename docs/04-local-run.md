# 로컬에서 MARGIN live 분석 확인

[이전: Copilot 앱 개발](03-copilot-app.md) | [처음으로](../README.md) | [문제 해결](05-troubleshooting.md)

생성한 MARGIN을 로컬에서 실행하고 Microsoft Entra 인증으로 `Bnk-Agent`의 실제 정책자금 분석 결과를 확인합니다.

## 연결 정보 확인

Copilot이 실행해 둔 개발 서버가 있다면 해당 터미널에서 `Ctrl+C`를 눌러 잠시 종료합니다. 앞 단계에서 만든 `.env`가 저장소 루트에 있고 다음 값이 설정되어 있는지 확인합니다.

```dotenv
VITE_WORKSHOP_MODE=live
FOUNDRY_PROJECT_ENDPOINT=https://<resource>.services.ai.azure.com/api/projects/<project>
FOUNDRY_AGENT_NAME=Bnk-Agent
FOUNDRY_TOKEN_SCOPE=https://ai.azure.com/.default
API_PORT=8787
```

액세스 토큰이나 API 키는 `.env`에 넣지 않습니다.

## 앱 실행

```powershell
npm install
npm run lint
npm run typecheck
npm test
npm run build
npm run test:e2e
npm run dev:full
```

`dev:full` 터미널은 실행한 상태로 둡니다. 브라우저에서 [http://localhost:3000](http://localhost:3000)을 열고 화면에 **연결 준비 완료**가 표시되는지 확인합니다.

## 교육용 고객정보 자동 채우기

고객정보 JSON을 직접 입력하지 않고 MARGIN의 **교육용 예시 채우기** 버튼을 사용합니다.

1. 첫 화면의 고객정보, 추천과 문서 영역이 비어 있는지 확인합니다.
2. 고객정보 폼에서 **교육용 예시 채우기**를 선택합니다.
3. 다음 합성 고객정보가 모든 필드에 채워지는지 확인합니다.

  - 상호명: `한결베이커리`
  - 대표자명: `김교육`
  - 교육용 식별번호: `EDU-2026-0001`
  - 지역과 구군: `부산`, `해운대구`
  - 업종: `C10712`, `빵류 제조업`
  - 업력: `38개월`
  - 연 매출: `840,000,000원`
  - 직원 수: `6명`
  - 신청 금액: `150,000,000원`
  - 자금 용도: `저탄소 생산설비 교체와 온라인 판매 채널 확대`

4. 예시를 채운 것만으로 **입력 완료**나 AI 분석이 자동 실행되지 않는지 확인합니다.
5. 내용을 확인한 뒤 **입력 완료**를 선택합니다.
6. 입력 요약이 표시되면 **AI 분석 실행**을 선택합니다.

> [!NOTE]
> **교육용 예시 채우기**는 합성 고객정보를 폼에 넣는 입력 편의 기능입니다. Agent 응답이나 API 결과를 대신하는 mock 기능이 아닙니다.

## 실제 Agent 분석 확인

서버 검증을 통과한 `mode: "live"` 결과에 다음 내용이 표시되는지 확인합니다.

- 정책자금 추천, 업종 분석과 실제 File Search 출처
- 영업점 의견서 문단 2개 이상
- 차입신청서 항목 3개 이상과 각 근거
- 담당자 검토 체크리스트와 진행률
- 추천과 문서가 담당자 검토 전 초안이라는 안내

초안 복사와 **다시 분석**이 동작하고, 고객정보를 수정하면 이전 요약과 Agent 결과가 제거되는지 확인합니다. 마지막으로 **전체 초기화**를 선택해 모든 입력과 결과가 빈 상태로 돌아가는지 확인합니다. 실제 고객정보는 입력하지 않습니다.

## 완료 확인

- [ ] `.env`에 Foundry 엔드포인트와 `Bnk-Agent` 이름을 설정했습니다.
- [ ] `az account show`에 올바른 구독과 사용자가 표시됩니다.
- [ ] lint, typecheck, 단위 테스트, build와 E2E가 통과합니다.
- [ ] `/api/health`가 HTTP 200과 `configured: true`를 반환합니다.
- [ ] **교육용 예시 채우기**로 합성 고객정보를 채운 뒤 명시적으로 분석을 실행했습니다.
- [ ] 실제 File Search 출처가 포함된 live 결과와 담당자 검토 항목을 확인했습니다.
- [ ] 실제 개인정보가 입력, 저장 또는 로그 출력되지 않았습니다.

확인이 끝나면 `dev:full`을 실행한 터미널에서 `Ctrl+C`를 눌러 서버를 종료할 수 있습니다.