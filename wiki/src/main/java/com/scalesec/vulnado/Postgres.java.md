# Documentation: `Postgres.java`

## Overview

The `Postgres` class provides functionality to interact with a PostgreSQL database. It includes methods for establishing a database connection, setting up the database schema, inserting seed data, and performing basic operations such as inserting users and comments. Additionally, it includes a utility method for generating MD5 hashes.

---

## Features

### 1. **Database Connection**
   - Establishes a connection to a PostgreSQL database using environment variables:
     - `PGHOST`: Hostname of the PostgreSQL server.
     - `PGDATABASE`: Name of the database.
     - `PGUSER`: Username for authentication.
     - `PGPASSWORD`: Password for authentication.
   - Uses the `DriverManager` class to create the connection.

### 2. **Database Setup**
   - Creates two tables if they do not already exist:
     - **`users`**: Stores user information.
     - **`comments`**: Stores user comments.
   - Cleans up any existing data in the tables.
   - Inserts predefined seed data for users and comments.

### 3. **MD5 Hashing**
   - Provides a utility method to generate an MD5 hash for a given input string. This is used to hash user passwords before storing them in the database.

### 4. **Data Insertion**
   - **Users**: Inserts user data into the `users` table with hashed passwords.
   - **Comments**: Inserts comment data into the `comments` table.

---

## Class Details

### **1. Methods**

| Method Name         | Description                                                                                     |
|---------------------|-------------------------------------------------------------------------------------------------|
| `connection()`      | Establishes and returns a connection to the PostgreSQL database.                                |
| `setup()`           | Sets up the database schema, cleans up existing data, and inserts seed data.                   |
| `md5(String input)` | Generates an MD5 hash for the given input string.                                               |
| `insertUser(String username, String password)` | Inserts a new user into the `users` table with a hashed password.   |
| `insertComment(String username, String body)` | Inserts a new comment into the `comments` table.                     |

---

### **2. Database Schema**

#### **`users` Table**
| Column Name  | Data Type      | Constraints                     |
|--------------|----------------|----------------------------------|
| `user_id`    | `VARCHAR(36)`  | Primary Key                     |
| `username`   | `VARCHAR(50)`  | Unique, Not Null                |
| `password`   | `VARCHAR(50)`  | Not Null                        |
| `created_on` | `TIMESTAMP`    | Not Null                        |
| `last_login` | `TIMESTAMP`    | Nullable                        |

#### **`comments` Table**
| Column Name  | Data Type      | Constraints                     |
|--------------|----------------|----------------------------------|
| `id`         | `VARCHAR(36)`  | Primary Key                     |
| `username`   | `VARCHAR(36)`  | Foreign Key (to `users.username`) |
| `body`       | `VARCHAR(500)` | None                            |
| `created_on` | `TIMESTAMP`    | Not Null                        |

---

### **3. Seed Data**

#### **Users**
| Username | Password (Plaintext)       |
|----------|----------------------------|
| `admin`  | `!!SuperSecretAdmin!!`     |
| `alice`  | `AlicePassword!`           |
| `bob`    | `BobPassword!`             |
| `eve`    | `$EVELknev^l`              |
| `rick`   | `!GetSchwifty!`            |

#### **Comments**
| Username | Comment         |
|----------|-----------------|
| `rick`   | `cool dog m8`   |
| `alice`  | `OMG so cute!`  |

---

## Insights

1. **Security Concerns**:
   - **Password Hashing**: The use of MD5 for password hashing is insecure and outdated. Consider using a stronger hashing algorithm like `bcrypt` or `PBKDF2`.
   - **Environment Variables**: Sensitive database credentials are retrieved from environment variables, which is a good practice. Ensure these variables are securely managed.

2. **Error Handling**:
   - The code uses `e.printStackTrace()` for error handling, which is not suitable for production environments. Consider using a logging framework like SLF4J or Log4j for better error tracking.

3. **Database Connection Management**:
   - Connections are not managed using a connection pool, which could lead to performance issues in high-load scenarios. Consider using a library like HikariCP for efficient connection pooling.

4. **SQL Injection**:
   - The use of `PreparedStatement` for SQL queries helps mitigate SQL injection risks.

5. **Scalability**:
   - The current implementation clears and reseeds the database during setup, which is suitable for development but not for production. Consider separating schema creation and data seeding into different processes.

6. **UUID Usage**:
   - The use of `UUID` for primary keys ensures uniqueness and avoids potential conflicts in distributed systems.

7. **Timestamp Management**:
   - The `created_on` column is automatically populated with the current timestamp, which simplifies record creation.

---

## File Metadata

| Key         | Value          |
|-------------|----------------|
| **File Name** | `Postgres.java` |
