# POC Of Salary API

---

##  Author Information

| Created     | Last updated | Version | Author      | Level           | Reviewer       |
|-------------|--------------|---------|-------------|-----------------|----------------|
| 29-07-2025  | 30-07-2025   | V1      | Mohd Sharik | Internal Review | Aman Raj       |

---

# Table of Contents

1. [Introduction](introduction)
2. [System Requirements](#system-requirements)
3. [Pre-requisites ](#pre-requisites )
4. [Architecture](#architecture)
5. [Pre-requisites ](#pre-requisites )
6. [API Setup and Execution](#api-setup-and-execution)
   - [Update System Packages](#update-system-packages)
   - [Installing Dependencies for Salary API](#installing-dependencies-for-salary-api)
   - [Working with the Salary API Git Repository](#working-with-the-salary-api-git-repository)
   - [Starting the Application](#starting-the-application)
   - [Access the API](#access-the-api)
7. [Conclusion](#conclusion)
8. [Contact](#contact)
9. [References](#references)

---

# Introduction

Salary API is a Java-based microservice in the OT-Microservices architecture that manages salary processing and record-keeping. It’s platform-independent and runs wherever a Java Runtime Environment (JRE) is available, ensuring accurate payroll handling and smooth integration with other services.

---

# System Requirements

| Requirement   | Details                     |
|---------------|-----------------------------|
| OS            | Ubuntu or other Linux Distro|
| RAM           | 4 GB minimum                |
| Disk Space    | 20 GB                       |
| Processor     | Dual-core recommended       |
| Instance Type | t2.medium                   |

---

# Pre-requisites 

| **Port** | **Protocol/Service**       | **Description**                                                              |
|----------|----------------------------|------------------------------------------------------------------------------|
| 22       | SSH                        | Used for secure shell access to the server.                                    |
| 8080     | HTTP (Swagger UI)          | Used for accessing Swagger UI for API documentation.                          |
| 9042     | ScyllaDB                   | The default port for connecting to ScyllaDB (Cassandra-compatible database).   |
| 6379     | Redis                      | The default port for connecting to the Redis in-memory data store.            |
| 80       | HTTP                       | Used for standard web traffic and serving HTTP requests.                      |
| 443      | HTTPS                      | Used for secure web traffic and serving HTTPS requests.                       |

---

# Architecture

![WhatsApp Image 2025-08-04 at 12 43 41 PM](https://github.com/user-attachments/assets/54e97576-b460-4250-ad05-09daa281fe7e)


---

# API Setup and Execution

## Update System Packages

```bash
sudo apt update
```





## Installing Dependencies for Salary API
| **Tool**      | **Installation Steps**                                                                                              |
|---------------|--------------------------------------------------------------------------------------------------------------------|
| **ScyllaDB**  | Follow this [link](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/SCRUM-82-kawalpreet/OT-Microservices/Softwares/Scylladb/POC/README.md) to install and configure ScyllaDB |
| **Redis**     | Follow this [link](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/SCRUM-78-tina/OT-Microservices/Softwares/Redis/POC/README.md) to install and configure Redis     |
| **Java 17**   | Follow this [link](https://github.com/Snaatak-Apt-Get-Swag/documentation/tree/89c6c6e8de0b6e6abbca8273c7f8ae0051f65c9b/Softwares/Java/Installation/Manual) to install Java                                                                        |
| **Maven**     | Follow this [link](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/scrum-34-mansoor/Softwares/Java/Maven/SOP/README.md) to install Maven                                                                                       |
| **jq**        | Follow this [link](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/scrum-46-tina/Others/JQ/Introduction/README.md) to install Jq                                                                                          |
| **make**      | Follow this [link](https://github.com/Snaatak-Apt-Get-Swag/documentation/blob/scrum-48-nitin/Others/Make/SOP/README.md) to install Make                                                                                        |

### golang-migrate
**1. Download and extract:**
```bash
curl -L https://github.com/golang-migrate/migrate/releases/download/v4.15.2/migrate.linux-amd64.tar.gz | tar xvz
```

<img width="1916" height="349" alt="Screenshot from 2025-07-30 09-01-29" src="https://github.com/user-attachments/assets/2beec8e7-4eed-47e9-9005-bd0ee00ff211" />

**2. Move the binary:**
   ```bash
   sudo mv migrate /usr/local/bin/migrate
   ```

<img width="1920" height="107" alt="Screenshot from 2025-07-31 02-27-31" src="https://github.com/user-attachments/assets/c6f0f042-1fc5-4f71-b672-a5045f7f759c" />


## Working with the Salary API Git Repository
**1. Clone the repository:**
   ```bash
   git clone https://github.com/OT-MICROSERVICES/salary-api.git
   cd salary-api
   ```

<img width="1919" height="351" alt="Screenshot from 2025-07-30 09-02-20" src="https://github.com/user-attachments/assets/099207b7-85ae-4b9f-b2e2-bacbc1d2f4ff" />

**2. Configure migration:**
   ```bash
   vi migration.json
   ```
   - Replace the IP address with your instance  private IP.

<img width="1920" height="293" alt="Screenshot from 2025-07-30 11-21-33" src="https://github.com/user-attachments/assets/beed1adb-baa1-4f50-8177-402079d85b8f" />

**3. Update application configuration:**
 ```bash
 vi src/main/resources/application.yml
 ```
 - Replace the IP address with your instance private IP.

<img width="1920" height="419" alt="Screenshot from 2025-07-30 11-22-06" src="https://github.com/user-attachments/assets/0db63ce3-5094-4327-a4a1-940b743ff732" />


**4. Update test configuration:**
   ```bash
   vi src/test/resources/application.yml
   ```
   - Replace the IP address with your instance private IP.

<img width="1920" height="419" alt="Screenshot from 2025-07-30 11-22-23" src="https://github.com/user-attachments/assets/bc3ae835-1692-4492-8c66-c087a1997984" />



**5. Update Java API configuration:**
   ```bash
   vi src/main/java/com/opstree/microservice/salary/config/OpenAPIConfig.java
   ```
**Changes to make in this file**

- Add the following inside (import java.util.List;) for configuring and enabling Cross-Origin Resource Sharing (CORS) .
```bash
import org.springframework.web.cors.CorsConfiguration;
import org.springframework.web.cors.UrlBasedCorsConfigurationSource;
import org.springframework.web.filter.CorsFilter;
```



- Replace the Localhost with your **public** IP (in devserver.seturl in `myOpenAPI() @bean`).
  
- add the following CorsFilter bean after the existing myOpenAPI() bean definition inside the OpenAPIConfig class.
  
     ```java
     @Bean
     public CorsFilter corsFilter() {
       CorsConfiguration config = new CorsConfiguration();
       config.addAllowedOrigin("*");
       config.addAllowedMethod("*");
       config.addAllowedHeader("*");
       UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
       source.registerCorsConfiguration("/**", config);
       return new CorsFilter(source);
     }
     ```

<img width="1920" height="979" alt="Screenshot from 2025-07-30 11-24-33" src="https://github.com/user-attachments/assets/67b78fec-e3e8-4959-9638-0de60f096a52" />


**6. Update test configurations:**
   ```bash
   sudo vi src/test/java/com/opstree/microservice/salary/config/OpenAPIConfigTests.java
   ```
   - Replace the IP address with your **public** IP (n assertEquals).

<img width="1920" height="508" alt="Screenshot from 2025-07-30 11-19-27" src="https://github.com/user-attachments/assets/1bb48096-07bb-4e7f-b528-e450580a7836" />

## Starting the Application

**1. Run migrations:**
   ```bash
   make run-migrations
   ```

<img width="1920" height="102" alt="Screenshot from 2025-07-30 09-11-11" src="https://github.com/user-attachments/assets/253bb3aa-717f-47c2-8ed1-f42a05e5c527" />


**2. Build the application:**
   ```bash
   make build
   ```
<img width="1920" height="557" alt="Screenshot from 2025-07-30 09-10-14" src="https://github.com/user-attachments/assets/3b0cb2c8-5bc1-42dc-ab8e-d5a802fe379d" />

<img width="1920" height="563" alt="Screenshot from 2025-07-30 09-08-33" src="https://github.com/user-attachments/assets/80f2d3b4-7c2a-4eb9-901b-fa98ceb5f380" />

**3. Create a service file:**

A service file manages the salary API with systemd, enabling automatic startup, easy control via systemctl, and recovery from failures, ensuring reliable and seamless operation.

```bash
sudo nano /etc/systemd/system/salary-api.service
   ```


   ```bash
   [Unit]
Description=Salary API Service
After=network.target

[Service]
User=ubuntu
WorkingDirectory=/home/ubuntu/salary-api
ExecStart=/usr/bin/java -jar /home/ubuntu/salary-api/target/salary-0.1.0-RELEASE.jar
SuccessExitStatus=143
Restart=on-failure
Environment=JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
Environment=SCYLLA_HOST=<private-ip>
Environment=REDIS_HOST=<private-ip>

[Install]
WantedBy=multi-user.target
```

**4. Service File Commands**

Below are the commands to manage the `salary-api` service using `systemd`. These commands help reload systemd configuration, enable the service to start on boot, start the service manually, and check its current status.

| Command                                  | Description                                                              |
|------------------------------------------|--------------------------------------------------------------------------|
| `sudo systemctl daemon-reload`           | Reloads systemd manager configuration to recognize changes in service files. |
| `sudo systemctl enable salary-api.service` | Enables the service to start automatically on system boot.              |
| `sudo systemctl start salary-api.service` | Starts the `salary-api` service immediately.                            |
| `sudo systemctl status salary-api.service` | Displays the current status of the service, including logs and errors.  |


## Access the API
Visit the following URL in your browser:
```
http://<public-ip>:8080/salary-documentation
```

<img width="1920" height="933" alt="Screenshot from 2025-07-30 12-27-42" src="https://github.com/user-attachments/assets/f88a4e2d-cafa-4bd3-85ca-eb6eda5efd4e" />

---

# Conclusion
The Salary API, a key component of the OT-Microservices system, manages salary transactions efficiently. It leverages ScyllaDB for scalable storage, Redis for caching, Migrate for database versioning, Swagger for documentation, and Maven for builds. Designed for high performance and seamless integration, it ensures fast data access, scalability, and enterprise-grade reliability.


---

## Contact

| Name         | Email Address               |
|--------------|-----------------------------|
| Mohd Sharik  | md.sharik.snaatak@mygurukulam.co  |

---

## References

| Description                      | Link   |
|----------------------------------|--------|
| ScyllaDB Introduction documentation  | [visit](https://cloud.docs.scylladb.com/stable/scylladb-basics/) |
| ScyllaDB Installation documentation | [visit](https://docs.scylladb.com/manual/stable/getting-started/install-scylla/install-on-linux.html) |
| Redis Installation Guide           | [visit](https://redis.io/docs/latest/operate/oss_and_stack/install/archive/install-redis/install-redis-on-linux/) |
| Maven                              | [visit](https://maven.apache.org/install.html) |
