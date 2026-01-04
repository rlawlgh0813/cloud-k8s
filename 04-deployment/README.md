# 04 — Deployment

Deployment는 쿠버네티스에서 **Pod를 항상 원하는 개수만큼 유지하고,  
업데이트와 복구를 자동으로 관리하는 컨트롤러 리소스**입니다.

---

## Core Idea

- Pod가 죽어도 자동으로 복구
- 항상 지정한 개수(replicas)를 유지
- 서비스 중단 없이 업데이트 가능
- 문제 발생 시 이전 상태로 복구 가능

Deployment의 구조는 다음과 같습니다.

Deployment (정책)  
→ ReplicaSet (Pod 개수 유지)  
→ Pod (실제 실행 단위)

---

## Deployment 기본 구조

Deployment는 다음 요소로 구성됩니다.

- replicas  
  → 유지할 Pod 개수
- selector.matchLabels  
  → 관리할 Pod의 라벨 지정
- template  
  → 생성할 Pod의 템플릿 (Pod YAML과 동일)

중요한 점은 selector와 template.labels가 반드시 일치해야 한다는 것입니다.  
일치하지 않으면 Deployment가 자신의 Pod를 인식하지 못합니다.

---

## 실습 0 — Deployment 조회

### 목적

Deployment 상태와 배포 이력을 확인하는 기본 명령 흐름을 익힙니다.

### 실습 내용

- Deployment 목록 조회
- Deployment 상세 정보 확인
- rollout history를 통한 배포 이력 확인
- 이전 revision으로 rollback 가능함을 확인

---

## 실습 1 — nginx Deployment 배포

### 목적

Deployment를 이용해 nginx Pod를 배포하고,  
외부에서 접근 가능한 서비스 형태로 실행합니다.

### 실습 내용

- nginx 이미지를 사용하는 Deployment 생성
- replicas를 통한 Pod 개수 유지 확인
- Service(NodePort)를 통해 외부 접근
- 브라우저에서 정상 접속 확인

### 확인 포인트

- Deployment 생성 시 ReplicaSet이 자동 생성됨
- Pod는 직접 생성하지 않아도 Deployment가 관리함

---

## 실습 2 — Rolling Update & Rollback

### 목적

Deployment의 핵심 기능인  
**무중단 업데이트와 롤백**을 실습합니다.

### 실습 내용

- nginx 이미지 버전 변경을 통한 rolling update
- rollout history로 revision 확인
- rollout undo로 이전 버전으로 복구
- toleration을 추가하여 taint된 노드에도 Pod 배치 허용

### 핵심 관찰

- 이미지 변경은 Pod를 직접 수정하는 것이 아님
- Deployment가 새로운 ReplicaSet을 생성
- Pod가 점진적으로 교체됨
- rollback 시 이전 ReplicaSet으로 즉시 복구됨

---

## Verification

아래 항목을 기준으로 실습 결과를 검증했습니다.

- Deployment 상태 정상 여부
- ReplicaSet 생성 및 변경 확인
- Pod 개수 유지 여부
- 이미지 업데이트 후 Pod 교체 흐름
- rollback 이후 상태 복구 여부

---

## Files

- deploy.yaml  
  → nginx Deployment 기본 배포
- deploy-rollback.yaml  
  → 이미지 변경 및 rollback 실습