# 05 — StatefulSet

StatefulSet은 쿠버네티스에서 상태(state)를 가지는 애플리케이션을  
안정적으로 배포하고 관리하기 위한 컨트롤러 리소스입니다.
Deployment와 달리, 각 Pod가 고유한 정체성과 순서를 보장받는 것이 핵심 특징입니다.

---

## Why StatefulSet

Deployment는 다음과 같은 특성을 가집니다.

- Pod는 서로 구분되지 않음
- Pod 이름이 매번 바뀔 수 있음
- Pod 삭제 시 새 Pod는 완전히 새 인스턴스

하지만 다음과 같은 서비스에는 적합하지 않습니다.

- 데이터베이스
- 캐시 서버
- 메시지 큐
- 상태를 유지해야 하는 서비스

이를 위해 StatefulSet이 사용됩니다.

---

## Core Characteristics

StatefulSet의 주요 특징은 다음과 같습니다.

- Pod 이름이 고정됨  
  (예: app-0, app-1, app-2)
- Pod 생성 및 삭제 순서 보장
- 각 Pod는 고유한 네트워크 ID를 가짐
- Pod가 재생성되어도 정체성 유지

---

## Deployment와의 차이점

- Deployment  
  → 무상태(stateless) 서비스에 적합  
  → Pod는 교체 가능

- StatefulSet  
  → 상태(stateful) 서비스에 적합  
  → Pod는 고유한 개체로 관리

---

## 실습 0 — StatefulSet 조회

### 목적

StatefulSet 리소스의 상태와 구성 정보를 확인합니다.

### 실습 내용

- StatefulSet 목록 조회
- StatefulSet 상세 정보 확인
- 생성된 Pod 이름 및 순서 확인

---

## 실습 1 — nginx StatefulSet 배포

### 목적

StatefulSet을 이용해 nginx Pod를 배포하고,  
Pod의 **고정된 이름과 순차적 생성**을 확인합니다.

### 실습 내용

- nginx 이미지를 사용하는 StatefulSet 생성
- replicas 수에 따라 Pod가 순차적으로 생성됨을 확인
- Pod 이름이 고정되는 구조 확인
- Pod 삭제 후 재생성 시 동일한 이름 유지 확인

---

## Update Strategy

이번 실습에서는 updateStrategy를 다음과 같이 설정했습니다.

- OnDelete  
  → StatefulSet 템플릿을 수정해도  
    기존 Pod는 자동으로 교체되지 않음  
  → Pod를 직접 삭제해야 새로운 설정이 반영됨

이를 통해 Deployment와 다른  
**보수적인 업데이트 방식**을 확인했습니다.

---

## Verification

아래 항목을 기준으로 StatefulSet 동작을 검증했습니다.

- Pod 이름이 순차적으로 생성되는지 확인
- Pod 삭제 후 동일한 이름으로 재생성되는지 확인
- replicas 변경 시 생성/삭제 순서 확인
- Deployment와의 동작 차이 이해

---

## Common Checks

실습 중 자주 사용한 명령들입니다.

- kubectl get sts
- kubectl describe sts <statefulset_name>
- kubectl get pod -o wide

---

## Troubleshooting / Pitfalls

- StatefulSet은 보통 Headless Service가 필요함
- Pod를 임의로 삭제해도  
  StatefulSet이 동일한 이름으로 복구함
- updateStrategy를 이해하지 않으면  
  업데이트 동작이 예상과 다를 수 있음

---

## Files

- sts.yaml  
  → nginx StatefulSet 기본 실습