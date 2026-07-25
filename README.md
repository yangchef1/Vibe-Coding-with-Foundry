# 정책자금 상담 도우미 워크숍

Microsoft Foundry와 GitHub Copilot app을 이용해 정책자금 추천과 영업점 의견서 작성을 지원하는 웹 애플리케이션을 만듭니다.

이 워크숍은 **New Microsoft Foundry**, **GitHub Copilot app 데스크톱 앱**, **Azure AI Projects 2.x API**를 사용합니다. Foundry Agents classic 절차는 사용하지 않습니다.

## 실습 결과

- **정책자금 추천 Agent**: File Search로 업체 조건에 맞는 정책자금 후보를 찾습니다.
- **영업점 의견서 Agent**: 추천 결과와 영업점 메모를 바탕으로 의견서 초안을 만듭니다.
- **상담 웹 애플리케이션**: 두 Agent를 하나의 업무 흐름으로 연결합니다.

실습 자료의 정책자금 정보는 교육용 가상 데이터입니다. 실제 금융 상담, 자격 판정, 여신 심사에는 사용할 수 없습니다.

## 실습 순서

### [1. 사전 준비](./1.%20사전%20준비/README.md)

실습에 필요한 로컬 도구, GitHub Copilot app, Azure 계정 정보, 이름 규칙, 정책자금 데이터 파일을 확인합니다.

- Node.js, npm, Git 설치 확인
- GitHub Copilot app 로그인 및 Agent 세션 확인
- Azure 구독과 Foundry 및 File Search 권한 확인
- 실습 리소스 이름 규칙 확인

---

### [2. Azure 구독 및 CLI 설정](./2.%20Azure%20구독%20및%20CLI%20설정/README.md)

Azure CLI로 교육용 구독에 로그인한 뒤, New Microsoft Foundry 프로젝트를 만들고 모델을 배포합니다.

- Azure CLI 설치 및 로그인
- Enter 키로 교육용 Azure 구독 선택 완료
- New Microsoft Foundry 프로젝트 생성
- `gpt-5.4` 모델 배포
- 프로젝트 Home에서 엔드포인트 위치 확인

---

### [3. Microsoft Foundry Agent 구성](./3.%20Microsoft%20Foundry%20Agent%20구성/README.md)

같은 정책자금 자료를 사용하는 두 개의 전문 Prompt Agent를 구성합니다.

- 정책자금 추천 Agent 생성
- File Search 데이터 업로드 및 연결
- 영업점 의견서 Agent 생성
- 각 Agent의 업무 시나리오 테스트

---

### [4. GitHub Copilot app으로 애플리케이션 만들기](./4.%20GitHub%20Copilot%20app으로%20애플리케이션%20만들기/README.md)

빈 로컬 폴더에서 웹 애플리케이션을 만들고 두 Foundry Agent를 연결합니다.

- 빈 폴더와 로컬 Agent 세션 생성
- 화면과 모의 업무 흐름 생성
- Microsoft Entra 인증과 Foundry Responses API 연동
- 추천 결과를 의견서 입력으로 전달

---

### [5. 결과 확인](./5.%20결과%20확인/README.md)

정책자금 추천부터 영업점 의견서 생성까지 전체 업무 흐름을 실행하고 결과를 확인합니다.

- File Search 근거와 프로그램 코드 확인
- 추천 결과에서 의견서로 이어지는 흐름 확인
- 오류 처리와 보안 항목 확인
- 교육용 Azure 계정 로그아웃

## 저장소 구성

```text
.
├── 1. 사전 준비/
│   ├── README.md
│   └── images/
├── 2. Azure 구독 및 CLI 설정/
│   ├── README.md
│   └── images/
├── 3. Microsoft Foundry Agent 구성/
│   ├── README.md
│   └── images/
├── 4. GitHub Copilot app으로 애플리케이션 만들기/
│   ├── README.md
│   └── images/
├── 5. 결과 확인/
│   ├── README.md
│   └── images/
└── data/
	└── policy-funds-sample.md
```

각 단계 폴더의 `README.md`를 번호 순서대로 진행합니다. `images/`에는 해당 단계의 화면 캡처를, `data/`에는 여러 단계에서 공통으로 사용하는 입력 자료를 둡니다.