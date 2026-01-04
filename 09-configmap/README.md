# 09 — ConfigMap

ConfigMap은 쿠버네티스에서 컨테이너 실행에 필요한 설정 정보를 코드와 분리해서 관리하기 위한 리소스입니다.  
애플리케이션 이미지를 수정하지 않고도 설정을 변경할 수 있어 재사용성과 유지보수성을 높일 수 있습니다.

---

## Why ConfigMap

다음과 같은 정보는 컨테이너 이미지에 포함시키지 않는 것이 바람직합니다.

- 환경 변수 값
- 포트 번호
- 애플리케이션 설정 값
- 문자열 기반 설정 정보

ConfigMap을 사용하면  
**이미지는 그대로 두고 설정만 유연하게 변경**할 수 있습니다.

---

## Core Concepts

- ConfigMap은 key-value 형태의 설정 저장소
- Pod는 ConfigMap을
  - 환경변수로 사용하거나
  - 파일 형태로 마운트 가능
- 하나의 ConfigMap은 여러 Pod에서 재사용 가능

---

## 실습 0 — ConfigMap 생성 및 확인 (CLI)

### 목적

CLI를 통해 ConfigMap을 생성하고,  
저장된 설정 값을 확인하는 흐름을 익힙니다.

### 실습 내용

- literal 값을 이용한 ConfigMap 생성
- ConfigMap 목록 조회
- 특정 ConfigMap 상세 조회
- Pod 내부에서 환경변수 확인

---

## 실습 1 — ConfigMap을 환경변수로 사용

### 목적

ConfigMap의 특정 key를  
컨테이너 환경변수로 주입하는 방식을 실습합니다.

### 실습 내용

- ConfigMap 생성
- ConfigMap의 key 하나를 환경변수로 매핑
- 컨테이너에서 해당 값 출력
- kubectl logs를 통해 출력 결과 확인

### 확인 포인트

- ConfigMap key와 Pod 환경변수 이름은 다를 수 있음
- valueFrom을 통해 특정 key만 선택 가능

---

## 실습 2 — ConfigMap을 파일로 사용

### 목적

ConfigMap의 모든 key-value를  
컨테이너 환경변수로 한 번에 제공하는 방식을 이해합니다.

### 실습 내용

- 여러 key를 포함한 ConfigMap 생성
- envFrom을 이용해 모든 key를 환경변수로 주입
- 컨테이너에서 환경변수 접근 확인

### 확인 포인트

- ConfigMap의 모든 key가 환경변수로 자동 생성됨
- 설정 변경 시 Pod 재시작이 필요함

---

## Verification

아래 항목을 기준으로 ConfigMap 동작을 검증했습니다.

- ConfigMap 생성 및 조회 여부
- Pod 환경변수 정상 주입 여부
- 로그를 통한 설정 값 출력 확인
- 여러 key-value 설정 동시 적용 여부

---

## Common Checks

실습 중 자주 사용한 명령들입니다.

- kubectl get configmap
- kubectl describe configmap <configmap_name>
- kubectl exec -it <pod_name> -- sh
- kubectl logs <pod_name>

---

## Troubleshooting / Pitfalls

- ConfigMap 수정 후  
  → 실행 중인 Pod에는 즉시 반영되지 않음
- key 이름 오타 시  
  → 환경변수가 생성되지 않음
- 민감한 정보(password 등)는  
  → ConfigMap이 아닌 Secret 사용 필요

---

## Files

- configmap.yaml  
  → ConfigMap 기본 정의
- configmap-env.yaml  
  → 환경변수로 ConfigMap 사용