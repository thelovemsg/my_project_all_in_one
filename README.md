# 👑 My Project - All In One! - All Play Ground (Any themes come to my mind)

## 🎯 Project Goal

Recently I found out I just focus on reading books, official documentation and AI-generated codes.
Of course, it doesn't mean it's entirely bad, but it's less than ideal for me.

I need to be a good engineer, which means I should focus on solving real-world problems.
Furthermore, I can build up my practical coding environment(actually, a repository) as as playground for applying what I've learned.
I finally realized that the most important thing in any fields to be good at doing something is to use what i've learned.

So, The project aims to systematically solve the most challenging backend issues (concurrency, data integrity) encountered in a high-volume traffic environment and validate their performance.

---

## 1. 📖 상황 및 문제 정의

**[상황 설명]**

* **시나리오:** 100개 한정 쿠폰에 대해 10,000명의 유저가 동시에 발급 요청을 보내는 **초고부하 이벤트**를 가정합니다.
* **해결 과제:** 일반적인 DB Lock 방식의 한계를 극복하고, **데이터 정합성**을 훼손하지 않으면서 **고성능(TPS)**을 달성해야 합니다.

## 2. ⚙️ 시스템 설계 및 기술 선택

### 2.1. DB 설계 (락 경합 최소화)

* **엔티티:** `Event` (제한 수량, 버전 관리), `CouponIssue` (발급 이력).
* **원칙:** 모든 동시 요청이 DB의 단일 Row를 점유하지 않도록, **Redis**를 카운팅에 활용하여 DB 락 경합을 최소화하는 설계에 중점을 둡니다.

### 2.2. 프론트엔드 개발 (극도 단순화)

* **목표:** 백엔드 테스트를 위한 **최소한의 UI**만 구성합니다. (Thymeleaf 또는 Vanilla JS)
* **기능:** 쿠폰 이벤트 목록, 발급 버튼, 그리고 **Kafka 대기열 기반의 순번 상태 표시** 기능만 구현합니다. (복잡한 SPA 프레임워크 사용 배제)

---

## 3. 💻 백엔드 개발 및 동시성 구현

### 3.1. 멀티 모듈 및 TDD/DDD

* **구조:** 멀티 모듈(`domain`, `infrastructure`, `api`) 기반으로 **DDD 레이어**를 물리적으로 분리하여 설계 원칙을 강제합니다.
* **개발 방식:** **TDD(Test-Driven Development)**를 통해 도메인 로직의 정합성을 최우선으로 검증합니다.

### 3.2. 핵심 구현 단계 (v1, v2, v3)

* **v1 (DB 락):** **JPA 비관적 락** (`PESSIMISTIC_WRITE`) 구현 및 저하된 TPS 확인.
* **v2 (Redis Atomic):** **Redis `INCR`** 명령어를 사용한 카운팅 구현으로 TPS를 대폭 개선.
* **v3 (Kafka 대기열):** **Kafka**를 이용한 **비동기 처리 시스템**을 구축하여 대기 순번 및 시스템 안정성 확보.

---

## 4. 📊 배포 및 성능 검증

**[배포]**

* **환경 구축:** Docker Compose를 사용하여 Spring App, DB, Redis, Kafka, **Prometheus/Grafana**까지 모든 인프라를 한 번에 배포할 수 있도록 구성합니다.

* **검증:** **JMeter**로 각 버전(v1 vs v3)의 API에 부하를 가하고, **Grafana** 대시보드를 통해 **TPS, Latency** 변화를 실시간으로 모니터링하여 성능 개선 효과를 수치로 증명합니다.
