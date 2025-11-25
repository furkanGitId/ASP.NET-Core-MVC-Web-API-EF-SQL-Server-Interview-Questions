# 🚀 ASP.NET Core Web API Beginner Guide

![.NET 8](https://img.shields.io/badge/.NET-8.0-purple?style=flat&logo=dotnet)
![EF Core](https://img.shields.io/badge/EF%20Core-Entity%20Framework-blue?style=flat)
![SQL Server](https://img.shields.io/badge/SQL%20Server-Database-red?style=flat)
![Status](https://img.shields.io/badge/Status-Active-success)

A **simple, step-by-step guide** designed for beginners to learn how to build a robust **RESTful Web API** using the modern **.NET 8** framework.

This project focuses on the core concepts of API development, including:
*   **Data Persistence:** Connecting to a **SQL Server** database.
*   **Object-Relational Mapping (ORM):** Using **Entity Framework Core** with a Code-First approach.
*   **API Design:** Creating clear, protected endpoints for basic data operations (CRUD).

By following this guide, you will set up a complete backend service ready to be consumed by any frontend application.

---

## 📋 Table of Contents

- [Key Features](#-key-features)
- [Technologies Used](#-technologies-used)
- [Prerequisites](#-prerequisites)
- [Getting Started](#-getting-started)
    - [1. Project Setup](#1-project-setup)
    - [2. Database Configuration](#2-database-configuration)
    - [3. Running Migrations](#3-running-migrations)
- [API Endpoints](#-api-endpoints)
- [Project Structure](#-project-structure)
- [Docker Setup (Advanced)](#-docker-setup-advanced)
- [License](#-license)

---

## ✨ Key Features

This project is built with best practices in mind, offering the following features:

*   **Modern Framework:** Developed on the latest **ASP.NET Core 8** for high performance and reliability.
*   **Code-First Database:** Utilizes **Entity Framework Core** to manage the database schema directly from C# models.
*   **Interactive Documentation:** Integrated **Swagger UI** for easy testing and exploration of all API endpoints.
*   **Asynchronous Operations:** Implements `async/await` for non-blocking I/O, ensuring better application responsiveness.
*   **JWT Authentication:** Includes basic **JSON Web Token (JWT)**-based authentication for securing user-related endpoints.

---

## 🛠 Technologies Used

| Category | Technology | Description |
| :--- | :--- | :--- |
| **Framework** | ASP.NET Core 8 | The foundation for building the Web API. |
| **Language** | C# | The primary programming language. |
| **Database** | Microsoft SQL Server | Used for persistent data storage. |
| **ORM** | Entity Framework Core | Simplifies database interactions using C# objects. |
| **Development** | Visual Studio 2022 | Recommended IDE for a seamless development experience. |

---

## ⚙️ Prerequisites

To successfully run and modify this project, you need the following software installed:

1.  **[.NET 8 SDK](https://dotnet.microsoft.com/en-us/download/dotnet/8.0)**: The runtime and command-line tools for building .NET applications.
2.  **[Visual Studio 2022](https://visualstudio.microsoft.com/)**: Ensure you select the **"ASP.NET and web development"** workload during installation.
3.  **SQL Server**: A running instance of SQL Server (e.g., **SQL Server Express**, **LocalDB**, or a containerized version).

---

## 🚀 Getting Started

Follow these steps to get the API running on your local machine.

### 1. Project Setup

If you haven't already, clone the repository or create a new ASP.NET Core Web API project in Visual Studio 2022.

Next, install the necessary Entity Framework Core packages via the **Package Manager Console** in Visual Studio:

```powershell
# Install the SQL Server provider and the tools package
Install-Package Microsoft.EntityFrameworkCore.SqlServer
Install-Package Microsoft.EntityFrameworkCore.Tools
```

### 2. Database Configuration

The database connection string and JWT settings are stored in `appsettings.json`.

Open `appsettings.json` and update the `DefaultConnection` to point to your local SQL Server instance.

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=YOUR_SERVER_INSTANCE;Database=AspNetCoreWebAPI;Trusted_Connection=True;TrustServerCertificate=True;"
  },
  "Jwt": {
    "Key": "X8v!pQz$7s@Lk#2bT9wR^eF6uHjZxC1m",
    "Issuer": "AspNetCoreWebAPIBackend",
    "Audience": "AngularFrontend"
  }
}
```

> **Note:** Replace `YOUR_SERVER_INSTANCE` with your SQL Server name (e.g., `(localdb)\\MSSQLLocalDB` for a default LocalDB instance).

### 3. Running Migrations

Entity Framework Core uses migrations to create and update the database schema. Run the following commands in the **Package Manager Console** to create the database and tables:

```powershell
# 1. Create the initial migration file
Add-Migration InitialCreate

# 2. Apply the migration to create the database and tables
Update-Database
```

Your database is now set up! You can run the project by pressing **F5** in Visual Studio. The browser will automatically open the **Swagger UI** for testing.

---

## 📡 API Endpoints

The API provides two main controllers: one for authentication and one for managing user data.

### Authentication Endpoint (`LoginsController`)

| Method | Endpoint | Description | Request Body Example |
| :--- | :--- | :--- | :--- |
| `POST` | `/api/login` | Validates credentials and returns a **JSON Web Token (JWT)**. This token is required for all protected endpoints. | ```json\n{\n  "username": "admin",\n  "password": "admin"\n}\n``` |

### User Management Endpoints (`UsersController`)

These endpoints are **protected** and require an `Authorization: Bearer <token>` header, where `<token>` is the JWT obtained from the `/api/login` endpoint.

| Method | Endpoint | Description | Request/Response Body Example |
| :--- | :--- | :--- | :--- |
| `GET` | `/api/user` | Retrieves a list of all user records. | *(N/A for GET)* |
| `GET` | `/api/user/{id}` | Retrieves a single user by their ID. | *(N/A for GET)* |
| `POST` | `/api/user` | Creates a new user record. | ```json\n{\n  "name": "Jane Doe",\n  "email": "jane.doe@example.com"\n}\n``` |
| `PUT` | `/api/user/{id}` | Updates an existing user record. | *(Same as POST body)* |
| `DELETE` | `/api/user/{id}` | Deletes a user record by their ID. | *(N/A for DELETE)* |

---

## 📂 Project Structure

This is a high-level overview of the project's key directories and files:

```text
AspNetCoreWebAPIBackend_AngularFrontend_MSSQLDatabase/
├── Controllers/
│   ├── LoginsController.cs       # Handles user login and JWT token issuance.
│   └── UsersController.cs        # Contains the protected CRUD operations for users.
├── Models/
│   ├── ApplicationDbContext.cs   # The central class for interacting with the database (DbSet definitions).
│   ├── login.cs                  # The C# model representing a login record in the database.
│   └── user.cs                   # The C# model representing a user record in the database.
├── Migrations/                   # Automatically generated history of database schema changes.
├── Program.cs                    # The application's entry point, responsible for configuration and startup.
└── appsettings.json              # Configuration file for connection strings and JWT settings.
```

---

## 🐳 Docker Setup (Advanced)

For developers looking to run the entire stack (API, Database, and a potential Frontend) in isolated containers, Docker is the recommended approach.

### 1. Prerequisites
Ensure you have [Docker Desktop](https://www.docker.com/products/docker-desktop/) installed and running.

### 2. Environment Variables
Create a file named `.env` in the root directory to manage sensitive configuration:

```env
DB_PASSWORD=12345
DB_USER=sa
DB_NAME=AspNetCoreWebAPI_DemoDB
JWT_KEY=X8v!pQz$7s@Lk#2bT9wR^eF6uHjZxC1m
JWT_ISSUER=AspNetCoreWebAPIBackend
JWT_AUDIENCE=AngularFrontend
SA_PASSWORD=12345
```

### 3. Run with Docker Compose
Use `docker-compose` to build and start all services simultaneously:

```bash
docker-compose up --build
```

### Accessing the Services

| Service | URL/Address | Credentials |
| :--- | :--- | :--- |
| **Backend API** | `http://localhost:5000/swagger` | *(N/A)* |
| **SQL Server** | `localhost,1433` | User: `sa`, Password: `12345` |
| **Frontend** | `http://localhost:4200` | *(If included in the compose file)* |

---

## 🔮 Roadmap

Future improvements planned for this project:

- [ ] Add robust password hashing and salt generation for security.
- [ ] Implement unit and integration tests for core functionality.
- [ ] Enhance Swagger UI to support JWT authentication directly.
- [x] Docker Support (Completed).

---

## 📄 License

This project is licensed under the MIT License - see the LICENSE.md file for details.
