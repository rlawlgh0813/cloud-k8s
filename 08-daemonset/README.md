# 08 — DaemonSet

DaemonSet은 쿠버네티스에서 **클러스터의 모든 노드마다 Pod를 1개씩 항상 실행**하도록 보장하는 컨트롤러 리소스입니다.  
노드 단위로 반드시 실행되어야 하는 에이전트형 워크로드에 사용됩니다.

---

## Why DaemonSet

다음과 같은 경우에는 Deployment보다 DaemonSet이 적합합니다.

- 각 노드마다 반드시 실행되어야 하는 서비스
- 로그 수집 에이전트
- 모니터링 에이전트
- 노드 단위 스토리지 또는 네트워크 컴포넌트

DaemonSet은  
**노드 수 = Pod 수**를 자동으로 유지합니다.

---

## Core Concepts

- DaemonSet은 노드마다 Pod 1개를 보장
- 노드가 추가되면 자동으로 Pod 생성
- 노드가 제거되면 해당 Pod도 함께 제거
- Pod를 직접 삭제해도 DaemonSet이 즉시 재생성

---

## 실습 0 — DaemonSet 생성 및 확인

### 목적

DaemonSet의 기본 동작과  
노드별 Pod 배치 구조를 확인합니다.

### 실습 내용

- DaemonSet 목록 조회
- DaemonSet 상세 정보 확인
- 각 노드에 Pod가 하나씩 실행 중인지 확인

---

## 실습 1 — DaemonSet 생성 (로그 수집기 예제)

### 목적

노드마다 실행되는 로그 수집 에이전트를  
DaemonSet으로 배포합니다.

### 실습 내용

- fluentd 이미지를 사용하는 DaemonSet 생성
- kube-system 네임스페이스에 배포
- 각 노드마다 Pod가 자동 생성되는지 확인
- Pod 삭제 시 자동으로 다시 생성되는지 확인

---

## 주요 설정 포인트

### tolerations

- control-plane 노드에는 기본적으로 NoSchedule taint가 적용됨
- DaemonSet Pod에 toleration을 추가하여
  → control-plane 노드에도 배치 가능하도록 설정

### hostPath volume

- 노드의 /var/log 디렉토리를
  컨테이너의 /var/log 경로로 마운트
- 각 노드의 로그를 직접 수집 가능

---

## Verification

아래 항목을 기준으로 DaemonSet 동작을 검증했습니다.

- 모든 노드에 Pod가 1개씩 존재하는지 확인
- Pod 삭제 후 자동 재생성 여부
- 새로운 노드 추가 시 Pod 자동 생성 여부
- control-plane 노드에도 Pod가 배치되는지 확인

---

## Common Checks

실습 중 자주 사용한 명령들입니다.

- kubectl get ds
- kubectl describe ds <daemonset_name>
- kubectl get pod -o wide
- kubectl delete pod <daemonset_pod_name>

---

## Troubleshooting / Pitfalls

- tolerations가 없으면
  → control-plane 노드에 Pod가 배치되지 않음
- hostPath 사용 시
  → 노드 파일 시스템 접근에 주의 필요
- DaemonSet Pod는
  → 수동 삭제해도 항상 복구됨

---

## Files

- daemonset.yaml  
  → fluentd 기반 DaemonSet 실습