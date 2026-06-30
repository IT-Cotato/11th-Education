# CI/CD와 GitHub Actions

### CI/CD 개념

CI/CD는 소프트웨어 개발의 핵심적인 두 가지 개념인 지속적 통합(Continuous Integration)과 지속적 배포(Continuous Delivery/Deployment)를 포괄하는 용어.

<br></br>

### CI : Continuous Integration

여러 개발자가 작성한 코드를 주기적으로 메인 코드베이스에 통합하여 코드 충돌이나 오류를 조기에 발견하고 해결하는 과정.

마치 여러 요리사가 각자 맡은 재료를 준비하고, 그때마다 맛을 보며 문제가 없는지 확인하는 것과 같음.

<br></br>

### CD : Continuous Delivery/Deployment

CD는 두 가지 개념으로 나뉨.

- **지속적 전달 (Continuous Delivery)** : 코드가 언제든지 프로덕션 환경(사용자에게)에 배포될 준비가 되었음을 의미. CI 과정을 거쳐 배포 가능한 상태로 만들어지지만, 최종 배포는 수동 승인을 통해 이루어짐.
- **지속적 배포 (Continuous Deployment)** : 코드 변경 사항이 테스트를 통과하면 자동으로 프로덕션 환경에 배포됨. 사람의 개입 없이 모든 과정이 자동으로 진행됨.

CI가 재료 준비와 중간 맛보기라면, CD는 준비된 요리를 손님에게 내놓는 과정.

'Delivery'는 요리가 언제든 나갈 준비가 되어 있지만 서빙은 사람이 결정하는 것이고, 'Deployment'는 회전초밥처럼 요리가 완성되면 자동으로 손님 테이블로 나가는 것에 비유할 수 있음.

<br></br>

### CI/CD 파이프라인

개발자가 배포할 때마다 일일이 빌드와 배포를 진행하는 것이 한 두 번이면 괜찮겠지만, 이 과정이 수없이 반복되면 번잡스럽고 지루해짐.

그래서 수없이 진행되는 배포 과정을 자동화시키도록 구축하게 되는데, 이를 **CI/CD 파이프라인**이라고 함.

<br></br>

# GitHub Actions

### GitHub Actions 개념

CI/CD 파이프라인을 구축할 수 있도록 GitHub가 제공하는 CI/CD 도구.

개발 업무를 마무리하기 위해 순차적으로 수행하는 작업들을 '워크플로우(Workflow)'라고 부르며, GitHub Actions는 이러한 워크플로우를 자동으로 실행시켜줌.

<br></br>

### GitHub Actions의 구성

구성 요소: Event, Job, Runner, Step

- **workflow**가 가장 큰 작업 단위.
- 하나의 workflow 안에는 여러 개의 **job**이 존재하며 병렬/직렬로 작동.
- job에서는 해당 job이 구동될 환경인 **runner**를 지정하고, **step**들이 순차적으로 실행됨.
- Job이 실행되기 전에 GitHub는 해당 Job을 실행할 수 있는 Runner를 찾고 실행시킴.

<br></br>

### Workflow

- 작업을 수행하는 데 필요한 모든 정보를 포함하는 자동화된 프로세스. push, pull request open, issue open 등과 같은 특정 이벤트에 대해 실행할 수 있는 작업 흐름을 정의.
- YAML 파일에 의해 정의되며 레포의 이벤트에 의해 트리거될 때 실행되거나, 수동으로 또는 정의된 일정에 따라 트리거될 수 있음.

<br></br>

### Event

- 레포에서 발생하는 push, pull request open, issue open 등의 특정한 활동.
- 특정 Event가 발생했을 시 그에 맞는 CI/CD 파이프라인을 구동하도록 설정 가능.

<br></br>

### Jobs

- 하나의 runner에서 실행될 여러 step의 모음.
- 하나의 workflow 안에 여러 job들을 설정 가능.
- workflow의 job들은 기본적으로 **병렬(동시에)**로 실행됨.
- 일부 job은 `needs` 키워드를 통해 다른 job이 완료되고 난 뒤 실행하도록 설정 가능.

<br></br>

### Step

- 실행 가능한 하나의 shell script 또는 action.
- job 안의 step들은 **순차적으로** 실행됨. 한 단계가 실패하면 해당 작업은 중단됨.

<br></br>

### Runner

- job을 실행할 서버.
- 클라우드형 CI/CD 플랫폼인 GitHub Actions는 직접 컴퓨터를 관리할 필요 없이 가상의 Runner를 통해 job을 실행시킴.
- 크게 두 가지로 나눠서 실행 가능:
  - GitHub에서 제공하는 runner 환경 (예: `ubuntu-latest`)
  - 본인이 직접 호스팅하는 self-hosted 환경

<br></br>

### Actions

- 특정 작업을 수행하도록 미리 정의한 코드 블록. workflow의 복잡성을 줄이고 재사용성을 높여줌.
- 예: `actions/checkout@v4` — GitHub에서 공식적으로 제공하는 checkout 액션. 워크플로가 실행되는 Runner에 GitHub 저장소의 코드를 다운로드. `@v4`는 해당 액션의 버전 4를 사용한다는 의미.
- `uses` 키워드에는 여러 유형이 올 수 있으며, GitHub Marketplace에서 필요한 action을 검색해 사용할 수 있음 (예: Google Cloud Run 배포 관련 action).

<br></br>

### YAML 파일의 위치와 역할

모든 GitHub Actions 워크플로우는 GitHub 저장소 내의 `.github/workflows` 디렉토리 안에 YAML 파일(`.yml` 또는 `.yaml`)로 정의됨.

```bash
my-project/
├── .github/
│   └── workflows/
│       └── cotato-pipeline.yml  <-- 여기에 워크플로우 파일 생성
├── src/
├── public/
├── package.json
├── tsconfig.json
├── vite.config.ts
└── ...
```

이 YAML 파일이 GitHub Actions에게 어떤 작업을 언제, 어떻게 수행할지 지시하는 '설계도' 또는 '레시피' 역할.

참고: [GitHub Actions Quickstart 공식 문서](https://docs.github.com/ko/actions/get-started/quickstart)

<br></br>

# 워크플로우 작성 단계

### 1. 워크플로우 이름 지정

YAML 파일 최상단에 `name` 키워드로 워크플로우의 이름을 지정.

```yaml
name: Deploy to Google Cloud Run
```

<br></br>

### 2. Event : 워크플로우를 트리거하는 특정 활동

이벤트는 저장소에서 발생하는 특정 활동으로, 워크플로우 실행을 시작하는 '방아쇠' 역할.

`on` 키워드에 트리거할 이벤트(예: push)를 작성하고, 어느 브랜치에 트리거될 것인지 작성.

```yaml
on:
  push:
    branches:
      - 'main'
```

main 브랜치에 push가 일어날 때 워크플로우가 실행되도록 설정한 예시.

<br></br>

### 3. Jobs : 특정 환경에서 실행되는 단계들의 묶음

```yaml
jobs:
  build-and-deploy:
    name: Build and Deploy
```

- `jobs:` 아래의 `build-and-deploy:`는 해당 job의 **ID** — 워크플로우 YAML 파일 내에서 해당 job을 참조하거나 설정할 때 사용되는 고유 식별자.
- `name:`은 GitHub Actions 콘솔에서 어떻게 표현될지 정의하는 것.

<br></br>

### 4. Runner : 워크플로우를 실행하는 서버 환경

```yaml
runs-on: ubuntu-latest
```

`runs-on:`은 job을 실행할 서버, 즉 job이 어떤 환경에서 실행될 것이냐에 대한 정의.

<br></br>

### 5. Step : 작업 내에서 순차적으로 실행되는 개별 명령

step은 각 작업 내에서 실제 작업을 수행하는 개별적인 명령 또는 태스크. 정의된 순서대로 실행되며, 한 단계가 실패하면 해당 작업은 중단됨.

```yaml
jobs:
  build-and-deploy:
    name: Build and Deploy
    runs-on: ubuntu-latest

    # 권한 설정
    permissions:
      contents: 'read'
      id-token: 'write'

    steps:
      # 1. 소스 코드 체크아웃
      - name: Checkout repository
        uses: actions/checkout@v4

      # 2. WIF를 사용하여 GCP에 인증
      - name: Authenticate to Google Cloud
        id: auth
        uses: 'google-github-actions/auth@v2'
        with:
          workload_identity_provider: ${{ env.GCP_WORKLOAD_IDENTITY_PROVIDER }}
          service_account: ${{ env.GCP_SERVICE_ACCOUNT }}

      # 3. Docker 로그인 설정
      # Artifact Registry에 인증하도록 gcloud CLI 설정
      - name: Configure Docker
        run: gcloud auth configure-docker ${{ env.ARTIFACT_REGISTRY_LOCATION }}-docker.pkg.dev
```

예시의 3개 step:
1. **Checkout repository** — `actions/checkout@v4` 액션으로 레포 코드를 Runner에 다운로드
2. **Authenticate to Google Cloud** — Workload Identity Federation(WIF)을 통한 GCP 인증
3. **Configure Docker** — Google Cloud SDK를 사용해 Artifact Registry 인증 관련 명령어 실행

<br></br>

# 환경 변수와 GitHub Secrets

중요한 키 값을 워크플로우 파일에 하드코딩하면 키가 탈취당할 위험이 있고, 이로 인해 거액의 비용(예시: 5000만원)이 청구될 수도 있음.

이를 방지하기 위해 **워크플로우 환경 변수**와 **GitHub Secrets**을 사용.

```yaml
env:
  # GCP 관련 설정
  GCP_PROJECT_ID: ${{ secrets.GCP_PROJECT_ID }}                       # GCP 프로젝트 ID
  GCP_WORKLOAD_IDENTITY_PROVIDER: ${{ secrets.GCP_WORKLOAD_IDENTITY_PROVIDER }}  # WIF
  GCP_SERVICE_ACCOUNT: ${{ secrets.GCP_SERVICE_ACCOUNT }}             # 접근할 서비스 계정
```

- 워크플로우 파일 내에서 `env.`로 시작하는 변수를 사용하려면, `env` 키워드로 워크플로우 내에서 사용할 환경 변수를 먼저 지정해야 함.
- 이 `env` 변수는 다시 `secrets.`으로 시작하는 키로 지정됨.

**Secrets 설정 위치**: 레포 > Settings > Secrets and variables > Actions > New repository secret

한 번 입력된 값은 수정 가능하나, 기존 값을 직접 보거나 수정하는 형식이 아니라 새롭게 값을 입력하는 형식으로 보안이 유지됨.

<br></br>

# 정리

- **CI**는 코드 통합과 충돌/오류의 조기 발견에 초점.
- **CD**는 통합된 코드를 실제 배포하는 과정으로, 수동 승인(Delivery)과 완전 자동화(Deployment)로 나뉨.
- **CI/CD 파이프라인**은 반복적인 빌드·배포 과정을 자동화한 것.
- **GitHub Actions**는 Event → Job(Runner + Step) 구조의 워크플로우를 통해 CI/CD 파이프라인을 구현.
- 민감 정보는 하드코딩하지 말고 **GitHub Secrets**와 워크플로우 **환경 변수**를 통해 안전하게 관리.
