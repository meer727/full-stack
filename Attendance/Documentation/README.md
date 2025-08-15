##    **Attendance API Documentation**

![logo](https://github.com/user-attachments/assets/6ab62d57-5eaf-4277-aa4e-487cb012b1a5)

## Author Information
| Created     | Last updated | Version | Author         | Comment | Reviewer |
|-------------|-----------|---------|----------------|---------|----------|
| 1-08-2025  | 4-08-2025 | V1  | Mansoor Alam |     Internal Review    | Anjali Dhiman    |
| 1-08-2025  | 7-08-2025 | V1  | Mansoor Alam |     L0    | Priyanshu Yadav    |
| 1-08-2025  | 13-08-2025 | V1  | Mansoor Alam |     L1  | Kirti Nehra   |


## **Table of Contents**

-   [**Introduction**](#introduction)
-   [**What is Attendance API ?**](#what-is-attendance-api)
-   [**Environment Setup**](#environment-setup)
      - [**Pre-requisites**](#pre-requisites)
      -  [**System Requirements**](#system-requirements)
      -  [**Dependencies**](#dependencies)
      -  [**Important Ports**](#important-ports)
-   [**Flow Diagram of Attendance API ?**](#flow-diagram-of-attendance-api)
    - [**Flow Overview**](#flow-overview)
    - [**Related Resources**](#related-resources)
-   **[Technology Stack](#technology-stack)**
    -   [**PostgreSQL**](#postgresql)
    -   [**Redis**](#redis)
    -   [**Poetry**](#poetry)
-   [**Conclusion**](#conclusion)
-   [**Contact Information**](#contact-information)
-   **[References](#references)**



## Introduction  
This documentation provides a comprehensive guide to the Attendance Microservice, covering its architecture, key components, and how it integrates into the overall system, It is intended for developers and DevOps engineers to understand, the document also highlights caching, storage, and deployment practices to ensure performance and reliability.



## What is Attendance API?  
Attendance API is a Python-based microservice designed to manage and track user attendance efficiently.  
It connects to a PostgreSQL database for storage, uses Redis for caching, and handles schema migrations with Liquibase.  
This API ensures high performance, modularity, and ease of integration within larger distributed systems.

---
## Environment Setup

To run the Attendance API application, the following tools and resources must be set up and configured properly. These components are essential for development, runtime, and maintaining consistency across environments.

- ### Pre-requisites 

| Tool/Resource | Description |
|---------------|-------------|
| **[PostgreSQL](https://github.com/Snaatak-Apt-Get-Swag/documentation/tree/SCRUM-81-sachin/OT-Microservices/Softwares/Postgresql/Introduction)** | A powerful open-source relational database system used to persist and manage all attendance records reliably. |
| **[Redis](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/SCRUM-78-tina/OT-Microservices/Softwares/Redis/POC/README.md)** | An in-memory key-value store used for caching frequently accessed attendance data, improving response times. |
| **[Poetry](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/scrum-25-mansoor/Softwares/Python/Poetry/SOP/README.md)** | A Python dependency management and packaging tool that ensures consistent environments across development and production. |

 
- ### System Requirements

| Requirement   | Details                      |
|---------------|------------------------------|
| OS            | Ubuntu 22.04 or other Linux-based OS |
| RAM           | 4 GB minimum                |
| Disk Space    | 15 GB                       |
| Processor     | Dual-core recommended       |
| Instance Type | t2.medium                   |

---

- ### Dependencies
| **Name**      | **Description**                                                   |
|--------------|-------------------------------------------------------------------|
| Poetry           | Python package manager for managing dependencies                |
| PostgreSQL        | Relational database system to store attendance data securely.    |
| Python       | Used for running the core application and managing dependencies. |
| Redis            | In-memory key-value store for caching to improve performance.    |


---

- ### **Important Ports**
| **Port No.** | **Service**        | **Description**                                      |
|--------------|--------------------|------------------------------------------------------|
| 22           | SSH                | Port used for secure remote login and VM access.    |
| 6379         | Redis              | Port used for Redis client connections.             |
| 8080         | Application        | Port used to run and access the application.        |
| 5432         | PostgreSQL         | Port for PostgreSQL database connections.           |
| 80/443       | HTTP/HTTPS         | Ports used for web browsing (HTTP) and secure connections (HTTPS). |

---
## **Flow Diagram of Attendance API** 

<img width="1800" height="576" alt="attendance" src="https://github.com/user-attachments/assets/b60d8750-fac2-48af-8276-8fc166d5845e" />



### Flow Overview

| Step                  | Description |
|-----------------------|-------------|
| **1. User Interaction** | The user initiates an HTTP request via the **Frontend Web Interface** (e.g., marking attendance, viewing records). |
| **2. Frontend Processing** | The **Frontend Web** receives the request and communicates with the **Attendance API** to perform required operations like fetching, storing, or updating attendance data. |
| **3. Redis Caching** | The **Attendance API** first checks **Redis Cache** to serve data quickly. If data exists, it’s fetched directly from Redis, reducing load on the database. |
| **4. Database Persistence** | If the data is not found in Redis or needs to be stored, the API interacts with **PostgreSQL**, which acts as the primary persistent storage for all attendance records. |
| **5. Response** | Once the request is processed (via Redis or PostgreSQL), the API returns the result to the **Frontend Web**, which formats and sends the final response to the user. |

---


### Related Resources
**For detailed information of system requirements, architecture, flow diagram, and installation steps, please refer to the following resources**
| Link         | Description         |
|--------------|------------------------|
| [Attendance API](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/scrum-70-hifza/OT-Microservices/Application/Attendance-API/POC/README.md ) |POC of Attendance records and data flow.| 


## **Technology Stack**

- PostgreSQL  

- Redis  

- Poetry  


## **PostgreSQL**

PostgreSQL is a great choice for managing attendance data because it handles organized data well, ensures the data is safe and correct through ACID (Atomicity, Consistency, Isolation, and Durability), and can grow as your project gets bigger.


**For detailed information, look at this PostgreSQL PoC:**

| Link         | Description         |
|--------------|---------------------|
| [PostgreSQL](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/SCRUM-80-nitin/OT-Microservies/Softwares/Postgresql/POC/README.md ) | PostgreSQL POC for attendance data management. |

## **Redis**

Redis speeds up the app by storing data in memory, making it ideal for managing user sessions and real-time updates like attendance changes. It's faster than traditional databases for temporary data.

**For detailed information, look at this Redis PoC:**

| Link         | Description         |
|--------------|---------------------|
| [Redis](link) | Redis PoC for managing user sessions and real-time attendance updates.|



## **Poetry**

Poetry simplifies managing Python dependencies, ensuring libraries are in sync and making sharing or deploying the app easier, maintaining a smooth development flow.


**For detailed information, look at this Poetry PoC:**

| Link         | Description         |
|--------------|---------------------|
| [Poetry ](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/scrum-25-mansoor/Softwares/Python/Poetry/SOP/README.md ) | Poetry PoC for managing Python dependencies and ensuring consistent development.|
---


## Conclusion  
The Attendance API uses PostgreSQL, Redis, Liquibase, Poetry, and Gunicorn to deliver a fast, scalable, and reliable attendance management system. These tools ensure secure storage, efficient performance, and smooth deployment. The service is production-ready and easy to maintain.

##  **Contact Information**


| **Name**    | **Email address**         |
|-------------|---------------------------|
| Mansoor Alam | mansoor.alam.snaatak@mygurukulam.co    |



## **References**

| No. | Reference                                              | Description              |
|-----|--------------------------------------------------------|--------------------------|
| 1   |  [PostgreSQL Official Site](https://www.postgresql.org) | Visit official website   |
| 2   |  [Redis Official Site](https://redis.io)              | Visit official website   |
| 3   |  [Liquibase Official Site](https://www.liquibase.org) | Visit official website   |
| 4   |  [Gunicorn Official Site](https://gunicorn.org)       | Visit official website   |
