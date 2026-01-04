# 07 — CronJob

CronJob은 쿠버네티스에서 정해진 시간 스케줄에 따라 Job을 주기적으로 실행하는 리소스입니다.  
Job이 “한 번 실행되는 작업”이라면, CronJob은 반복 실행되는 작업을 담당합니다.

---

## Why CronJob

다음과 같은 작업은 CronJob에 적합합니다.

- 주기적인 데이터 백업
- 로그 정리
- 상태 점검 스크립트
- 정기 배치 작업

CronJob은  
**스케줄 관리 + Job 생성 + 실행 결과 관리**를 한 번에 제공합니다.

---

## Core Concepts

- CronJob  
  → 스케줄 관리자
- Job  
  → CronJob이 주기적으로 생성
- Pod  
  → 실제 작업 실행 후 종료

실행 흐름은 다음과 같습니다.

CronJob  
→ (스케줄 시점) Job 생성  
→ Job이 Pod 생성  
→ Pod 작업 수행 후 종료

---

## 실습 0 — CronJob 생성 및 동작 확인

### 목적

CronJob의 기본 생성과  
주기적으로 Job이 생성되는 흐름을 확인합니다.

### 실습 내용

- CronJob 생성
- 설정한 스케줄에 따라 Job 생성 확인
- 생성된 Pod가 실행 후 종료되는지 확인

---

## 실습 1 — 주기적 명령 실행 (date)

### 목적

정해진 시간마다  
간단한 명령이 실행되는 구조를 이해합니다.

### 실습 내용

- 일정 주기로 date 명령 실행
- Pod 로그를 통해 실행 시각 확인
- 매 주기마다 새로운 Job이 생성됨을 확인

---

## 실습 2 — 실행 시간 조절 (sleep)

### 목적

CronJob으로 실행되는 작업의  
실행 시간을 제어하는 방식을 확인합니다.

### 실습 내용

- sleep 명령을 포함한 CronJob 생성
- 작업이 일정 시간 유지된 후 종료됨을 확인
- 다음 스케줄과의 관계 이해

---

## 실습 3 — YAML 기반 CronJob 정의

### 목적

CLI가 아닌 YAML을 통해  
CronJob을 선언적으로 정의합니다.

### 실습 내용

- CronJob 매니페스트 작성
- jobTemplate 구조 이해
- Pod에 실행될 컨테이너 및 명령 정의

---

## 실습 4 — 실행 기록 관리 (History Limit)

### 목적

CronJob 실행 기록이  
무한히 쌓이지 않도록 관리하는 방법을 이해합니다.

### 실습 내용

- successfulJobsHistoryLimit 설정
- failedJobsHistoryLimit 설정
- 오래된 Job이 자동으로 정리되는지 확인

---

## Verification

아래 항목을 기준으로 CronJob 동작을 검증했습니다.

- CronJob 상태 확인
- 스케줄에 따른 Job 생성 여부
- Job이 생성한 Pod 실행 및 종료 여부
- Pod 로그를 통한 작업 결과 확인
- Job 기록 자동 정리 여부

---

## Common Checks

실습 중 자주 사용한 명령들입니다.

- kubectl get cronjob
- kubectl get job
- kubectl get pod
- kubectl logs <pod_name>

---

## Troubleshooting / Pitfalls

- 스케줄 표현식(cron 형식) 오류 시  
  → Job이 생성되지 않음
- 작업 실행 시간이  
  스케줄 주기보다 길 경우  
  → Job이 겹쳐 실행될 수 있음
- History limit 설정이 없으면  
  → Job 기록이 계속 누적됨

---

## Files

- cronjob-history.yaml  
  → 실행 기록 제한 설정