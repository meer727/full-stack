# Documentation on Employee-API

<img width="311" height="162" alt="Image" src="https://github.com/user-attachments/assets/4d1f784e-0f45-499b-86f5-47ca6e6c4940" />

---

## Author Information
| Last Updated On | Version | Author           | Level           | Reviewer               |
|-----------------|---------|------------------|-----------------|------------------------|
| 01-08-2025      | V1.0    | Kawalpreet Kour  | Internal Review | Pritam                 |
| 05-08-2025      | V1.0    | Kawalpreet Kour  | L0              | Shreya/Sharvari        |
| 07-08-2025      | V1.1   | Kawalpreet Kour  | L1              | Abhishek V             |
|                 |         | Kawalpreet Kour  | L2              | Abhishek Dubey/Rishabh sharma |

---

<details>
  <summary><h2><strong>Table of Contents</strong></h2></summary>

- [Purpose](#purpose)
- [Pre-requisites](#pre-requisites)  
- [Architecture](#architecture)  
- [Dataflow Diagram & Steps](#dataflow-diagram--steps)  
- [Step-by-step installation](#step-by-step-installation-of-employee-api)  
- [Troubleshooting](#troubleshooting)  
- [Endpoint Information](#endpoint-information)
- [Notes](#notes)
- [Contact Information](#contact-information)
- [References](#references)
</details>

---
## Purpose

This document provides a clear and concise guide to set up, configure, and manage the Employee API, which streamlines employee data handling, ensures secure access, and integrates seamlessly with enterprise systems for better automation and data management.

---

## Pre-requisites

Before starting the deployment process, verify that the system meets all necessary hardware, software, and security requirements.

---
### System Requirements

| Hardware Specifications | Minimum Recommendation |
|------------------------|-----------------------|
| Processor              | Dual-core             |
| RAM                    | 4GB                   |
| Disk                   | 20GB SSD              |
| OS                     | Ubuntu 22.04 LTS      |

---

### Dependencies

### Build time Dependency

| Name   | Version  | Description                        |
|--------|----------|------------------------------------|
| Golang | 1.21.6+  | For building and running the API   |
| Make    | Latest    | Automation of build tasks    |
| Git     | Latest    | Source code management       |

### Run time Dependency

| Name      | Version  | Description                              |
|-----------|----------|------------------------------------------|
| ScyllaDB  | v6.1+     | Main database for employee data          |
| Redis     | Latest   | Optional caching                         |
| Java      | 17       | For ScyllaDB driver/sample scripts       |
| migrate   | Latest   | DB migrations                            |

---

### Important Ports

| Port  | Description                        |
|-------|------------------------------------|
| 22   | SSH          | Server access                            |
| 80   | HTTP         | Web traffic                              |
| 443  | HTTPS        | Secure web traffic  
| 8080 | Employee API | API & Swagger UI 
| 9042  | Used by ScyllaDB                   |
| 6379  | Redis default port                 |

---

### Other Dependency

| Name         | Version | Description                           |
|--------------|---------|---------------------------------------|
| Swagger UI| Latest    | API documentation & testing    |
| jq        | Latest    | Command-line JSON processor    |

---
## Architecture

**Description:**  
The Employee API is a stateless, RESTful Golang microservice that interacts with ScyllaDB for persistent employee data storage and optionally with Redis for caching. It is deployed as a systemd service for reliability and scalability.

<img width="800" height="410" alt="empsal" src="https://github.com/user-attachments/assets/f6594fac-0c44-484e-97be-27d2faca5944" />

---

## Dataflow Diagram & Steps

| Step | Component        | Action                                                                 |
|------|------------------|------------------------------------------------------------------------|
| 1    | **User**         | Interacts with UI to view/add/update employee data                     |
| 2    | **Frontend UI**  | Sends HTTP request to Employee API                                     |
| 3    | **Employee API** | Validates request, applies business logic, checks Redis cache          |
| 4a   | **Redis** (Cache Hit) | Returns cached data to API                                             |
| 4b   | **Redis** (Cache Miss) → **ScyllaDB** | If data is not cached, fetches from DB and may update Redis          |
| 5    | **Employee API** | Returns structured JSON response to UI                                 |
| 6    | **Frontend UI**  | Renders data as table view / confirmation / error message              |

---


<img width="500" height="600" alt="data flow-employee" src="https://github.com/user-attachments/assets/f8435420-a354-419c-a5e0-70d99129cd42" />

---

### Key Technologies

- **Frontend**: React  (based on UI implementation)
- **Backend**: Golang Employee API
- **Cache**: Redis
- **Database**: ScyllaDB (NoSQL, Column-store)

---

## Step-by-step installation of Employee API

_Follow this link for full installation guide_  
(**[Click here to view installation guide](https://team-snaatak-p-15.atlassian.net/browse/SCRUM-72)**)


---

## Troubleshooting

Common issues and resolutions:

| **Issue**                             | **Possible Cause**                                      | **Suggested Solution**                                                                 |
|--------------------------------------|---------------------------------------------------------|-----------------------------------------------------------------------------------------|
| API not running                      | Port conflict                                            | Run `netstat -tuln | grep 8080` and change port in code if needed                      |
| ScyllaDB not starting                | Service error or crash                                  | Check logs using `sudo journalctl -u scylla-server` and DB status with `nodetool status` |
| Go not found / Python error          | Missing dependencies                                     | Run: `sudo apt update && sudo apt install python3-apt golang-go -y`                   |
| Redis not responding                 | Redis service not started                               | Start it: `sudo systemctl start redis-server`                                          |
| Wrong ScyllaDB host/IP               | Incorrect config in `config.yaml`                       | Edit the file and correct `scylladb.host` value                                        |
| Swagger UI not working               | Private IP not set correctly in Swagger URL             | In `main.go`, set: `url := ginSwagger.URL("http://<PRIVATE_IP>:8080/swagger/doc.json")`|

---
## Notes

- Redis acts as a performance booster by caching frequent queries.
- ScyllaDB handles large-scale data operations efficiently.
- System is scalable and stateless for reliability.
---


## Endpoint Information

| Endpoint                             | Method | Description                   |
|-------------------------------------|--------|-------------------------------|
| `/api/v1/employee/health`           | GET    | Health check endpoint         |
| `/api/v1/employee`                  | POST   | Create new employee record    |
| `/api/v1/employee/{id}`             | GET    | Fetch employee by ID          |
| `/api/v1/employee/{id}`             | PUT    | Update employee details       |
| `/api/v1/employee/{id}`             | DELETE | Delete employee record        |
| `/api/v1/employee`                  | GET    | Get all employee records      |

---

## Contact Information

| Name             | Email                                         |
|------------------|-----------------------------------------------|
| Kawalpreet Kour  | Kawalpreet.kour.snaatak@mygurukulam.co        |

---

## References

| Description                     | Link                                                                                          |
|---------------------------------|-----------------------------------------------------------------------------------------------|
| ScyllaDB Installation Guide     | [Click here](https://team-snaatak-p-15.atlassian.net/browse/SCRUM-82)                         |
| Redis Installation Guide        | [Click here](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/SCRUM-78-tina/OT-Microservices/Softwares/Redis/POC/README.md) 
