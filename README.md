# Explanation of the Docker Compose File

This `docker-compose.yml` file is designed to run an application and an SQL Server database together using **Docker Compose**. Below is a detailed explanation of its contents:

---

## **1. `version: '3.4'`**
Specifies the version of the **Docker Compose file format** being used. Version `3.4` supports modern and advanced features.

---

## **2. `services`**
Defines the services that will run as part of this application. This file includes two services:
- **dockercompose** (the application)
- **sqlserver** (the database)

---

### **dockercompose Service**
- **`build`**:
  - **`context: ./DockerCompose`**: Specifies the directory containing the `Dockerfile` used to build the Docker image.
  - **`dockerfile: Dockerfile`**: Points to the `Dockerfile` to be used for building the image.
- **`ports`**:
  - **`8080:8080`** and **`8081:8081`**: Maps container ports 8080 and 8081 to the host machine's ports. This makes the application accessible on these ports.
- **`depends_on`**:
  - **`sqlserver`**: Indicates that this service depends on the SQL Server container and will start only after the SQL Server container is ready.
- **`environment`**:
  - **`ASPNETCORE_ENVIRONMENT=Development`**: Sets the application environment to **Development**.
- **`networks`**:
  - **`productnetwork`**: Connects the service to the `productnetwork` Docker network, enabling communication with other services in the same network.
- **`restart: on-failure`**:
  - Automatically restarts the container if it stops due to an error.

---

### **sqlserver Service**
- **`image`**:
  - **`mcr.microsoft.com/mssql/server:2022-latest`**: Uses the latest Microsoft SQL Server Docker image for the 2022 version.
- **`environment`**:
  - **`SA_PASSWORD: "Password12345!"`**: Sets the password for the SQL Server system administrator (**SA**). A strong password is required.
  - **`ACCEPT_EULA: "Y"`**: Accepts the SQL Server End-User License Agreement (mandatory).
- **`ports`**:
  - **`1433:1433`**: Maps the container's SQL Server port (1433) to the same port on the host machine. This allows database connections from the host.
- **`volumes`**:
  - **`sqlvolume:/var/opt/mssql`**: Attaches a persistent volume for storing SQL Server data. This ensures data is retained even if the container is restarted or removed.
- **`networks`**:
  - **`productnetwork`**: Connects the service to the `productnetwork` for seamless communication with other services.

---

## **3. `networks`**
- **`productnetwork`**: Creates a custom Docker network to facilitate communication between the `dockercompose` and `sqlserver` services.

---

## **4. `volumes`**
- **`sqlvolume`**: Defines a persistent Docker volume for storing SQL Server data. This ensures that database files are retained outside the container's lifecycle.

---

## **Purpose of This File**
This configuration file sets up an environment to run an **ASP.NET Core application** and **SQL Server** together:
- The application runs in **Development mode** and communicates with the database.
- SQL Server stores its data in a persistent volume to avoid data loss during container restarts.
- Both services are connected via a shared network, and their ports are exposed for external access.

![Screenshot 6](https://github.com/ilkersatur/DockerCompose-NetApi-SqlServer/blob/main/DockerCompose/docker.jpg)
