

<img width="800" height="701" alt="image" src="https://github.com/user-attachments/assets/f32ddb1e-44a5-4182-b335-13c31d0fd2b3" />





# Full Stack Document 

---

### Author Information

| **Author**   | **Created on** | **Version** | **Last updated by** | **Last edited on** | **Level** | **Reviewer**  |
|--------------|----------------|-------------|---------------------|--------------------|-----------|---------------|
| Ishaan    | 6-08-25    | v1.0  |  Ishaan  |8-08-25   | Internal    | Rohit Chopra    | 

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Purpose](#2-Purpose) 
3. [Pre-requisites ](#3-Pre-requisites)
4. [ System Requirements](#4-System-Requirements)
5. [Dependencies](#5-Dependencies)  
6. [Ports](#6-Ports)
7. [Architecture](#7-Architecture)  
8. [API Setup and Execution](#8-API-Setup-and-Execution)  
9. [Troubleshooting](#9-Troubleshooting)
10. [FAQs](#10-faqs)
11. [Conclusion](#11-conclusion)  
12. [Contact Information](#12-contact-information) 
13. [References](#13-References)

---

# 1. Introduction
This document provides a comprehensive overview of the Full Stack Microservices Architecture built under the OT-Microservices ecosystem. 


---

# 2. Purpose

A full-stack application is a unified software solution that includes both the frontend (user interface and interactions) and the backend (server-side logic, databases, and processing). It provides an end-to-end system capable of handling all aspects of functionality, from user experience to data management.

This application streamlines employee operations by managing employee data, tracking attendance, and processing salaries using a modular microservices architecture. It uses Redis for caching, PostgreSQL and ScyllaDB for storage, and a ReactJS frontend for user interaction — enabling scalable, efficient, and maintainable service delivery.

---

# 3. Pre-requisites

The Full Stack API application has dependencies on other REST APIs from **[OT-Microservices](https://github.com/OT-MICROSERVICES)**. To run the application successfully, ensure the following APIs are configured and running:

* **[Employee API](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/SCRUM-73-kawalpreet/OT-Microservices/Applications/Employee-API/Introduction/README.md)**
* **[Attendance API](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/SCRUM-71-mansoor/OT-Microservices/Applications/Attendance-API/Introduction/README.md)**
* **[Salary API](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/SCRUM-75-mansoor/OT-Microservices/Applications/Salary-API/Introduction/README.md)**
* **[Frontend API](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/scrum-77-sunny/OT-Microservices/Frontend-api/Introduction/README.md)**
* **[Notification Worker](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/SCRUM-67-tina/OT-Microservices/Applications/Notification/Introduction/README.md)**

---

# 4. System Requirements

| Requirement   | Details                      |
|---------------|-----------------------------|
| **OS**        | Ubuntu 22.04 |
| **RAM**       | 4 GB minimum                |
| **Disk Space**| 40 GB                       |
| **Processor** | Dual-core recommended       |
| **Instance**  | t2.large                    |

---

# 5. Dependencies
### A. **Build-Time Tools**

| **Tool**  | **Version** | **Used For**                            |
| --------- | ----------- | --------------------------------------- |
| Python    | 3.10.12      | `attendance-api`, `notification-worker` |
| Go        | 1.21.6+       | `employee-api`                          |
| Java      | 17+         | `salary-api`                            |
|  npm | 8+      |  `React dependencies`             |



### B. **Runtime Dependencies**

| **Service**           | **Libraries/Tools Used**       |
| --------------------- | ------------------------------ |
| `employee-api`        | Redis, Scylla db |
| `attendance-api`      | Redis , Liquibase        |
| `salary-api`          |Scylladb, Redis    |
| `notification-worker` | Elastic search, smtplib           |
| `frontend`            | React         |



---
# 6. Ports

| Port  |   Description                                         |
|-------|-----------------------------------------------------|
| 8080 |   Used By Employee API     |
| 8081 | Used by Salary API                        |
|   5000  |   Used By Attendance API                              |
|   5432 |    Used By PostgreSQL                                |
|6379    | Used by Redis         |
|3000| Used by Frontend |
| 9042 |    Used By ScyllaDb      |
---

# 7. Architecture

<img width="1375" height="780" alt="image" src="https://github.com/user-attachments/assets/53a1bd8c-3e0a-49d3-b32c-1775134c776b" />




---




# 8. API Setup and Execution

## Setup Backend and Frontend API

| Component             | Setup Link                                                                                                                                           | Description                                      |
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------|
| Employee API          | [Configure Employee API](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/SCRUM-72-sachin/OT-Microservices/Applications/Employee-api/POC/README.md)         |The Employee API is a  Golang microservice that manages employee data using ScyllaDB  |
| Attendance API        | [Configure Attendance API](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/12db0585666ba06c440dd3f49d5346fa6c342599/OT-Microservices/Application/Attendance-API/POC/README.md) | The Attendance API is a Python microservice that tracks employee attendance                     |
| Salary API            | [Configure Salary API](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/SCRUM-74-sharik/OT-Microservices/Applications/Salary-API/POC/README.md)             | The Salary API is a Java microservice that manages salary processing and records using ScyllaDB for storage |
| Frontend              | [Configure Frontend API](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/SCRUM-76-ishaan/OT-Microservices/Applications/Frontend-API/POC/README.md)         | The OT-Microservices Frontend is a ReactJS app that provides a user interface for managing employee, attendance, and salary       |
| Notification Worker   | [Configure Notification Worker](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/scrum-66-sunny/OT-Microservices/Application/Notification/POC/README.md)     | The Notification Worker is a Python service that sends email alerts via SMTP to automate and streamline notification delivery             |



---

## 9. Troubleshooting

| **Issue**              | **Solution**                                              |
| ---------------------- | --------------------------------------------------------- |
| Redis not connecting   | Ensure Redis is running and accessible on port 6379       |
| DB connection fails    | Validate `.env` credentials and check firewall            |
| API not responding     | Ensure all services are running and healthy               |
| SMTP errors            | Check email credentials, SMTP port, and service status    |
| CORS / frontend issues | Validate proxy setup via Traefik and update CORS policies |

---

## 10. FAQs

#### 1. Is this application free?
Yes, OT-Microservices is open-source.

#### 2. Can I deploy this application on all Cloud platforms?
Yes, it is cloud-agnostic and can run anywhere Docker is supported.

#### 3. Do you offer an enterprise version of this application?
No, only the open-source version is available

---

## 11. Conclusion

This full-stack enables robust attendance tracking, salary computation, and employee management through modular microservices. Its scalable design allows independent development and deployment of services while maintaining performance and observability. Redis and databases serve as the backbone for service communication and data persistence.


---


## 12. Contact Information

| Name| Email Address      | GitHub | URL |
|-----|--------------------------|-------------|---------|
| Ishaan | ishaan.aggarwal.snaatak@mygurukulam.co|  Ishaan-Dev1  |   https://github.com/Ishaan-Dev1  |


---

## 13. References

| Resource                         |  Link                                                                 |
|--------------------------------|--------------------------------------------------------------------------------|
| OT-Microservices README | [Link](https://github.com/opstree/OT-Microservices) |
| Frontend documentation  | [Link](http://github.com/Snaatak-Apt-Get-Swag/documentation/blob/scrum-77-sunny/OT-Microservices/Frontend-api/Introduction/README.md) |
| Employee API documentation | [Link](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/SCRUM-73-kawalpreet/OT-Microservices/Applications/Employee-API/Introduction/README.md)                                     |
| Attendance API documentation | [Link](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/SCRUM-71-mansoor/OT-Microservices/Applications/Attendance-API/Introduction/README.md)                                  |
| Salary API documentation   | [Link](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/SCRUM-75-mansoor/OT-Microservices/Applications/Salary-API/Introduction/README.md)                                  |
