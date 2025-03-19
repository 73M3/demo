# Documentation: `Comment.java`

## Overview
The `Comment` class is part of the `com.scalesec.vulnado` package and provides functionality for managing comments in a database. It includes methods for creating, fetching, and deleting comments, as well as persisting them to a PostgreSQL database. The class interacts with the database using JDBC.

---

## Class: `Comment`

### Attributes
| Attribute Name | Type       | Description                                      |
|----------------|------------|--------------------------------------------------|
| `id`           | `String`   | Unique identifier for the comment.              |
| `username`     | `String`   | Username of the user who created the comment.   |
| `body`         | `String`   | Content of the comment.                         |
| `created_on`   | `Timestamp`| Timestamp indicating when the comment was created. |

### Constructor
```java
public Comment(String id, String username, String body, Timestamp created_on)
```
- **Parameters**:
  - `id`: Unique identifier for the comment.
  - `username`: Username of the user who created the comment.
  - `body`: Content of the comment.
  - `created_on`: Timestamp indicating when the comment was created.
- **Purpose**: Initializes a `Comment` object with the provided attributes.

---

## Methods

### `create(String username, String body)`
- **Description**: Creates a new comment and saves it to the database.
- **Parameters**:
  - `username`: Username of the user creating the comment.
  - `body`: Content of the comment.
- **Returns**: A `Comment` object if successfully saved; throws exceptions otherwise.
- **Exceptions**:
  - `BadRequest`: Thrown if the comment cannot be saved.
  - `ServerError`: Thrown if an unexpected error occurs during the save operation.
- **Logic**:
  - Generates a unique ID using `UUID`.
  - Sets the current timestamp as the creation time.
  - Calls the `commit()` method to persist the comment to the database.

---

### `fetch_all()`
- **Description**: Retrieves all comments from the database.
- **Returns**: A `List<Comment>` containing all comments.
- **Logic**:
  - Executes a SQL query (`SELECT * FROM comments`) to fetch all records.
  - Maps each record to a `Comment` object and adds it to the list.
  - Handles exceptions by printing stack traces and error messages.

---

### `delete(String id)`
- **Description**: Deletes a comment from the database based on its ID.
- **Parameters**:
  - `id`: The unique identifier of the comment to be deleted.
- **Returns**: `true` if the deletion is successful; `false` otherwise.
- **Logic**:
  - Executes a SQL query (`DELETE FROM comments WHERE id = ?`) to delete the comment.
  - Uses a prepared statement to prevent SQL injection.

---

### `commit()`
- **Access**: Private
- **Description**: Persists the current `Comment` object to the database.
- **Returns**: `true` if the insertion is successful; `false` otherwise.
- **Logic**:
  - Executes a SQL query (`INSERT INTO comments (id, username, body, created_on) VALUES (?,?,?,?)`) to insert the comment.
  - Uses a prepared statement to safely insert values into the database.

---

## Insights

### Database Interaction
- The class heavily relies on JDBC for database operations. It uses `Connection`, `Statement`, and `PreparedStatement` objects to interact with a PostgreSQL database.
- The `Postgres.connection()` method is assumed to provide a valid database connection. This method is not defined in the class, so it must exist elsewhere in the project.

### Error Handling
- Error handling is implemented using `try-catch` blocks. However, the `delete` method has a flaw: it always returns `false` due to the `finally` block overriding the return value. This should be corrected to ensure proper functionality.

### Security Concerns
- The use of prepared statements in `delete` and `commit` methods mitigates SQL injection risks.
- However, the `fetch_all` method uses a plain SQL query (`SELECT * FROM comments`) without parameterization, which could be vulnerable to SQL injection if user input is incorporated into the query in the future.

### Scalability
- The `fetch_all` method retrieves all comments without pagination or filtering, which could lead to performance issues if the database contains a large number of records.

### Exception Classes
- The class references custom exceptions (`BadRequest` and `ServerError`) that are not defined within the provided code. These exceptions must be implemented elsewhere in the project.

### UUID for Unique Identification
- The use of `UUID` ensures that each comment has a globally unique identifier, which is suitable for distributed systems.

### Timestamp Usage
- The `created_on` attribute uses `Timestamp` to store the creation time, which is appropriate for tracking temporal data.

---

## Potential Improvements
1. **Pagination**: Add support for pagination in the `fetch_all` method to handle large datasets efficiently.
2. **Error Handling**: Fix the `finally` block in the `delete` method to ensure the correct return value.
3. **Validation**: Implement input validation for `username` and `body` in the `create` method to ensure data integrity.
4. **Logging**: Replace `System.err.println` with a proper logging framework for better error tracking and debugging.
