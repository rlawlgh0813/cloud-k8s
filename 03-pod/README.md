# 03 — Pod

Pod는 쿠버네티스에서 컨테이너를 실행하는 가장 기본적인 배포 단위입니다.  
이 파트에서는 환경변수, 다중 컨테이너, 로그 흐름, 서비스 노출까지 직접 실습했습니다.

---

## What I Practiced

- 단일 Pod 생성 및 조회
- NodePort를 통한 외부 접근
- Pod 내부 환경변수 설정 및 로그 확인
- 하나의 Pod에 여러 컨테이너 배치

---

## Why It Matters

- 모든 상위 리소스(Deployment, Job 등)는 **결국 Pod를 생성**
- Pod 단위로 다음이 이루어짐
  - 스케줄링
  - 네트워킹
  - 볼륨
  - 로그 수집

---

## Core Concepts (간단 요약)

- Pod는 1개 이상의 컨테이너 집합
- Pod 내부 컨테이너들은
  - 동일한 IP
  - 동일한 네트워크 네임스페이스
  - 볼륨 공유
- Pod는 기본적으로 일시적(Ephemeral)

---

## 1) 단일 Pod 실행 (nginx)

### 목적
가장 기본적인 Pod 생성과 상태 확인 흐름을 익힙니다.

### 실습 내용
- nginx 이미지를 사용하는 Pod 생성
- Pod 상태 및 상세 정보 확인
- 정상 실행 여부 검증

---

## 2) NodePort를 통한 외부 접근

### 목적
Pod를 외부에서 접근 가능하도록 노출하는 과정을 이해합니다.

### 실습 내용
- Pod 기반 Service(NodePort) 생성
- Node IP와 NodePort를 통한 접근
- Service와 Pod의 관계 확인

---

## 3) 환경변수 설정 및 로그 확인

### 목적
컨테이너 실행 시 환경변수를 주입하고, Pod 로그가 어떻게 수집되는지 확인합니다.

### 실습 내용
- Pod spec에 환경변수 설정
- 컨테이너에서 환경변수 출력
- kubectl logs로 로그 확인

### 확인 포인트
- 컨테이너 표준 출력 → 노드 런타임 로그 → kubelet → kubectl logs
- 로그는 Pod 단위로 관리됨

---

## 4) 다중 컨테이너 Pod

### 목적
하나의 Pod 안에 여러 컨테이너를 배치하는 구조를 이해합니다.

### 실습 내용
- nginx, redis, memcached 등 여러 컨테이너 배치
- Pod 내부 컨테이너 선택 실행(exec -c)
- 컨테이너 간 네트워크 공유 확인

---

## 5) Scale Up / Down (Pod 관점에서의 한계)

### 목적
파드 개수를 늘리거나 줄이는 과정을 직접 경험하고,  
Pod 자체는 scale 대상이 아니라는 점을 이해하는 것이 목표입니다.

### 실습 내용
- ReplicaSet 또는 Deployment를 통해 Pod 개수 조절
- scale up 시 새로운 Pod 생성 확인
- scale down 시 Pod 삭제 확인

### 핵심 관찰 포인트
- Pod는 단일 실행 단위로, 직접 scale up / down 할 수 없음
- `kubectl scale`은 Pod가 아닌 Deployment / ReplicaSet 같은 컨트롤러 리소스를 대상으로 동작
  → 컨트롤러가 원하는 개수를 유지하기 위해 Pod를 생성/삭제함

---

## Verification

아래 항목을 통해 Pod 동작을 검증했습니다.

- Pod 상태 조회 (Running / Pending)
- Pod 상세 정보 확인
- 로그 출력 정상 여부
- 외부 접근(NodePort) 확인

---

## Common Checks

실습 중 자주 사용한 명령들입니다.

- kubectl get pod
- kubectl describe pod <pod_name>
- kubectl logs <pod_name>
- kubectl exec -it <pod_name> -- sh

---

## Troubleshooting / Pitfalls

- Pod가 Pending 상태로 멈출 때  
  → 노드 자원 부족 또는 스케줄링 이슈 확인
- 다중 컨테이너 Pod에서 exec 시  
  → 반드시 컨테이너 지정 필요
- Pod는 직접 업데이트하기보다  
  → 새로 생성하는 것이 일반적

---

## Files

- pod-env.yaml  
  → 환경변수 설정 Pod
- pod-multi-container.yaml  
  → 다중 컨테이너 Pod
- replicaset.yaml
  → pod scale 확인