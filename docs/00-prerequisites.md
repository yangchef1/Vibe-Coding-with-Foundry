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
3. **Subscriptions**에서 강사가 지정한 구독이 보이는지 확인합니다.
4. [Microsoft Foundry](https://ai.azure.com/)도 같은 계정으로 열리는지 확인합니다.

개인 실습에 사용할 구독이 없다면 [Azure 무료 계정](https://azure.microsoft.com/pricing/purchase-options/azure-account)을 만들 수 있습니다. 조직 실습에서는 새 구독을 만들기 전에 강사에게 확인합니다.

**확인:** Azure Portal에서 실습 구독을 선택할 수 있고 Microsoft Foundry 첫 화면이 열립니다.

## GitHub 계정과 Copilot 사용 권한 준비

1. [GitHub](https://github.com/login)에 로그인합니다. 계정이 없다면 **Create an account**에서 계정을 만듭니다.
2. 조직 계정이라면 SSO 인증을 완료합니다.
3. [GitHub Copilot](https://github.com/copilot)을 열어 프롬프트 입력창이 보이는지 확인합니다.
4. 플랜 선택 화면이 보이면 Copilot 플랜을 활성화하거나 조직 관리자에게 Copilot 사용 권한을 요청합니다.

Copilot Business 또는 Enterprise 계정은 관리자가 **Copilot CLI** 정책을 허용해야 로컬 에이전트 세션을 사용할 수 있습니다.

**확인:** 실습 저장소를 Fork할 계정으로 로그인되어 있고 Copilot 프롬프트를 입력할 수 있습니다.

## Git 설치와 사용자 정보 설정

```powershell
winget install --id Git.Git -e --source winget
```

WinGet을 사용할 수 없다면 [Git for Windows](https://git-scm.com/install/windows)의 Windows 설치 파일을 사용합니다. 설치 후 새 PowerShell을 열어 버전과 커밋 작성자 정보를 설정합니다.

```powershell
git --version
git config --global user.name "GitHub 표시 이름"
git config --global user.email "GitHub에 등록한 이메일"
```

**확인:** `git --version`이 버전을 출력합니다.

## GitHub Copilot app 설치와 최초 실행

1. [GitHub Copilot app 다운로드 페이지](https://github.com/features/ai/github-app)를 엽니다.
2. Windows용 설치 파일을 내려받아 실행합니다.
3. 앱을 열고 **Sign in to GitHub**를 선택한 뒤 브라우저에서 로그인을 승인합니다.
4. 저장소 선택은 건너뛰어도 됩니다. 테마를 선택하고 초기 설정을 완료합니다.
5. 왼쪽 **Sessions** 옆의 **+**를 선택해 **Local folder or repository** 또는 **GitHub repository**가 보이는지 확인합니다.

> [!IMPORTANT]
> 이 실습에서 말하는 GitHub Copilot app은 독립형 데스크톱 앱입니다. VS Code의 GitHub Copilot 확장이나 GitHub Desktop과 다른 프로그램입니다.

**확인:** 앱에 본인 계정이 표시되고 **Sessions**에서 새 프로젝트를 추가할 수 있습니다.

## Node.js LTS와 npm 설치

```powershell
winget install --id OpenJS.NodeJS.LTS -e --source winget
```

WinGet을 사용할 수 없다면 [Node.js 다운로드 페이지](https://nodejs.org/en/download)에서 Windows Installer를 사용합니다. `npm`은 함께 설치됩니다. 새 PowerShell에서 확인합니다.

```powershell
node --version
npm --version
```

**확인:** Node.js가 `v24`이고 npm 버전도 출력됩니다.

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

## 준비를 못 한 경우의 구제 경로

| 증상 | 진행 방법 |
| --- | --- |
| Azure 권한이 없음 | 강사가 준비한 프로젝트를 사용하고 `Foundry User` 역할을 요청합니다. |
| 모델 배포 권한 또는 할당량이 없음 | 강사가 배포한 모델과 에이전트를 공유받습니다. |
| Azure CLI 로그인이 안 됨 | 앱 생성까지 진행하고 실제 Foundry 응답 확인은 로그인 가능한 실습 PC에서 수행합니다. |
| GitHub Copilot app 설치가 막힘 | 설치된 실습 PC를 함께 사용하고 본인 Fork에는 결과만 반영합니다. |
| Node.js 설치가 안 됨 | Node.js가 설치된 실습 PC를 사용합니다. cloud sandbox는 로컬 Azure 로그인 상태를 공유하지 않습니다. |

## 완료 확인

- [ ] GitHub Copilot app에서 저장소가 열립니다.
- [ ] `git status --short`를 실행할 수 있습니다.
- [ ] `az account show`에 올바른 구독이 표시됩니다.
- [ ] Foundry 프로젝트를 만들 권한이 있거나 강사 프로젝트에 접근할 수 있습니다.

[다음: Foundry 구성](01-foundry.md)