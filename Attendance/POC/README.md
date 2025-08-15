# Attendance API Manual Setup & Troubleshooting Guide

**Author:** Hifza Khan  
**Created:** 28-July-2025  
**Version:** v1.0

---

## Overview

This document provides step-by-step instructions to set up the attendance-api microservice **without Docker**, covering manual installation, virtual environment setup, **PostgreSQL** and **Redis** configuration, and troubleshooting during installation.

---

## 1. Install Python 3.11 & Dev Tools

```sh
sudo apt update
sudo apt-add-repository ppa:deadsnakes/ppa -y
sudo apt install python3.11 python3.11-venv python3.11-dev build-essential libpq-dev -y
```

### (Optional) Set Python 3.11 as Default

```sh
sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.11 1
sudo update-alternatives --config python3  # Select Python 3.11
```

### Install Pip3

```sh
sudo apt install python3-pip openjdk-17-jdk
```

---

## 2. Install & Configure PostgreSQL

### 2.1 Install PostgreSQL

```sh
sudo apt install postgresql postgresql-contrib -y
```

### 2.2 Start PostgreSQL

```sh
sudo service postgresql start
sudo service postgresql status
```

### 2.3 Create User and Database

```sh
sudo -u postgres psql
```
Then in the psql shell:

```sql
CREATE USER app WITH PASSWORD 'pass';
CREATE DATABASE attendance_db OWNER app;
GRANT ALL PRIVILEGES ON DATABASE attendance_db TO app;
\q
```

### 2.4 Create Records Table

```sh
sudo -u postgres psql
```
Then in the psql shell:

```sql
\c attendance_db
CREATE TABLE records (
    id SERIAL PRIMARY KEY,
    name TEXT NOT NULL,
    status TEXT NOT NULL,
    date DATE NOT NULL
);
GRANT ALL PRIVILEGES ON TABLE records TO app;
\q
```

---

## 3. Install & Configure Redis

```sh
sudo apt install redis-server -y
sudo systemctl enable redis-server
sudo systemctl start redis-server
```

### Test Redis

```sh
redis-cli ping  # Output should be: PONG
```

---

## 4. Clone the Project Repository

```sh
git clone https://github.com/OT-MICROSERVICES/attendance-api.git
cd attendance-api
```

---

## 5. Setup Python Virtual Environment

```sh
python3 -m venv attendance-api-py3.12
source attendance-api-py3.12/bin/activate
cd attendance-api
pip install flask
```

---

## 6. Install Python Dependencies

### 6.1 If requirements.txt exists:

```sh
pip install -r requirements.txt
```

### 6.2 If not, install manually:

```sh
sudo apt update
sudo apt install -y python3-dev libpq-dev build-essential
pip install flask psycopg2 flasker prometheus-flask-exporter redis pyyaml
pip install flask peewee voluptuous redis json-logging py jwtformatter python-json-logger flask-caching
pip install prometheus-flask-exporter flasker pytest-mock pytest-cov psycopg2 PyYAML
pip install --upgrade pip setuptools wheel
```

---

## 7. Install & Configure Liquibase (for migrations)

### 7.1 Install Liquibase

```sh
mkdir -p ~/liquibase
cd ~/liquibase
wget https://github.com/liquibase/liquibase/releases/download/v4.23.0/liquibase-4.23.0.tar.gz
tar -xzf liquibase-4.23.0.tar.gz
sudo mkdir -p /opt/liquibase/
sudo cp -r ~/liquibase/liquibase /usr/local/bin/liquibase
liquibase --version
```

### 7.2 Add PostgreSQL JDBC Driver

```sh
cd /opt/liquibase/lib
sudo wget https://jdbc.postgresql.org/download/postgresql-42.6.0.jar
```

### 7.3 Configure Liquibase

Create or edit the `liquibase.properties` file:

```
changeLogFile=migration/changelog.xml
url=jdbc:postgresql://localhost:5432/attendance
username=app
password=pass
driver=org.postgresql.Driver
classpath=/opt/liquibase/lib/postgresql-42.6.0.jar
```

### 7.4 Run Liquibase Migration

```sh
liquibase update
```
Run migrations:

```sh
make run-migrations
```

<img width="1804" height="2694" alt="carbon (36)" src="https://github.com/user-attachments/assets/9159282f-7c10-4ea4-bf72-59b344ae29f0" />

#### Check the DB in psql:

```sh
sudo -u postgres psql attendance
\dt
\q
```

---

## 8. Update config.yaml

Edit the `config.yaml` file:

```yaml
postgres:
  host: 127.0.0.1
  port: 5432
  user: app
  password: pass
  db: attendance_db

redis:
  host: 127.0.0.1
  port: 6379
  password: ""
```

---

## 9. Fix & Verify app.py

Make sure the last lines of `app.py` look like:

```python
app.register_blueprint(create_record, url_prefix="/api/v1")

if __name__ == "__main__":
    app.run(debug=True, host="0.0.0.0", port=5000)
```
<img width="1233" height="667" alt="image" src="https://github.com/user-attachments/assets/6987a844-85aa-4009-ab20-13557be78317" />

---

## 10. Run the Application

```sh
source attendance-api-py3.12/bin/activate
python app.py
```
<img width="2048" height="826" alt="carbon (39)" src="https://github.com/user-attachments/assets/6c9e39ea-92df-4334-9da1-f0936b9932be" />

- Should run on all addresses (0.0.0.0)
- Accessible at: http://127.0.0.1:5000/ or public IP if on cloud

---

## 11. Test the API

```sh
curl http://127.0.0.1:5000/api/v1/attendance/health
```

Or use the Swagger UI:  
http://13.62.54.155:5000/apidocs/#/attendance/post_api_v1_attendance_create

<img width="1912" height="972" alt="image" src="https://github.com/user-attachments/assets/72fd608d-5f23-4256-bd08-51dfb635c2e4" />

---

## Troubleshooting

### Common Errors & Solutions

| Error | Solution |
|-------|----------|
| ModuleNotFoundError: No module named 'flask' | Activate virtual environment with `source venv/bin/activate`, then `pip install flask` |
| redis-server not found | Install with `sudo apt install redis-server` |
| psycopg2 build errors | Run `sudo apt install libpq-dev python3-dev build-essential` before pip install |
| Config file missing | Create `config.yaml` in root folder and add DB/Redis info manually |
| App not running on 0.0.0.0 | Ensure `app.run(host="0.0.0.0", port=5000)` is set in app.py |

---

## Advanced: Liquibase Table Troubleshooting

### Option 1: Drop the table manually, then run Liquibase

1. Connect to PostgreSQL:
    ```sh
    sudo -u postgres psql attendance_db
    ```
2. Check if records table exists:
    ```sql
    \dt
    ```
3. If it exists, drop it:
    ```sql
    DROP TABLE records;
    \q
    ```
4. Re-run Liquibase migrations:
    ```sh
    cd /opt/liquibase
    make run-migrations
    ```

---

### Option 2: Skip table creation if it already exists

Edit your `changelog.xml` (inside resource/db/changelog/) as follows:

```xml
<changeSet id="*" author="Hafza">
    <preConditions onFail="MARK_RAN">
        <not>
            <tableExists tableName="records"/>
        </not>
    </preConditions>
    <createTable tableName="records">
        <column name="id" type="int">
            <constraints primaryKey="true"/>
        </column>
        <column name="name" type="varchar(255)"/>
        <column name="status" type="varchar(255)"/>
        <column name="date" type="date"/>
    </createTable>
</changeSet>
```

---

### If Still Not Working

**Delete Liquibase tracking tables** (if they're corrupt/outdated):

```sh
psql -d attendance_db
DROP TABLE databasechangelog;
DROP TABLE databasechangeloglock;
\q
```

---

## Pro Tips

- Test SQL before running in production.
- Preview Liquibase SQL:
    ```sh
    liquibase updateSQL
    ```
- Run Python in background:
    ```sh
    nohup python3 app.py > log 2>&1 &
    ```
- To kill a process:
    ```sh
    ps aux | grep python
    ```
<img width="2048" height="790" alt="carbon (42)" src="https://github.com/user-attachments/assets/a83fb510-70f5-4918-977a-2f3b32b9434e" />

---

## Notes

- **PostgreSQL** runs on port **5432**, **Redis** on **6379**
- Ensure PostgreSQL and Redis are running before starting the app.
- Always activate your Python virtual environment before using pip or python commands.
- Use Swagger or curl to verify the API is up.
