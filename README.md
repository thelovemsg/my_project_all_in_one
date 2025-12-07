# 👑 My Project - All In One! - All Play Ground 
### (Any themes come to my mind)

## 🎯 Project Goal

Recently I found out I just focus on reading books, official documentation and AI-generated codes.
Of course, it doesn't mean it's entirely bad, but it's less than ideal for me.

I need to be a good engineer, which means I should focus on solving real-world problems.
Furthermore, I can build up my practical coding environment(actually, a repository) as as playground for applying what I've learned.
I finally realized that the most important thing in any fields to be good at doing something is to use what i've learned.

So, The project aims to systematically solve the most challenging backend issues (concurrency, data integrity) encountered in a high-volume traffic environment and validate their performance.

---

# 🎬 First Episode: High-Concurrency Event System

## 1. 📖 Scenario & Problem Definition

**[Scenario]**
* **High-Concurrency Event:** Simulating a scenario where **10,000 users** send simultaneous requests to claim **100 limited-edition coupons**.
* **The Challenge:** To achieve **high throughput (TPS)** without compromising **data integrity**, overcoming the limitations of traditional DB locking mechanisms.

## 2. ⚙️ System Design & Tech Stack

### 2.1. Database Design (Minimizing Lock Contention)
* **Entities:** `Event` (Quantity limits, Versioning), `CouponIssue` (History).
* **Principle:** Focused on minimizing DB lock contention. Instead of locking a single DB row for every request, I utilize **Redis** for counting to handle high concurrency efficiently.

### 2.2. Frontend (Minimalist)
* **Goal:** A minimal UI strictly for backend testing (using Thymeleaf or Vanilla JS).
* **Features:** Coupon event list, issue button, and a **Kafka-based queue position display**. (Excluding complex SPA frameworks to focus on backend logic).

---

## 3. 💻 Backend Architecture & Concurrency

### 3.1. Multi-Module & TDD/DDD
* **Structure:** A physical separation of **DDD layers** using a multi-module architecture (`domain`, `infrastructure`, `api`) to enforce design principles.
* **Methodology:** Adopting **TDD (Test-Driven Development)** to prioritize and verify the consistency of domain logic.

### 3.2. Evolution of Implementation (v1, v2, v3)
* **v1 (DB Lock):** Implemented **JPA Pessimistic Lock** (`PESSIMISTIC_WRITE`). Verified TPS degradation due to lock contention.
* **v2 (Redis Atomic):** Significantly improved TPS by using **Redis `INCR`** operations for atomic counting.
* **v3 (Kafka Queue):** Established an **asynchronous processing system** using **Kafka** to manage waiting queues and ensure system stability under heavy load.

---

## 4. 📊 Deployment & Performance Verification

**[Deployment]**
* **Infrastructure:** Using **Docker Compose** to deploy the entire stack—Spring App, DB, Redis, Kafka, and **Prometheus/Grafana**—in a single command.

**[Verification]**
* **Load Testing:** Using **JMeter** to apply load to each API version (v1 vs. v3).
* **Monitoring:** visualizing **TPS and Latency** changes in real-time via **Grafana** dashboards to quantitatively prove performance improvements.
