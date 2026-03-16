# Public Skills

공개 가능한 AI 에이전트용 skill을 모아두는 저장소입니다.

이 저장소의 목적은 특정 도구 하나에 종속되지 않는, 재사용 가능한 공개용 skill 패키지를 배포하는 것입니다. 여기의 skill은 public-safe 해야 하지만, 필요하다면 사용자가 접근 권한을 가진 private source-of-truth를 런타임에 읽는 bootstrap 구조를 사용할 수 있습니다.

## 원칙

- 이 저장소에는 공개해도 되는 내용만 둡니다.
- 비밀값, 토큰, 내부 IP, 내부 전용 도메인, 고객 데이터는 포함하지 않습니다.
- private 레포를 source-of-truth로 참조할 수는 있지만, 그 안의 민감한 값을 public skill 본문에 복사하지는 않습니다.
- 환경별로 달라지는 값은 public skill에 하드코딩하지 않고, 런타임 문서 조회나 placeholder로 처리합니다.
- skill 하나만 설치해도 기본적인 작업 흐름을 시작할 수 있어야 합니다.

## 디렉터리 구조

각 skill은 아래 형태를 따릅니다.

```text
<skill-name>/
  SKILL.md
  references/
    ...
  scripts/
    ...
```

- `SKILL.md`: 언제 이 skill을 써야 하는지와 기본 작업 절차
- `references/`: 상세 계약, 예시, 제약 조건
- `scripts/`: 반복 작업을 줄이기 위한 보조 스크립트가 필요할 때만 추가

## 사용 방식

설치 방식은 사용하는 도구에 따라 다를 수 있지만, 기본 전제는 같습니다.

- GitHub 경로나 로컬 디렉터리에서 개별 skill을 설치하거나 참조할 수 있어야 합니다.
- public skill은 가능한 한 독립적으로 동작해야 합니다.
- 다만 조직 내부 사용자를 위한 bootstrap skill이라면, 권한이 있는 사용자의 환경에서 private source-of-truth를 읽도록 설계할 수 있습니다.

## 공개 전 체크리스트

새 skill을 추가하거나 수정할 때는 아래를 확인합니다.

1. 내부 인프라 정보가 들어 있지 않은가
2. secret 이름, 내부 호스트, 내부 IP, 고객 데이터가 그대로 노출되지 않는가
3. private 문서를 읽어야만 이해되는 구조라면, 적어도 접근 실패 시 어떻게 멈추는지 명확한가
4. 공개 저장소에 올려도 법적/운영상 문제가 없는가
5. public skill이 private source-of-truth를 읽는 경우, 민감값을 로컬/런타임에만 소비하고 public 문서에는 남기지 않는가

## 현재 포함된 skill

- `prototype-deployer`: `CINEV/prototype-deployer`의 `docs/ai`를 런타임에 읽고 다른 서비스 레포에 배포 체계를 붙이기 위한 bootstrap skill

## 권장 프롬프트

`prototype-deployer` skill을 설치한 뒤에는 아래처럼 요청하면 됩니다.

```text
prototype-deployer 스킬로 이 레포 배포 준비 해줘.
```

조금 더 안정적으로 작업시키고 싶다면 이렇게 요청하는 편이 좋습니다.

```text
prototype-deployer 스킬로 이 레포 배포 준비 해줘. 먼저 CINEV/prototype-deployer의 docs/ai를 읽고 그 기준으로 deploy.yaml, deploy workflow, 필요하면 Dockerfile까지 정리해줘.
```

## 유지 원칙

공개 skill은 가능한 한 짧고 명확해야 합니다. 세부 구현은 참조 문서로 분리하되, 민감한 계약은 public 저장소에 복사하지 않고 source-of-truth에서 가져오는 방향을 우선합니다.
