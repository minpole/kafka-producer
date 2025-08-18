# appspec.yml 설명서

이 문서는 AWS CodeDeploy 등에서 사용되는 `appspec.yml` 파일의 예시와 각 항목의 의미를 설명합니다.

---

## 기본 구조

- **version**: 앱스펙 파일의 버전 (필수, 현재는 0.0만 지원)
- **os**: 배포 대상 서버의 운영체제 (linux 또는 windows)

## files
- 소스 파일/디렉토리를 대상 서버의 경로로 복사하는 규칙을 지정
- 여러 개의 source, destination 지정 가능
- source가 디렉토리면 내부 파일/디렉토리만 복사됨 (디렉토리 자체는 복사되지 않음)
- source가 파일이면 해당 파일만 복사됨

예시:
```yaml
files:
  - source: /
    destination: /src/kafka-producer
```

## file_exists_behavior
- 대상 경로에 동일 파일이 있을 때 동작 지정
- `DISALLOW` | `OVERWRITE` | `RETAIN` 중 선택
- 예시: `OVERWRITE` (덮어쓰기)

## permissions
- 복사된 파일/디렉토리의 소유자, 그룹, 권한 설정
- pattern, except 키워드로 상세 지정 가능

예시:
```yaml
permissions:
  - object: /src/kafka-producer
    owner: kafka
    group: kafka
    mode: 755
```

## hooks
- 배포 과정의 특정 시점에 실행할 스크립트 지정
- 예시: AfterInstall 단계에서 쉘 스크립트 실행

```yaml
hooks:
  AfterInstall:
    - location: deploy/after_install.sh
      timeout: 120
      runas: kafka
```

---

이 파일은 배포 자동화 시 파일 복사, 권한 설정, 배포 후 작업 등을 정의하는 데 사용됩니다.
