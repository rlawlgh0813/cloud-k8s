# 12 — Ingress

Ingress는 쿠버네티스에서 HTTP 기반(L7 계층)의 트래픽을 제어하는 네트워크 리소스입니다.  
외부에서 들어오는 요청을 받아, 요청의 도메인(host)이나 경로(path)에 따라   적절한 Service로 라우팅하는 게이트웨이 역할을 합니다.

---

## Why Ingress

Service만 사용할 경우 다음과 같은 한계가 있습니다.

- 서비스마다 포트를 다르게 열어야 함
- 외부 노출 시 NodePort / LoadBalancer 의존
- URL 기반 라우팅 불가능

Ingress를 사용하면

- 하나의 엔드포인트로 여러 서비스 접근 가능
- 포트 1개(80/443)만 사용
- 도메인 / 경로 기반 트래픽 분기 가능

---

## Core Concepts

- Ingress는 Pod로 직접 트래픽을 보내지 않음
- 항상 **Service(보통 ClusterIP)** 를 대상으로 라우팅
- HTTP 요청 정보를 기준으로 라우팅 결정
- L7 계층에서 동작

트래픽 흐름은 다음과 같습니다.

외부 요청  
→ Ingress  
→ Service  
→ Pod

---

## Ingress 기본 구조

Ingress는 다음 요소로 구성됩니다.

- ingressClassName  
  → 사용할 Ingress Controller 지정
- rules  
  → host 또는 path 기준 라우팅 규칙
- backend  
  → 트래픽을 전달할 Service와 포트

---

## 실습 1 — 도메인 기반 접근

### 목적

Ingress를 통해  
특정 도메인으로 들어오는 요청을 Service로 전달합니다.

### 실습 내용

- Deployment 생성
- ClusterIP Service 생성
- 도메인 기반 Ingress 생성
- 브라우저에서 도메인으로 접근 확인

### 확인 포인트

- Ingress는 Pod가 아닌 Service로 트래픽 전달
- 외부에서는 도메인으로 접근

---

## 실습 2 — Path 기반 라우팅

### 목적

하나의 도메인에서  
URL 경로에 따라 서로 다른 Service로 트래픽을 분기합니다.

### 실습 내용

- 여러 Deployment와 Service 생성
- path 기준 Ingress 규칙 설정
- /, /hello, /httpd 경로별 라우팅 확인

### 확인 포인트

- 하나의 포트로 여러 서비스 제공 가능
- URL 경로에 따라 Service 분기

---

## 실습 3 — Host 기반 라우팅 (nip.io)

### 목적

도메인(host) 이름에 따라  
각기 다른 Service로 트래픽을 전달합니다.

### 실습 내용

- nip.io를 활용한 도메인 구성
- host 기반 Ingress 규칙 설정
- 서비스별 개별 도메인 접근 확인

### 확인 포인트

- 서비스마다 고유한 도메인 사용 가능
- 외부 DNS 설정 없이 실습 가능

---

## Verification

아래 항목을 기준으로 Ingress 동작을 검증했습니다.

- Ingress 생성 및 조회 여부
- host / path 규칙 정상 동작 여부
- Service를 통한 Pod 연결 여부
- 브라우저 접근 정상 여부

---

## Common Checks

실습 중 자주 사용한 명령들입니다.

- kubectl get ingress
- kubectl describe ingress <ingress_name>
- kubectl get service
- kubectl get pod -o wide

---

## Troubleshooting / Pitfalls

- Ingress Controller가 없으면 동작하지 않음
- Service가 ClusterIP가 아니면 구성 복잡해짐
- selector 또는 Service 이름 오타 시 트래픽 전달 실패
- rewrite-target 설정 누락 시 경로 문제 발생 가능

---

## Files

- ing-domain.md
  → 도메인 기반 Ingress
- ing-path.yaml  
  → 경로 기반 Ingress
- ing-host.yaml  
  → host 기반 Ingress (nip.io)