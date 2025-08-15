# Employee API POC
<img width="311" height="162" alt="Image" src="https://github.com/user-attachments/assets/4d1f784e-0f45-499b-86f5-47ca6e6c4940" />

## Author Information
| Last Updated On | Version | Author       | Level           | Reviewer   |
|-----------------|---------|--------------|-----------------|------------|
| 29-07-2025      | V1.0    | Sachin Kumar | Internal Review | Pritam     |
| 31-07-2025      | V1.1    | Sachin Kumar | L0              |Shreya/Sharvari|
| 07-08-2025      |         | Sachin Kumar | L1              | Abhishek V |
|                 |         | Sachin Kumar | L2              | Abhishek Dubey/Rishabh sharma|
---

## Table of Contents

- [Objective](#objective)
- [Pre-requisites](#pre-requisites)
  - [System Requirements](#system-requirements)
  - [Dependencies](#dependencies)
  - [Important Ports](#important-ports)
- [Step-by-step Installation](#step-by-step-installation)
- [Basic Operations](#basic-operations)
- [Contact Information](#contact-information)
- [References](#references)

---

## Objective

This Proof of Concept demonstrates the installation, configuration, and basic usage of the Employee REST API—a Golang-based microservice that performs employee-related operations within the OT-Microservice.

---

## Pre-requisites

### System Requirements

| Hardware Specifications | Minimum Recommendation            |
|------------------------ |-----------------------------------|
| Processor              | 2 cores                           |
| RAM                    | 4GB                               |
| Disk                   | 20GB SSD                          |
| OS                     | Ubuntu 22.04 LTS                  |
| Network                | Internet access for package install|

### Dependencies

| Name      | Version      | Description                                     |
|-----------|--------------|-------------------------------------------------|
| Golang    | 1.21.6+      | For building and running the API                |
| ScyllaDB  | 5.4+         | Main database                                   |
| Redis     | Latest       | Optional caching                                |
| Java      | 17           | For ScyllaDB driver/sample scripts              |
| migrate   | Latest       | DB migrations                                   |
| jq        | Latest       | CLI JSON processor                              |

### Important Ports

| Port | Description                         |
|------|-------------------------------------|
| 9042 | ScyllaDB CQL native transport port  |
| 6379 | Redis default port                  |
| 8080 | Employee API default HTTP port      |
| 8081 | Alternative API port (if 8080 busy) |

---

## Step-by-step Installation

### 1. Clone the Employee API Repository

```bash
git clone https://github.com/OT-MICROSERVICES/employee-api.git
cd employee-api
```

<img width="859" height="296" alt="Image" src="https://github.com/user-attachments/assets/0d8cf109-daa2-4f2c-bc10-cc6761e7aa80" />

### 2. Install Golang
> For Install Golang follow this documentation. [Golang Poc](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/scrum-38-hifza/Golang/Installation-Manual/README.md)

### 3. Install Java 17 
> For Install java follow this documentation. [Java Poc](https://github.com/Snaatak-Apt-Get-Swag/documentation/tree/89c6c6e8de0b6e6abbca8273c7f8ae0051f65c9b/Softwares/Java/Installation/Manual)

### 4. jq (JSON processor)
>  For Install jq follow this documentation.  [Jq POC](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/scrum-46-tina/Others/JQ/Introduction/README.md)


### 5. Install ScyllaDB and Configure ScyllaDB
> For Install ScyllaDB follow this documentation. [Scylladb POC](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/SCRUM-82-kawalpreet/OT-Microservices/Softwares/Scylladb/POC/README.md)


### 6. Update  in API Config

Edit `config.yaml`:

```yaml
scylladb:
  host: ["172.31.16.8"]
  username: scylladb
  password: password
  keyspace: employee_db

redis:
  enabled: false
  host: 172.31.16.8
  password: password
  database: 0
```
<img width="756" height="231" alt="Image" src="https://github.com/user-attachments/assets/ca52d4d3-558b-488e-96a4-cad69686b2da" />


### 7. Create the Employee Database

Enter ScyllaDB shell:

```bash
cqlsh <PRIVATE_IP>
CREATE KEYSPACE IF NOT EXISTS employee_db WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 1};
EXIT
```
<img width="1362" height="517" alt="Image" src="https://github.com/user-attachments/assets/fb95a855-c7c7-424e-8936-5fa0cfd2695f" />

### 8. Install and Start Redis
> For Install Redis follow this documentation. [Redis Poc](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/SCRUM-78-tina/OT-Microservices/Softwares/Redis/POC/README.md)

### 9. DB Migration Tool
>For Install Migration follow this documentation. [Migrate Poc](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/scrum-50-kawalpreet/Other-Tools/Migrate/SOP/README.md)

### 10. Run Employee API as a Background Service 

To keep your API running even after closing the terminal, you should run it as a background service.
Running your API as a systemd service ensures it will automatically restart on crash and survive server reboots.

1. **Create a systemd service file:**

   ```bash
   sudo nano /etc/systemd/system/employee-api.service
   ```

2. **Paste this configuration (adjust paths if needed):**

   ```ini
   [Unit]
   Description=Employee API Service
   After=network.target

   [Service]
   User=ubuntu
   WorkingDirectory=/home/ubuntu/employee-api
   ExecStart=/usr/local/go/bin/go run main.go
   Restart=always
   RestartSec=10
   Environment="GIN_MODE=release"

   [Install]
   WantedBy=multi-user.target
   ```
   <img width="878" height="299" alt="Image" src="https://github.com/user-attachments/assets/c2dfb9a9-63a7-461c-8a23-aff077636c93" />

3. **Enable and start the service:**

   ```bash
   sudo systemctl daemon-reload
   sudo systemctl start employee-api
   sudo systemctl enable employee-api  
   ```

4. **Verify it's running:**

   ```bash
   sudo systemctl status employee-api
   ```

<img width="802" height="431" alt="Image" src="https://github.com/user-attachments/assets/cccaece9-4b9b-4c26-8cfe-8dc905b00944" />

---

## Basic Operations

### 1. Health Check

```bash
curl http://localhost:8080/api/v1/employee/health
# Response:
{"message":"Employee API is running fine and ready to serve requests"}
```
<img width="1172" height="504" alt="Image" src="https://github.com/user-attachments/assets/b9057cbe-42f7-4d65-931b-a89045a68883" />

Or test in browser:

```
http://<your-public-ip>:8080/api/v1/employee/health
```
<img width="1362" height="764" alt="Image" src="https://github.com/user-attachments/assets/10f91475-1338-4d77-a021-27623c061665" />

### 2. API Endpoints

| Endpoint                             | Method | Description                   |
|-------------------------------------- |--------|-------------------------------|
| /api/v1/employee/health              | GET    | Health check                  |
| /api/v1/employee                     | POST   | Create employee record        |
| /api/v1/employee/{id}                | GET    | Get employee by ID            |
| /api/v1/employee/{id}                | PUT    | Update employee               |
| /api/v1/employee/{id}                | DELETE | Delete employee               |
| /api/v1/employee                     | GET    | List all employees            |

---


## Contact Information

| Name            | Email address                           |
|-----------------|----------------------------------------|
| Sachin Kumar    | [sachin.kumar.snaatak@mygurukulam.co](sachin.kumar.snaatak@mygurukulam.co) |

---

## References

| Link                                                                                   | Description                               |
|---------------------------------------------------------------------------------------|-------------------------------------------|
| [Employee API GitHub](https://github.com/OT-MICROSERVICES/employee-api)               | Employee API source code                  |
| [ScyllaDB Documentation](https://docs.scylladb.com/)                                  | Official ScyllaDB documentation           |
| [ScyllaDB Installation Guide](https://docs.scylladb.com/stable/operating-scylla/procedures/install/install-ubuntu.html) | ScyllaDB Ubuntu installation guide      |
| [Go Documentation](https://golang.org/doc/)                                           | Official Golang docs                      |
| [Redis Documentation](https://redis.io/documentation)                                 | Official Redis docs                       |
