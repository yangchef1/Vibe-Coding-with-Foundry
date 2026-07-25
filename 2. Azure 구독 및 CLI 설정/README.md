# 2. Azure 구독 및 CLI 설정

이 단계에서는 Azure CLI를 설치하고 교육용 계정과 구독을 선택한 뒤, Foundry 프로젝트의 연결 정보를 확인합니다.

## Azure CLI 설치

아래 명령을 실행합니다.

```powershell
winget install --exact --id Microsoft.AzureCLI
```

설치가 끝나면 다음과 같이 진행합니다.

1. 열려 있는 PowerShell 창을 모두 닫습니다.
2. 새 PowerShell을 엽니다.
3. `az version`을 다시 실행합니다.

`winget`도 찾을 수 없다면 [Windows용 Azure CLI 공식 설치 페이지](https://learn.microsoft.com/cli/azure/install-azure-cli-windows)에서 64비트 MSI 설치 파일을 사용합니다.

## Azure 계정 로그인

1. PowerShell에서 아래 명령을 실행합니다.

    ```powershell
    az login
    ```

2. Windows 계정 선택 창이나 브라우저가 열리면 교육용 계정을 사용합니다.
3. 구독 선택 목록이 나오면 안내받은 구독 번호를 입력합니다.
4. 암호를 PowerShell 명령에 직접 적지 않습니다.

현재 계정과 구독을 확인합니다.
다시 `az account show`를 실행해 올바른 구독이 표시되는지 확인합니다.

## Microsoft Foundry 프로젝트 접속

### 프로젝트 열기

1. 브라우저에서 [Microsoft Foundry](https://ai.azure.com)를 엽니다.
2. Azure CLI에서 사용한 교육용 계정으로 로그인합니다.
3. 전환 스위치가 보이면 `New Foundry`가 켜져 있는지 확인합니다.
4. 왼쪽 위 프로젝트 이름을 클릭합니다.
5. 안내받은 Foundry 프로젝트를 선택합니다.

프로젝트가 보이지 않거나 권한 오류가 나오면 새 프로젝트를 만들지 않습니다. 로그인 계정, 테넌트, 프로젝트 역할을 먼저 확인합니다.

### 모델 배포 확인

1. 오른쪽 위 `Build`를 클릭합니다.
2. 왼쪽 메뉴에서 `Models`를 클릭합니다.
3. 안내받은 모델 배포가 목록에 있는지 확인합니다.

모델이 없다면 실습 중 새 모델을 임의로 배포하지 않고 준비된 배포 이름과 프로젝트를 다시 확인합니다.

### 프로젝트 엔드포인트 기록

1. 프로젝트의 `Home` 또는 시작 화면으로 이동합니다.
2. `Project endpoint` 옆 복사 버튼을 클릭합니다.
3. 메모장에 아래 형식으로 기록합니다.

    ```text
    PROJECT_ENDPOINT=https://리소스이름.services.ai.azure.com/api/projects/프로젝트이름
    RECOMMENDER_AGENT=fund-recommender-<alias>
    OPINION_AGENT=branch-opinion-writer-<alias>
    ```

프로젝트 엔드포인트 끝에 `/openai/v1`이나 `/responses`를 추가하지 않습니다.

다음 단계: [3. Microsoft Foundry Agent 구성](../3.%20Microsoft%20Foundry%20Agent%20구성/README.md)