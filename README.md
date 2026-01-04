# cloud-k8s

본 레포지토리는 COOP-클라우드트랙 과목**에서 진행한 쿠버네티스 기반 클라우드 이론 및 실습 내용을 정리한 포트폴리오용 저장소입니다.

---

## About This Repository

이 레포지토리는 다음을 목표로 합니다.

- 쿠버네티스 핵심 리소스의 역할과 동작 원리 이해
- CLI 및 YAML 기반 리소스 생성 경험
- 서비스 운영 관점에서의 리소스 사용 흐름 정리

모든 내용은 실습 단위별로 README와 YAML을 함께 정리했습니다.

---

## Environment

- Kubernetes Cluster (Multi-node)
- Control Plane + Worker Nodes
- kubectl 기반 실습
- Nginx Ingress Controller
- NFS 기반 스토리지 환경

---

## Repository Structure

각 폴더는 하나의 쿠버네티스 리소스를 중심으로 구성됩니다.

- README.md  
  → 해당 리소스의 개념 요약 및 실습 흐름 설명
- YAML files  
  → 실제 실습에 사용한 매니페스트

---

## Contents Overview

### Core Concepts & Basics

- 01-node  
  → 노드 단위 스케줄링 제어 (Cordon / Drain / Taint)
- 02-namespace  
  → 네임스페이스 기반 리소스 분리
- 03-pod  
  → Pod 생성, 환경변수, 다중 컨테이너, Pod의 한계

### Controllers

- 04-deployment  
  → Pod 자동 복구, 스케일링, 롤링 업데이트, 롤백
- 05-statefulset  
  → 상태를 가지는 워크로드 관리
- 06-job  
  → 단발성 작업 실행
- 07-cronjob  
  → 주기적 작업 스케줄링
- 08-daemonset  
  → 노드 단위 에이전트 워크로드

### Configuration Management

- 09-configmap  
  → 설정 정보 분리 및 관리
- 10-secret  
  → 민감 정보 관리 (환경변수 / 파일)

### Networking

- 11-service  
  → Pod 네트워크 추상화 (ClusterIP / NodePort / LoadBalancer)
- 12-ingress  
  → HTTP 기반 라우팅 (Domain / Path / Host)

### Storage

- 13-storage  
  → Volume, PV, PVC, 정적/동적 프로비저닝

---

## What I Learned

이 레포지토리를 통해 다음을 직접 경험했습니다.

- Pod는 실행 단위이며, 실제 운영은 컨트롤러가 담당
- Service와 Ingress를 통한 네트워크 추상화
- ConfigMap과 Secret을 통한 설정 분리
- 상태 저장을 위한 스토리지 구조
- 쿠버네티스 리소스 간의 역할 분리와 책임 구조