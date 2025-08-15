# Frontend API Documentation

---

## Author Information
| Last Updated On | Version | Author           | Level           | Reviewer               |
|-----------------|---------|------------------|-----------------|------------------------|
| 06-08-2025      | V1.1    |  Sunny Kumar | Internal Review | Aman Raj                 |
|  06-08-2025               |       | Sunny Kumar  | L0              | Shikha Tripathi        |
|                 |         | Sunny Kumar  | L1              | Kirti Nehra            |
|                 |         | Sunny Kumar  | L2              | Ashwini Singh/Deepak Nishad |



---

<details>
  <summary><strong> Table of Contents</strong></summary>

1. [Introduction](#1-introduction)
2. [Purpose](#2-purpose)  
3. [Pre-Requisites](#3-pre-requisites)  
4. [System Requirements](#4-system-requirements)   
5. [Ports](#5-ports)
6. [Architecture](#6-Architecture)
7. [Installation of Frontend API](#7-Installation-of-Frontend-API)   
8. [Troubleshooting](#8-troubleshooting)   
9. [Contact Information](#9-contact-information)  
10. [References](#10-references) 
</details>

---
## Purpose

The OT-MICROSERVICES Frontend is a ReactJS web application that provides the main user interface for the OT-Microservices stack. It enables management and visualization of employee, attendance, and salary information by communicating with the backend REST APIs.

---

## Pre-requisites

| **Dependency** | **Version** |
| -------------- | ----------- |
| Node.js        | v12.22.9    |
| npm            | v8.5.1      |
| Make       | v4.3        |




---
## System Requirements

| Hardware/Software | Minimum Recommendation  |
|-------------------|------------------------|
| Processor         | 1 CPU (t2.micro EC2)   |
| RAM               | 1 GiB                  |
| Disk              | 8 GB                   |
| OS                | Ubuntu 22.04 LTS       |

---




## Ports


| Port | Service              | Description                         |
|------|---------------------|-------------------------------------|
| 22   | SSH                 | Remote terminal access              |
| 3000 | HTTP (ReactJS)      | Default frontend port               |


---


## Architecture

**Description:**  
This architecture diagram shows a front-end web app interacting with three microservices—Employee API, Salary API, and Attendance API—via REST endpoints. 

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/84caf3b1-926f-4578-826c-0cddbf3d8f49" />

<br>

| Component          | Description                                                                                                |
| ------------------ | ---------------------------------------------------------------------------------------------------------- |
| **Frontend**       | The user interface of the application that interacts with users and sends/receives data from backend APIs. |
| **Employee API**   | Manages employee-related data such as profiles, departments, and contact information.                      |
| **Salary API**     | Handles salary processing, including payroll calculations, and payments.                       |
| **Attendance API** | Tracks employee attendance, leaves, and working hours.                                                     |



---




## API Setup and Execution

_Follow this link for API Setup guide_  
(**[Click here to view Setup guide](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/SCRUM-76-ishaan/OT-Microservices/Applications/Frontend-API/POC/README.md)**)


---

## Troubleshooting

| Issue                        | Solution                                                         |
|------------------------------|------------------------------------------------------------------|
| JS heap out of memory        | Use `NODE_OPTIONS="--max_old_space_size=4096" npm run build`     |
| Port 3000 in use             | Run `serve -s build -l 3001` or kill previous process            |
| Cannot access externally     | Check firewall/security group for port 3000                      |
| Deprecated/vulnerable dependencies   | Run `npm audit fix` (optional, not blocking for deployment)      |                           |

---



## Contact Information

| **Field** | **Details**                                                                     |
| --------- | ------------------------------------------------------------------------------- |
| Name      | Sunny Kumar                                                                     |
| Email     | [sunny.kumar.snaatak@mygurukulam.co](mailto:sunny.kumar.snaatak@mygurukulam.co) |

---

## References


| Description                       | Link                                                                 |
|------------------------------------|----------------------------------------------------------------------|
| Frontend API                       | https://github.com/OT-MICROSERVICES/frontend                         |
| Javascript heap out of memory      | https://geekflare.com/fix-javascript-heap-out-of-memory-error/       |
