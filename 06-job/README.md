# 06 — Job

Job은 쿠버네티스에서 정해진 작업을 수행하고 종료하는 워크로드 리소스입니다.  
Deployment처럼 계속 실행되는 서비스가 아니라, 작업의 완료(success)가 목표인 리소스입니다.

---

## Why Job

다음과 같은 작업은 Deployment에 적합하지 않습니다.

- 배치 작업
- 초기화 스크립트
- 데이터 처리
- 단발성 명령 실행

이러한 경우 Job을 사용해  
**작업의 성공 여부를 쿠버네티스가 관리**하도록 합니다.

---

## Core Concepts

- Job은 하나 이상의 Pod를 생성
- Pod가 작업을 완료하면 Job은 성공 상태로 종료
- 실패 시 설정에 따라 재시도 가능
- Job = 지시자, Pod = 실행자

---

## 실습 0 — Job 생성 및 확인 흐름

### 목적

Job의 기본 동작 흐름을 이해합니다.

### 실습 내용

- Job 생성
- Job 상태 확인
- Job이 생성한 Pod 확인
- Pod 로그를 통해 작업 결과 확인

---

## 실습 1 — 단순 Job 실행 (sleep)

### 목적

일정 시간 동안 실행된 후 종료되는  
가장 단순한 Job을 실습합니다.

### 실습 내용

- sleep 명령을 실행하는 Job 생성
- 일정 시간 후 Pod가 Completed 상태로 종료됨을 확인
- Job이 성공 상태로 전환되는 흐름 확인

---

## 실습 2 — 출력 Job 실행 (echo)

### 목적

Job 실행 결과가  
Pod 로그로 남는다는 점을 확인합니다.

### 실습 내용

- echo 명령을 실행하는 Job 생성
- kubectl logs를 통해 출력 결과 확인
- Job 완료 후에도 로그 조회 가능함을 확인

---

## 실습 3 — YAML 기반 Job 정의

### 목적

CLI가 아닌 YAML을 통해  
Job을 선언적으로 정의하는 방식을 실습합니다.

### 실습 내용

- Job 매니페스트 작성
- 컨테이너에서 실행할 명령 정의
- Job 생성 후 정상 완료 여부 확인

---

## 실습 4 — 재시도 정책 (restartPolicy / backoffLimit)

### 목적

작업 실패 시  
재시도 동작을 제어하는 방식을 이해합니다.

### 실습 내용

- restartPolicy 설정
- 실패 시 재시도 여부 확인
- backoffLimit을 통한 최대 재시도 횟수 제한

---

## 실습 5 — 다중 Job 실행 (completions)

### 목적

하나의 Job으로  
여러 번의 작업 성공을 요구하는 구조를 이해합니다.

### 실습 내용

- completions 설정
- 지정된 횟수만큼 Job이 성공해야 종료됨을 확인

---

## 실습 6 — 병렬 Job 실행 (parallelism)

### 목적

여러 작업을  
동시에 병렬로 실행하는 방식을 실습합니다.

### 실습 내용

- parallelism 설정
- 여러 Pod가 동시에 실행되는 구조 확인
- 병렬 처리 흐름 이해

---

## Verification

아래 항목을 기준으로 Job 동작을 검증했습니다.

- Job 상태 (Active / Succeeded / Failed)
- Job이 생성한 Pod 상태
- Pod 로그 출력 결과
- 재시도 및 완료 조건 충족 여부

---

## Common Checks

실습 중 자주 사용한 명령들입니다.

- kubectl get job
- kubectl describe job <job_name>
- kubectl get pod
- kubectl logs <pod_name>

---

## Troubleshooting / Pitfalls

- Job Pod는 완료 후 종료됨  
  → 실행 중 exec 접근 불가한 경우 있음
- restartPolicy 설정에 따라  
  실패 시 동작이 달라짐
- Job 삭제 시  
  생성된 Pod도 함께 정리됨

---

## Files

- job.yaml  
  → 기본 Job 실행
- job-retry.yaml  
  → 재시도 정책 실습
- job-multi.yaml  
  → completions / parallelism 실습