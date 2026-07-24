# 사전 준비 점검

[처음으로](../README.md) | [다음: Foundry 구성](01-foundry.md)

## 준비물

| 항목 | 이 실습에서 사용하는 용도 | 준비 완료 기준 |
| --- | --- | --- |
| Azure 구독과 Microsoft Entra 계정 | Foundry 프로젝트, 모델, 에이전트 생성 | Azure Portal에서 실습 구독을 열 수 있습니다. |
| GitHub 계정과 Copilot 사용 권한 | 저장소 Fork와 Copilot 에이전트 세션 | GitHub Copilot 화면에서 프롬프트를 입력할 수 있습니다. |
| GitHub Copilot app | 독립형 데스크톱 개발 환경 | 앱에 로그인한 뒤 **Sessions** 메뉴가 보입니다. |
| Git | 저장소 Clone, 변경 비교, Pull Request 생성 | `git --version`이 버전을 출력합니다. |
| Node.js LTS와 npm | 웹 앱 실행과 테스트 | `node --version`과 `npm --version`이 버전을 출력합니다. |
| Azure CLI | Azure 로그인과 구독 선택 | `az account show`가 실습 구독을 출력합니다. |

아래 절차는 Windows와 PowerShell을 기준으로 합니다. 설치 명령을 실행하기 전에 Windows Package Manager를 확인합니다.

```powershell
winget --version
```

명령을 찾을 수 없다면 Microsoft Store에서 **앱 설치 관리자**를 설치하거나 업데이트한 뒤 새 PowerShell을 엽니다. 회사에서 설치가 제한된 PC에서는 임의로 보안 정책을 우회하지 말고 관리자에게 설치를 요청합니다.

## Azure 구독과 Microsoft Entra 계정 준비

1. [Azure Portal](https://portal.azure.com/)에 실습용 Microsoft Entra 계정으로 로그인합니다. 강사가 계정을 제공했다면 해당 계정을 사용합니다.
2. 첫 로그인에서 요구하는 MFA 등록을 완료합니다.
3. **Subscriptions** 에서 강사가 지정한 구독이 보이는지 확인합니다.
4. [Microsoft Foundry](https://ai.azure.com/)도 같은 계정으로 열리는지 확인합니다.

개인 실습에 사용할 구독이 없다면 [Azure 무료 계정](https://azure.microsoft.com/pricing/purchase-options/azure-account)을 만들 수 있습니다. 조직 실습에서는 새 구독을 만들기 전에 강사에게 확인합니다.

**확인:** Azure Portal에서 실습 구독을 선택할 수 있고 Microsoft Foundry 첫 화면이 열립니다.

## Azure CLI 설치와 구독 선택

```powershell
winget install --exact --id Microsoft.AzureCLI
```

WinGet을 사용할 수 없다면 [Azure CLI Windows 설치 안내](https://learn.microsoft.com/cli/azure/install-azure-cli-windows)의 MSI를 사용합니다. 설치 후 새 PowerShell에서 로그인합니다.

```powershell
az version
az login
az account list --output table
az account set --subscription "구독 이름 또는 ID"
az account show --output table
```

구독이 하나라면 `az account set`은 생략할 수 있습니다.

**확인:** `az account show --output table`에 올바른 실습 구독이 표시됩니다.

## 권한 확인

직접 Foundry 리소스와 프로젝트를 만들려면 구독 또는 리소스 그룹에 다음 중 하나가 필요합니다.

- `Foundry Account Owner`
- `Foundry Owner`
- Azure `Owner` 또는 `Contributor`

강사가 프로젝트를 미리 만들었다면 프로젝트 범위의 `Foundry User` 역할만으로 모델과 에이전트를 사용할 수 있습니다. 역할 이름 변경이 아직 반영되지 않은 화면에는 이전 이름인 `Azure AI User`가 보일 수 있습니다.

## 전체 도구 확인

새 PowerShell에서 한 번에 확인합니다.

```powershell
git --version
node --version
npm --version
az version
```

하나라도 명령을 찾을 수 없으면 해당 항목의 설치 절차를 다시 확인하고 PowerShell을 새로 엽니다. 토큰, 구독 키, 암호를 채팅, 코드, `.env.example`에 붙여 넣지 않습니다.

## 저장소 준비

이 저장소를 Fork한 뒤 GitHub Copilot app에서 다음 순서로 엽니다.

1. 왼쪽 **Sessions** 옆의 **+** 를 선택합니다.
2. **Add project from**에서 **GitHub repository**를 선택해 Fork를 Clone합니다.
3. 이미 Clone했다면 **Local folder or repository**를 선택합니다.
4. 세션 실행 위치는 **local repository**를 선택합니다.

저장소가 올바르게 열렸는지 확인합니다.

```powershell
git status --short
```

이 저장소에는 완성된 앱 소스와 `package.json`이 없습니다. 앱 개발 단계에서 Copilot이 생성하므로 지금은 `npm install`을 실행하지 않습니다.

## 완료 확인

- [ ] GitHub Copilot app에서 저장소가 열립니다.
- [ ] `git status --short`를 실행할 수 있습니다.
- [ ] `az account show`에 올바른 구독이 표시됩니다.
- [ ] Foundry 프로젝트를 만들 권한이 있거나 강사 프로젝트에 접근할 수 있습니다.

[다음: Foundry 구성](01-foundry.md)