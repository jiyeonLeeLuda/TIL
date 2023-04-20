# CICD

**updated 2023.04.20, 2023.04.18**

- CI/CD 란? 왜 해야하는 것일까?
  - 우리 팀에서 어떻게 적용할지 고민해보기.

## CICD란?

소프트웨어 공학에서 CI/CD는

- 지속적 통합(영어: continuous integration)과
- 지속적 배포(영어: continuous delivery, CD)가 결합한 사례를 의미한다.

### Continuous Integration, 지속적 통합이란?

> 지속적 통합의 실행은 소스/버전 관리 시스템에 대한 변경 사항을 정기적으로 커밋하여 모든 사람에게 동일 작업 기반을 제공하는 것으로 시작합니다. **커밋할 때마다 빌드와 일련의 자동 테스트가 이루어져 동작을 확인하고 변경으로 인해 문제가 생기는 부분이 없도록 보장합니다.** 지속적 통합은 그 자체로 유익하지만 CI/CD 파이프라인을 구현하기 위한 첫 번째 단계이기도 합니다.

### Continuous Delivery, Continuous Deployment, 지속적 배포란?

> 코드 변경이 파이프라인의 이전 단계(자동화된 빌드 및 테스트 프로세스)를 모두 성공적으로 통과하면 수동 개입 없이 해당 변경 사항이 프로덕션에 자동으로 배포됩니다. 강력하고 신뢰할 수 있는 자동화 배포 파이프라인을 구축하면 하루에도 여러 번 이루어지는 릴리스가 특별하지 않은 일상이 됩니다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FckMQmf%2FbtreLLPDmsL%2FtoxwM0zsTV38PrftEhmbt1%2Fimg.png"/>

참고 [CI/CD란?](https://seosh817.tistory.com/104)

## CI/CD의 장점

- 더 작은 단위로 코드 수정 및 배포가 가능하다.
- 업데이트 및 유지 관리가 쉽다.
- 자동화를 통해 배포 속도가 빨라진다.
- 장애 지점을 사전에 파악이 가능하여 해결시간이 단축되고, 백로그가 감소한다.
- 테스트 코드의 신뢰성이 올라가고, 팀내 공유하는 정보의 투명성이 증가한다.
- 이를 통해 고객 만족도가 올라간다.

## CI/CD 도구를 선택시 고려할 점.

- 어떤 기능이 있는가.
- 플러그인등 확장 프로그램이 잘 구비 되어있는가.
- 지원되는 프로그래밍 언어 및 플랫폼
- 예산 (비용)

## CI/CD Tools

- [버디](https://buddy.works/)

  - 웹 개발자를 위한 CI/CD 솔루션
  - 간단하고 유익한 UI/UX로 15분 구성
  - 변경 세트에 따라 순식간에 이루어지는 배포
  - 빌드는 캐시된 종속성이 있는 격리된 컨테이너에서 작동합니다.
  - 널리 사용되는 모든 언어, 프레임워크 및 작업 관리가 지원됩니다.
  - 전용 Docker/Kubernetes 작업 명단
  - YAML 구성 및 병렬 처리 지원

- [젠킨스](https://www.jenkins.io/)

  - 강력한 공동체 의식을 지닌 오픈 소스 도구입니다.
  - 설정하기 쉽습니다.
  - 완전 무료입니다.
  - Java를 사용하여 생성되므로 모든 주요 플랫폼에서 실행될 수 있습니다.

- [깃랩 (깃허브 액션)](https://docs.gitlab.com/ee/ci/)

  - 구성 단순성
  - 소스 코드 안전성
  - 파이프라인 자동화
  - 배포 계획가능

- [서클 CI](https://circleci.com/ko/)

  - 빌드 문제를 조사하기 위해 모든 작업에 SSH를 통합합니다.
  - config.yml 파일에서 병렬 처리를 활성화하여 작업 실행을 가속화합니다.
  - 두 개의 간편한 키로 캐싱을 구성하여 이전 작업의 데이터를 재사용할 수 있습니다.
  - 특정 플랫폼을 지원하도록 자체 호스팅 러너를 설정합니다.

- [트래비스 CI](https://www.travis-ci.com/)

  - 아티팩트 생성 및 코드 품질 평가
  - 간단한 클라우드 서비스 배포
  - 사소한 코드 수정과 실질적인 코드 수정을 모두 감지할 수 있습니다.

- [AWS 파이프라인](https://aws.amazon.com/ko/getting-started/hands-on/set-up-ci-cd-pipeline/)

  - AWS는 전체 DevOps CI CD 도구 제품군을 제공합니다.
  - 코드용 파이프라인은 다른 서비스에 연결할 수 있습니다. GitHub와 같은 타사 제품이나 Amazon Simple Storage Service와 같은 AWS 서비스일 수 있습니다.
  - AWS CodeBuild를 사용하여 코드 빌드, 테스트 및 컴파일 지원
  - 컨테이너 기반 앱은 지속적으로 클라우드로 전송됩니다.

## 장점에도 불구하고 CI/CD를 업무에 도입하고 있지 못하는 이유

(KuberCon+ 설문조사 결과 아래와 같은 이유로 32%는 CI/CD를 사용하지 않는다고 응답함)

- 구축/운영에 필요한 스킬셋이 부족해서
- 어떤 툴을 써야할지 모르겠어서
- Best Practice에 대한 지식 부족
- 전통적인 개발 방식에서 벗어나기 어려워서

실제로 온프레미스(!== 클라우드) 환경에서의 CI/CD구축는 시작 장벽이 높다.

- CI 환경 구성 (ex.젠킨스)
- SCM 환경 구성 (ex. git)
- 자동화 도구 구축(ex. maven)
- 컨테이너 오케스트레이션 환경 구성 (ex. kubernetes)

클라우드 상에서 CI/CD는 상대적으로 쉽다.

## Naver 클라우드 플랫폼 DevOps Tools

- CI/CD 상품

  - SorceCommit
    - git과 연동 가능 및, 동일한 명령어 제공
  - SorceBuild
    - 빌드 & 프로비저닝
  - SorceDeploy
    - 배포
    - 스테이지 별로 다양하게 구성 가능.
  - SorcePipeline
    - Commit,Build,Deploy를 통합 및 자동화 하는 기능제공.

- 보안 상품

  - FileSafer
  - AppSecurityChecker

- +DevOps
  - Jenkins
  - KubernetesService
  - ContainerRegistory
  - CloudFunctions

참고 [클라우드 상에서 효율적인 CI/CD 사용 방법](https://www.edwith.org/nclouddevtools/lecture/350981?isDesc=false)
