# Employee-API POC
| Created     |    Version   | Author | Comment | Reviewer |
|:------------------:|:-------------:|:-------------:|:-------------:|:------------------:|
| 13-11-2024   | V1   | Radha | Initial Commit |  |
| 19-11-2024   | V2   | Radha | L0 | Aakash Tripathi |  
| 21-11-2024   | V2   | Radha | L1 | Deepak Nishad |   
| 02-11-2024   | V2   | Radha | L1 | Anjali |          


## Table of contents
- [Introduction](#Introduction)
- [Prerequisites](#Prerequisites)
- [System Requirements](#system-requirements)
- [Important Ports](#important-ports)
- [Dependencies](#Dependencies)
- [Step by step installation](#step-by-step-installation)
- [Changes needed to be done in cloned files](#changes-needed-to-be-done-in-cloned-files)
- [Create keyspace inside ScyllaDB](#create-keyspace-inside-scylladb)
- [Service file](#run-api-in-the-background-with-service-file)
- [Endpoints Information](#endpoints-information)
- [Contact Information](#contact-information)
- [Reference Links](#reference-links)
  
## Introduction

Welcome to the Employee-API POC. Employee REST API is a golang based microservice which is responsible for all the employee related transactions in the OT-Microservices. This application is completely platform independent and can be run on any kind of platform.



## Prerequisites

The application doesn't have any specific pre-requisites except the database connectivity. Additionally, we can add Redis as cache system but it's not part of the mandatory setup and Migrate to setup table in ScyllaDB.

- [ScyllaDB](https://opensource.docs.scylladb.com/stable/getting-started/install-scylla/install-on-linux.html)
- [Redis](https://redis.io/docs/latest/operate/oss_and_stack/install/install-redis/install-redis-on-linux/)
- [Migrate](https://www.geeksforgeeks.org/how-to-install-golang-migrate-on-ubuntu/)

## System Requirements

| System requirement | Minimum Requirement  |
|:-----------------------:|:--------------------:|
| Operating System        | Ubuntu 22.04         |
| Disk space            | 20 GB    |
| RAM                 | 4 GB|
| Instance Type        | t2.medium|

## Important Ports

| Port No | Description | Traffic | 
|:----:|:----:| :----: |
|8080 | Employee API port | Inbound |
|22   | SSH | Inbound | 
|9042 | ScyllaDB | Inbound  |
|6379 | Redis | Inbound |

## Dependencies
### Buildtime Dependency
| Buildtime  Dependency  | version |
|:-----------------------:|:--------------------:|
| Golang | 1.20 |
| Migrate | 4.17 |

### Runtime Dependency
| Runtime Dependency | version |
|:-----------------------:|:--------------------:|
| ScyllaDB | 5.4.6-0.20240418.10f137e367e3 |
| Redis | 5.0.7 |

 
## Step by step installation
### Clone git repo  
 ```
git clone https://github.com/OT-MICROSERVICES/employee-api.git
```

![image](https://github.com/user-attachments/assets/357783da-0d37-47d1-a9e7-5b59d6448343)


### Installing Pre-Requisites
#### Steps to install ScyllaDB:

- **Follow the given link of documentation to install ScyllaDB**

[To Install ScyllaDB](https://opensource.docs.scylladb.com/stable/getting-started/install-scylla/install-on-linux.html)

     
#### Steps to install Migrate:
  ```  
Install the migration tool:
curl -L https://github.com/golang-migrate/migrate/releases/download/v4.15.2/migrate.linux-amd64.tar.gz | tar xvz  
sudo mv migrate /usr/local/bin/migrate  
migrate --version
  ```
Update the IP address in the migration.json file in the employee-api directory.

- Run migration commands:
```
make run-migrations
```
    
#### Steps to install Redis:
- **Follow the given link of documentation to install Redis**

[To Install Redis](https://redis.io/docs/latest/operate/oss_and_stack/install/install-stack/)

    
### Additional command that we can install (if not already present)
#### Install make
```
 sudo apt install make
```
#### Install JQ
```
sudo apt install jq
```
     
#### Install Golang
- **To download the Go programming language's version 1.21.6 binary distribution** 
     ```
     curl -OL https://golang.org/dl/go1.21.6.linux-amd64.tar.gz
     ```
     ![image](https://github.com/user-attachments/assets/c471300f-cec8-4894-81a0-d83819842fe8)


- **To verify the integrity of the file you downloaded**
     ```
     sha256sum go1.21.6.linux-amd64.tar.gz
     ```
- **To extract the Go programming language binary distribution into the /usr/local directory**
     ```
     sudo tar -C /usr/local -xvf go1.21.6.linux-amd64.tar.gz
     ```
 - **To configure the user's environment upon login**
     ```
     sudo vi ~/.profile
     ```
 - **Add the following information to the end of your file**
     ```
      export PATH=$PATH:/usr/local/go/bin
     ```
     ![image](https://github.com/user-attachments/assets/d85e8a14-d7dd-4699-882a-5c071040e93e)

- **Apply the changes**
     ```
     source ~/.profile
     ```
 
## Changes needed to be done in cloned files
#### Changes to be made in ```config.yaml```
   1. Edit Host IP add Localhost IP

![image](https://github.com/user-attachments/assets/4a4dd25f-8ebf-4712-9336-a1b00685471e)


#### Changes to be made in ```migration.json```
  Edit IP add Localhost IP
  
![image](https://github.com/user-attachments/assets/2ae5eb31-dd02-423f-b0d1-9619a4d7dbd5)

    
### Create keyspace inside ScyllaDB
- **To connect with Cassandra database instance**
   ```
   cqlsh
  ```
  ![image](https://github.com/user-attachments/assets/1b45c55d-edb0-4eaf-b520-145f152143e2)


- **creates a keyspace named employee_db**
    ```
    CREATE KEYSPACE IF NOT EXISTS employee_db
    WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 1};
    ```
### Execute commands mention in make file
```
make run-migrations
```
This will excute command mention in make file. Which will automates the process of updating the database schema by applying new changes defined in migration scripts.

```
 make build
```
This will excute command mention in make file under build. which compile the Go source code in the current directory and produce an executable file named employee-api.

## We can run application by executing employee-api script:
   ```
   ./employee-api
   ```
![image](https://github.com/user-attachments/assets/7566e22d-ace9-40f2-88e9-c46db2307f63)



## Endpoints Information

| Endpoint | Method | Description |
|----------|--------| ----------- |
| /metrics | GET    | Application healthcheck and performance metrics are available on this endpoint |
| /api/v1/employee/health| GET    | Endpoint for providing shallow healthcheck information about application health and readiness |
| /api/v1/employee/health/detail | GET    | Endpoint for providing detailed health check about dependencies as well like - ScyllaDB and Redis |
| /api/v1/employee/create | POST   | Data creation endpoint which accepts certain JSON body to add employee information in database |
| /api/v1/employee/search | GET    | Endpoint for searching data information using the params in the URL |
| /api/v1/employee/search/all | GET    | Endpoint for searching all information across the system |
| /api/v1/employee/search/location | GET    | Application endpoint for getting the count and information of location |
| /api/v1/employee/search/designation | GET    | Application endpoint for getting the count and information of designation |
| /swagger/index.html | GET    | Swagger endpoint for the application API documentation and their data models |

## Run Api in the background with service file
* create a service for api
  
  ```sh
  sudo vim /etc/systemd/system/employee.service
  ```
  ```sh
  [Unit]
  Description=Employee Service
  After=network.target

  [Service]
  User=ubuntu
  Group=ubuntu
  WorkingDirectory=/home/ubuntu/application/employee-api
  Environment=GIN_MODE=release
  ExecStart=/home/ubuntu/application/employee-api/employee-api
  Restart=always
  RestartSec=3
 
  [Install]
  WantedBy=multi-user.target
  ```
## Contact Information

| Name| Email Address      |
|-----|--------------------------|
| Radha Gondchor |radha.gondchor.snaatak@mygurukulam.co | 

## Reference Links

| Links | Description      |
|-----  |--------------------------|
| https://github.com/OT-MICROSERVICES/documentation-template/wiki/Application-Template | documentation-template | 
| https://github.com/OT-MICROSERVICES/employee-api/tree/main?tab=readme-ov-file | Employee api |




