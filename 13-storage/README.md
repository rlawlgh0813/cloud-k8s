# 13 — Storage (Volume / PV / PVC)

쿠버네티스의 Storage는 Pod의 생명주기와 분리된 데이터 저장을 가능하게 하는 핵심 기능입니다.  

이 파트에서는  
- Pod 내부 볼륨 사용
- 노드 디스크 활용
- Persistent Volume / Persistent Volume Claim
- 정적 / 동적 프로비저닝  
까지의 흐름을 단계적으로 실습했습니다.

---

## Why Storage Matters

Pod는 기본적으로 **휘발성(Ephemeral)** 입니다.

- Pod 삭제 시 내부 데이터 소멸
- 다른 노드로 이동 시 데이터 유지 불가

Storage 리소스를 사용하면

- Pod가 삭제되어도 데이터 유지
- Pod 재생성 시 동일한 데이터 접근
- 상태가 있는 애플리케이션 운영 가능

---

## Core Concepts

### Volume

- Pod 내부에서 사용하는 저장 공간
- Pod 생명주기와 함께 생성·삭제됨

### Persistent Volume (PV)

- 클러스터가 미리 확보한 실제 스토리지 리소스
- Pod와 독립적인 생명주기

### Persistent Volume Claim (PVC)

- 사용자가 원하는 스토리지를 요청하는 객체
- PVC가 PV에 바인딩됨

연결 구조는 다음과 같습니다.

Pod  
→ PVC  
→ PV  
→ 실제 스토리지

---

## 실습 1 — Pod Volume (emptyDir)

### 목적

Pod 내부에서 컨테이너 간 데이터를 공유하는  
가장 기본적인 볼륨 사용 방식을 실습합니다.

### 실습 내용

- emptyDir 볼륨 생성
- 하나의 Pod 내 여러 컨테이너에서 볼륨 공유
- 로그 데이터 공유 확인

### 특징

- Pod가 살아있는 동안만 유지
- Pod 삭제 시 데이터 소멸

---

## 실습 2 — hostPath Volume

### 목적

노드의 디스크를  
Pod에 직접 마운트하는 방식을 실습합니다.

### 실습 내용

- hostPath 볼륨 생성
- 노드 디렉토리를 컨테이너에 마운트
- Pod 삭제 후에도 노드에 데이터 유지 확인

### 주의점

- Pod가 다른 노드로 이동하면 데이터 접근 불가
- 테스트 또는 단일 노드 환경에 적합

---

## 실습 3 — 정적 프로비저닝 (Local PV)

### 목적

관리자가 미리 준비한 스토리지를  
PV로 등록하고 PVC로 사용하는 구조를 이해합니다.

### 실습 내용

- 노드별 로컬 디렉토리 생성
- PersistentVolume 수동 생성
- PVC를 통해 PV 바인딩
- Pod에서 PVC 사용

### 핵심 포인트

- provisioner 없음
- PV를 직접 관리
- nodeAffinity로 노드 지정

---

## 실습 4 — 정적 프로비저닝 (NFS)

### 목적

여러 노드에서 동시에 접근 가능한  
공유 스토리지를 사용하는 방식을 실습합니다.

### 실습 내용

- NFS 기반 PersistentVolume 생성
- ReadWriteMany 접근 모드 사용
- 여러 Pod에서 동일 데이터 접근 확인

### 핵심 포인트

- Pod 위치와 무관하게 동일 데이터 사용 가능
- 상태 저장 서비스에 적합

---

## 실습 5 — 동적 프로비저닝 (NFS Provisioner)

### 목적

PVC 생성 시  
PV가 자동으로 생성되는 구조를 이해합니다.

### 실습 내용

- StorageClass 정의
- NFS Subdir External Provisioner 설치
- PVC 생성 시 PV 자동 생성 확인
- Pod에서 동적 PV 사용

### 핵심 포인트

- PVC 중심의 스토리지 관리
- 운영 환경에서 가장 일반적인 방식

---

## Verification

아래 항목을 기준으로 Storage 동작을 검증했습니다.

- Pod 재생성 후 데이터 유지 여부
- PVC와 PV 바인딩 상태
- 접근 모드에 따른 동작 차이
- 노드 변경 시 데이터 접근 여부

---

## Common Checks

실습 중 자주 사용한 명령들입니다.

- kubectl get pv
- kubectl get pvc
- kubectl describe pv <pv_name>
- kubectl describe pvc <pvc_name>
- kubectl exec -it <pod_name> -- sh

---

## Troubleshooting / Pitfalls

- PVC가 Pending 상태일 경우  
  → StorageClass 또는 PV 설정 확인
- 접근 모드 불일치 시  
  → 바인딩 실패
- hostPath는 운영 환경에 부적합
- 동적 프로비저닝은 Provisioner 동작 여부 확인 필요

---

## Files

- pod-volume.yaml  
  → emptyDir / hostPath 볼륨 실습
- pv-local.yaml  
  → 로컬 정적 PV
- pvc.yaml  
  → PVC 정의
- pv-nfs.yaml  
  → NFS 기반 정적 PV
- storageclass.yaml  
  → StorageClass 정의