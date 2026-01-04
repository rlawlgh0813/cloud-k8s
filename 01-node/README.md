# 01 — Node Operations (Cordon / Drain / Taint)

쿠버네티스에서 노드 단위로 스케줄링을 제어하고, 노드 점검이나 장애 상황에서 파드를 안전하게 이동·제어하는 방법을 실습한 내용입니다.

---

## What I Practiced

- Cordon / Uncordon  
  → 특정 노드에 **새 Pod 배치를 막거나 해제**
- Drain  
  → 노드를 **비우면서(evict)** 동시에 스케줄링 차단
- Taint / Toleration  
  → 노드에 **출입 제한 조건**을 걸고, 조건을 만족하는 Pod만 허용

---

## Why It Matters (운영 관점)

- 노드 점검, 커널 업데이트, 재부팅 전에  
  → 새로운 Pod 유입을 차단
- 기존 Pod를 다른 노드로 이동시켜  
  → 서비스 중단 최소화
- 특정 워크로드를  
  → 원하는 노드에만 배치 가능

---

## Prerequisites

실습 전, 현재 클러스터의 노드 상태를 확인합니다.

kubectl get nodes -o wide

---

## 1) Cordon / Uncordon — 새 Pod 배치 막기

### 목적

특정 노드에 **새로운 Pod가 스케줄링되지 않도록** 설정합니다.  
기존 Pod는 그대로 유지됩니다.

### 절차

- 노드 확인  
  kubectl get nodes -o wide
- 스케줄링 차단  
  kubectl cordon <node_name>
- 상태 확인 (SchedulingDisabled 표시)  
  kubectl get nodes -o wide
- 차단 해제  
  kubectl uncordon <node_name>
- 상태 재확인  
  kubectl get nodes -o wide

### 포인트

- cordon은 **새 Pod만 막고**, 기존 Pod는 이동시키지 않습니다.

---

## 2) Drain — 노드 비우기 (점검 / 장애 대응)

### 목적

노드에서 실행 중인 Pod를 **다른 노드로 이동(evict)**시키고,  
해당 노드를 점검 상태로 만듭니다.

### 사용 명령

kubectl drain <node_name> --ignore-daemonsets --force --delete-emptydir-data

### 옵션 의미

- ignore-daemonsets  
  → DaemonSet Pod는 eviction 대상에서 제외
- delete-emptydir-data  
  → emptyDir는 임시 저장소이므로 drain 시 데이터 삭제 가능
- force  
  → 학습/실습 환경에서 강제 진행

### 확인

kubectl get pods -o wide  
→ Pod가 다른 노드에서 재생성되는지 확인

---

## 3) Taint / Toleration — 특정 조건 Pod만 허용

### 목적

노드를 기본적으로 **출입 금지 상태**로 만들고,  
특정 조건을 만족하는 Pod만 배치되도록 설정합니다.

### Taint 적용 (노드 제한)

kubectl taint nodes <node_name> key=value:NoSchedule

→ toleration이 없는 Pod는 해당 노드에 배치되지 않습니다.

### Taint 확인

kubectl describe node <node_name> | grep -i taint

### Toleration 추가

Deployment 또는 Pod spec에 toleration을 추가하여  
해당 노드 배치를 허용합니다.

### 결과 확인

kubectl get pods -o wide  
→ toleration 없는 Pod는 배치되지 않음  
→ toleration 있는 Pod만 배치됨

---

## Common Checks

실습 중 자주 사용한 명령들입니다.

- kubectl get nodes -o wide  
- kubectl get pods -o wide  
- kubectl describe node <node_name>  
- kubectl describe pod <pod_name>

---

## Troubleshooting / Pitfalls

- DaemonSet Pod는 기본적으로 drain 대상이 아님  
  → ignore-daemonsets 옵션 필요
- emptyDir는 drain 시 데이터가 사라질 수 있음  
  → 임시 데이터 용도로만 사용
- Taint만 걸면 Pod가 안 올라가는 게 정상  
  → 반드시 toleration을 함께 설정해야 함