# 🏗️ Architecting a Simple but Resilient App on AWS  
### ECS • Multi-AZ Deployment • Microservice Architecture

---

<p align="center">
  <img width="400" height="591" alt="Diag-1 0" src="https://github.com/user-attachments/assets/50c7a9b9-a789-4776-a1ca-decc12ced688" />
</p>

---

## 🌐 Route 53  

**Amazon Route 53** is designed for *extremely high availability* and *low-latency DNS resolution*.  
It is a **global, distributed service** that leverages an **Anycast network**, ensuring automatic failover and resilient routing across regions.

---

## ⚖️ ALB – Web Tier  

Route 53 routes incoming HTTPS traffic to the **Application Load Balancer (ALB)** for the web tier.  
The ALB terminates SSL/TLS connections and distributes traffic to **ECS tasks** running the Apache web server.

---

## 🐳 ECS – Web Service  

The **ECS Web Service** runs **two Apache 2.4 task replicas** across multiple **Availability Zones (AZs)**.  
This provides *high availability* and *resilience*.  
Each task handles requests and forwards them to the **UI ALB** for backend communication.

---

## ⚖️ ALB – UI Tier  

The **UI Application Load Balancer** acts as the entry point for the **backend UI microservice**.  
It intelligently balances traffic across ECS containers hosting the front-end application.

---

## 🐳 ECS – UI Service  

This ECS cluster hosts the **React-based UI**.  
It runs **two container replicas**, each deployed in different Availability Zones.  
This ensures *fault tolerance*, *scalability*, and *continuous availability* of the user interface.

---

<p align="center">
  <em>✅ Designed for reliability, scalability, and seamless microservice communication on AWS.</em>
</p>
