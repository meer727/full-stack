## Documentation of Salary Api

![logo](https://github.com/user-attachments/assets/675b2dfa-3306-48db-9846-d7f5f981b799) 

##  **Author Information**
| Created     | Last Updated  | Author  |   Version          | Comment          | Reviewer         |
|-------------|---------|---------------|--------------------|------------------|------------------|
| 29-07-2025  | 03-08-2025 | Mansoor Alam   |   V1 | Internal Reviewer| Anjali Dhiman        |
| 29-07-2025  | 07-08-2025   | Mansoor Alam   |   V2 | L0| Priyanshu Yadav      |
| 29-07-2025  | 13-08-2025   | Mansoor Alam   |   V3 | L1| kirti Nehra   |

---


##  Table of Contents

1. [Introduction](#introduction)
2. [What is Salary API?](#what-is-salary-api)   
3. [Pre-Requisites](#pre-requisites)  
    - [System Requirements](#system-requirements)  
4. [Dependencies](#dependencies)  
    - [Build Time Dependency](#build-time-dependency)  
    - [Run Time Dependency](#run-time-dependency)  
    - [Important Ports](#important-ports)  
6. [Architecture](#architecture)
    - [Flow Overview](#flow-overview)
7. [Troubleshooting](#troubleshooting)
8. [Endpoints](#endpoints)
9. [Conclusion](#conclusion)
10. [Contact Information](#contact-information)  
11. [References](#references)  

---

## Introduction
This documentation provides a complete guide to the Salary API microservice, including its architecture, dependencies, deployment steps, and runtime behavior. It is intended for developers and DevOps engineers to help understand, set up, and maintain the service within the OT-Microservices ecosystem.


**SALARY API POC [CLICK HERE](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/SCRUM-74-sharik/OT-Microservices/Applications/Salary-API/POC/README.md)**

## What is Salary API?  
Salary API is a Java-based microservice designed to handle salary processing and record-keeping in a scalable and efficient manner.  
It integrates with ScyllaDB and Redis to ensure fast access and reliable storage, and fits into a larger microservices architecture.  
This service acts as a centralized source for all salary-related data and operations.


---
## Pre-Requisites

To run the Salary API application, certain dependencies and tools must be in place. While **Maven** is sufficient to compile the code, running the application requires the following components:

| Tool/Resource | Description |
|---------------|-------------|
| **[ScyllaDB](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/SCRUM-83-sharik/OT-Microservices/Softwares/Scylladb/Introduction/README.md)**  | A highly efficient NoSQL database optimized for low latency and high throughput, suitable for handling large volumes of salary data. |
| **[Redis](link)**     | An in-memory data store used for caching and message brokering, which significantly improves response times and overall performance. |
| **[Migrate](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/scrum-50-kawalpreet/Other-Tools/Migrate/SOP/README.md)**   | A schema migration tool that helps in managing and applying database changes across environments reliably. |
| **[Maven](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/scrum-34-mansoor/Softwares/Java/Maven/SOP/README.md)**     | A build and dependency management tool for Java projects, used here to compile and package the application. |
| **[Java](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/scrum-30-nitin/Softwares/Java/Introduction/README.md)**      | The core programming language used to develop the application, requiring a Java Runtime Environment (JRE) to execute. |


- ### System Requirements

| Requirement   | Details                      |
|---------------|------------------------------|
| OS            | Ubuntu 22.04 or other Linux-based OS |
| RAM           | 4 GB minimum                |
| Disk Space    | 15 GB                       |
| Processor     | Dual-core recommended       |
| Instance Type | t2.medium                   |

---
## Dependencies

- ### Build Time Dependency


| Tool      | Description                                                                 |
|----------|-----------------------------------------------------------------------------|
| **Maven**         | A powerful build automation tool used primarily for Java projects. Handles project dependencies, builds, and plugins. |
| **OpenJDK**        | The open-source implementation of the Java Platform, Standard Edition. Required for compiling and running Java applications. |
| **Make**          | A build automation tool used in Unix/Linux environments. Executes instructions defined in Makefiles. |
| **jq**             | A lightweight and flexible command-line JSON processor. Useful for reading and manipulating JSON data in shell scripts. |

- ### Run Time Dependency

The following databases are used in this project. Ensure they are properly installed and running before executing the application.

| Database    | Description                                                                 |
|------------|-----------------------------------------------------------------------------|
| **ScyllaDB**     | A high-performance, distributed NoSQL database compatible with Apache Cassandra. Ideal for handling large-scale, low-latency workloads. |
| **Redis**      | An in-memory key-value data store, widely used as a database, cache, and message broker. Lightweight and fast. |

---
- ### Important Ports

| *Port* | *Protocol/Service*       | *Description*                                                              |
|----------|----------------------------|------------------------------------------------------------------------------|
| 22       | SSH                        | Used for secure shell access to the server.                                    |
| 8080     | HTTP (Swagger UI)          | Used for accessing Swagger UI for API documentation.                          |
| 9042     | ScyllaDB                   | The default port for connecting to ScyllaDB (Cassandra-compatible database).   |
| 6379     | Redis                      | The default port for connecting to the Redis in-memory data store.            |
| 80       | HTTP                       | Used for standard web traffic and serving HTTP requests.                      |
| 443      | HTTPS                      | Used for secure web traffic and serving HTTPS requests.                       |

---
## Architecture

<img width="1760" height="760" alt="salary" src="https://github.com/user-attachments/assets/23ca7164-2b03-4c60-98d4-23affb07e5d9" />



---

### Flow Overview

| Step                              | Description                                                                                     |
|-----------------------------------|-------------------------------------------------------------------------------------------------|
| User → Salary API | A client sends a request to the Salary API to retrieve or manage salary-related data.           |
| Salary API → Redis              | The API checks Redis cache to see if the requested data is already available (cache lookup).    |
|  Redis (Cache Hit)              | If the data is found in Redis, it is immediately returned to the API and then to the client.    |
|  Redis (Cache Miss)             | If the data is not found, Redis fetches it from the primary database (ScyllaDB).                |
| Redis → ScyllaDB               | On cache miss, Redis queries ScyllaDB for the required salary information.                      |
|  ScyllaDB → Redis → API         | ScyllaDB returns the data, which Redis caches temporarily and forwards to the Salary API.       |
|  Salary API → Client            | The API sends the final response back to the user or client, completing the request cycle.      |


---

## Troubleshooting

| **Issue**                             | **Possible Cause**                                              | **Suggested Solution**                                                                 |
|--------------------------------------|-----------------------------------------------------------------|----------------------------------------------------------------------------------------|
| **Unable to Connect to ScyllaDB**    | ScyllaDB is not running or misconfigured (host/port issue)      | Ensure ScyllaDB is active using `nodetool status`. Verify host/port in `application.yml`. |
| **Unable to Connect to Redis**       | Redis is not running or misconfigured (host/port issue)         | Check host and port in the config file. Confirm Redis is active using `ss -tuln`.     |
| **Connection Issues on EC2**         | EC2 Security Group rules are blocking access                    | Open inbound ports 6379 (Redis) and 9042 (ScyllaDB) in your EC2 Security Group settings. |
| **Port 8080 Already Occupied**       | Another application is using port 8080                          | Run `lsof -i :8080` to find the process. Kill it or update the app to use a different port. |


---

## Endpoint Information

| **Endpoint**                   | **Method** | **Description**                                                                               |
|--------------------------------|------------|-----------------------------------------------------------------------------------------------|
| `/api/v1/salary/create/record` | POST       | Data creation endpoint which accepts certain JSON body to add salary information in database  |
| `/api/v1/salary/search`        | GET        | Endpoint for searching data information using the params in the URL                           |
| `/api/v1/salary/search/all`    | GET        | Endpoint for searching all information across the system                                      |
| `/actuator/prometheus`         | GET        | Application healthcheck and performance metrics are available on this endpoint                |
| `/actuator/health`             | GET        | Endpoint for providing shallow healthcheck information about application health and readiness |


## Conclusion  
The Salary API offers a scalable and reliable solution for managing salary operations within the OT-Microservices ecosystem.  
With Redis caching and ScyllaDB storage, it ensures high performance. This documentation supports easy setup, deployment, and maintenance.

## Contact Information

| Name         | Email Address                                 |
|--------------|-----------------------------------------------|
| Mansoor Alam | mansoor.alam.snaatak@mygurukulam.co           |

---
## References

| Reference               | Link                                                                           |
|-------------------------|--------------------------------------------------------------------------------|
| ScyllaDB Installation documentation    | [Click here](https://docs.scylladb.com/manual/master/getting-started/install-scylla/index.html)                     |
| Redis Installation Guide              | [Click here](https://redis.io/docs/latest/operate/oss_and_stack/install/archive/install-redis/install-redis-on-linux/)                           |
| Maven                   | [Click here](https://www.digitalocean.com/community/tutorials/install-maven-linux-ubuntu)                                         |
