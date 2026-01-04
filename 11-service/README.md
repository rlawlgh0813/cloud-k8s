# 11 — Service

Service는 쿠버네티스에서 라벨을 기준으로 여러 Pod를 하나의 네트워크 엔드포인트로 묶어주는 리소스입니다.  
Pod가 재생성되거나 IP가 변경되어도, Service의 IP와 접근 방식은 유지되어 안정적인 통신을 제공합니다.

---

## Why Service

Pod는 다음과 같은 특성을 가집니다.

- 언제든지 재생성될 수 있음
- IP 주소가 고정되지 않음
- 개수가 동적으로 변함

Service는 이러한 Pod들을 묶어  
**항상 동일한 주소로 접근할 수 있도록 추상화 계층**을 제공합니다.

---

## Core Concepts

- Service는 라벨 셀렉터로 Pod를 선택
- Service는 고정된 IP를 가짐
- Pod 변경과 무관하게 동일한 엔드포인트 제공
- 내부적으로 Pod 간 로드밸런싱 수행
- L4 계층에서 트래픽 전달

---

## Service Types

이번 실습에서는 다음 세 가지 타입을 다뤘습니다.

- ClusterIP  
  → 클러스터 내부 전용 통신
- NodePort  
  → 노드 포트를 통한 외부 접근
- LoadBalancer  
  → 외부 로드밸런서를 통한 접근

---

## 실습 0 — Service 조회

### 목적

Service 리소스의 상태와  
Pod 연결 정보를 확인합니다.

### 실습 내용

- Service 목록 조회
- 특정 Service 상세 정보 확인
- 연결된 Pod 정보 확인

---

## 실습 1 — NodePort Service

### 목적

Service를 통해  
클러스터 외부에서 Pod에 접근합니다.

### 실습 내용

- Deployment를 NodePort Service로 노출
- 모든 노드에 특정 포트가 열리는 구조 확인
- 노드 IP와 NodePort를 통해 외부 접근 확인

### 확인 포인트

- NodePort 범위는 30000 ~ 32767
- 어느 노드로 접속해도 Service를 통해 Pod로 전달됨

---

## 실습 2 — ClusterIP Service

### 목적

클러스터 내부에서만 사용 가능한  
전용 네트워크 엔드포인트를 생성합니다.

### 실습 내용

- ClusterIP 타입 Service 생성
- 고정된 내부 IP 확인
- 외부에서는 직접 접근 불가함을 확인
- Pod 간 통신에 사용 가능함을 확인

---

## 실습 3 — LoadBalancer Service

### 목적

외부 로드밸런서를 통해  
Service를 외부에 노출하는 방식을 이해합니다.

### 실습 내용

- LoadBalancer 타입 Service 생성
- 외부 접근용 IP 할당 확인
- 일반 포트(80/443)로 접근 가능함을 확인

### 확인 포인트

- 클라우드 환경에서 주로 사용
- 내부적으로는 ClusterIP + NodePort 구조 포함

---

## Verification

아래 항목을 기준으로 Service 동작을 검증했습니다.

- Service 생성 및 조회 여부
- Pod와 Service의 라벨 매칭 여부
- 외부/내부 접근 정상 여부
- Pod 변경 시에도 접근 유지 여부

---

## Common Checks

실습 중 자주 사용한 명령들입니다.

- kubectl get service
- kubectl describe service <service_name>
- kubectl get endpoints
- kubectl get pod -o wide

---

## Troubleshooting / Pitfalls

- selector 라벨 불일치 시  
  → Service가 Pod를 찾지 못함
- NodePort는 모든 노드에 포트가 열림  
  → 보안 설정 주의 필요
- LoadBalancer는  
  → 환경에 따라 외부 IP 할당이 지연될 수 있음

---

## Files

- svc-nodeport.yaml  
  → NodePort Service 실습
- svc-clusterip.yaml  
  → ClusterIP Service 실습
- svc-loadbalancer.yaml  
  → LoadBalancer Service 실습