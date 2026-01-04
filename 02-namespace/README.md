# 02 — Namespace

Namespace는 쿠버네티스 클러스터 내부 리소스를 논리적으로 분리하기 위한 단위입니다.  
이 파트에서는 단순 생성에 그치지 않고, 컨텍스트 전환과 네임스페이스 기반 작업 흐름을 중심으로 실습했습니다.

---

## What I Practiced

- Namespace 생성 및 조회
- kubectl context 기반 namespace 전환
- 특정 namespace 단위로 리소스 관리

---

## Why It Matters

- 하나의 클러스터에서 팀 / 프로젝트 / 환경(dev, test, prod)을 분리 가능
- 리소스 충돌 방지 및 관리 범위 명확화

---

## Core Concepts (간단 요약)

- 모든 리소스는 특정 namespace에 속함
- namespace를 지정하지 않으면 기본값은 `default`
- kubectl은 현재 context의 namespace를 기준으로 동작

---

## 1) Namespace 생성

### 목적
새로운 작업 공간을 만들고,  
리소스를 해당 공간 안에서만 관리하기 위함입니다.

### 실습 내용
- namespace 목록 조회
- 새로운 namespace 생성
- 생성 결과 확인

---

## 2) Context 기반 Namespace 전환

### 목적
매번 `-n <namespace>` 옵션을 붙이지 않고,  
현재 작업 namespace를 고정하기 위함입니다.

### 실습 내용
- 현재 context 확인
- context의 namespace 변경
- 이후 모든 kubectl 명령이 해당 namespace 기준으로 실행됨

---

## Verification

Namespace 단위로 정상 동작하는지 아래 항목을 확인했습니다.

- namespace 목록 조회
- 현재 context에 설정된 namespace 확인
- 다른 namespace의 리소스가 기본적으로 보이지 않는지 확인

---

## Common Checks

실습 중 자주 사용한 명령들입니다.

- kubectl get ns
- kubectl config get-contexts
- kubectl config view --minify

---

## Troubleshooting / Pitfalls

- namespace를 바꿨는데 리소스가 안 보일 때  
  → 현재 context의 namespace 확인 필요
- `default` namespace에서 작업하다가 다른 namespace를 의도치 않게 섞는 경우 주의
- admin context 사용 시, 잘못된 namespace에서 리소스를 삭제하지 않도록 주의

---

## Files

- namespace.yaml  
  → 실습에서 사용한 Namespace 정의 매니페스트
