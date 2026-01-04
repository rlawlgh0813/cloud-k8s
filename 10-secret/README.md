# 10 — Secret

Secret은 쿠버네티스에서 민감한 정보를 안전하게 관리하기 위한 리소스입니다.  
ConfigMap과 유사하지만, 비밀번호·토큰·인증 정보처럼 노출되면 안 되는 데이터를 저장하는 데 사용됩니다.

---

## Why Secret

다음과 같은 정보는 ConfigMap이 아닌 Secret으로 관리해야 합니다.

- 비밀번호
- 인증 토큰
- API Key
- 민감한 환경 변수 값

Secret을 사용하면  
설정 정보를 컨테이너 이미지나 코드와 분리하면서  
**보안 수준을 한 단계 높일 수 있습니다.**

---

## Core Concepts

- Secret은 key-value 형태의 데이터 저장소
- 내부적으로 Base64 인코딩되어 저장됨
- Pod에서는
  - 환경변수로 사용하거나
  - 파일 형태로 마운트 가능
- 기본적으로 읽기 전용으로 마운트 가능

---

## 실습 0 — Secret 조회 및 확인

### 목적

Secret 리소스를 조회하고,  
Pod 내부에서 Secret 값이 어떻게 제공되는지 확인합니다.

### 실습 내용

- Secret 목록 조회
- 특정 Secret 상세 조회
- Pod 내부로 진입
- Secret이 파일 형태로 존재하는지 확인

---

## 실습 1 — Secret을 환경변수로 사용

### 목적

Secret의 특정 key를  
컨테이너 환경변수로 주입하는 방식을 실습합니다.

### 실습 내용

- Secret 생성
- Secret의 key를 환경변수로 매핑
- 컨테이너에서 환경변수 값 출력
- 로그를 통해 값 확인

### 확인 포인트

- valueFrom.secretKeyRef를 통해 특정 key만 선택 가능
- 환경변수 이름과 Secret key 이름은 다를 수 있음

---

## 실습 2 — Secret을 파일로 사용

### 목적

Secret을 볼륨으로 마운트하여  
파일 형태로 사용하는 방식을 이해합니다.

### 실습 내용

- Secret을 기반으로 볼륨 생성
- 컨테이너 내부 특정 경로에 Secret 마운트
- Secret key가 파일로 생성되는 구조 확인

### 확인 포인트

- Secret은 기본적으로 readOnly로 마운트
- 각 key는 개별 파일로 생성됨
- 파일명을 통해 Secret 값 접근 가능

---

## Verification

아래 항목을 기준으로 Secret 동작을 검증했습니다.

- Secret 생성 및 조회 여부
- Pod 환경변수 정상 주입 여부
- 파일 마운트 방식 정상 동작 여부
- 컨테이너 내부에서 Secret 값 접근 가능 여부

---

## Common Checks

실습 중 자주 사용한 명령들입니다.

- kubectl get secret
- kubectl describe secret <secret_name>
- kubectl exec -it <pod_name> -- sh
- kubectl logs <pod_name>

---

## Troubleshooting / Pitfalls

- Secret 값은 Base64 인코딩일 뿐 암호화는 아님
- Secret 수정 후  
  → 실행 중인 Pod에는 즉시 반영되지 않음
- 실수로 Secret을 로그에 출력하지 않도록 주의 필요

---

## Files

- secret-env.yaml  
  → 환경변수로 Secret 사용
- secret-volume.yaml  
  → 파일 형태로 Secret 사용