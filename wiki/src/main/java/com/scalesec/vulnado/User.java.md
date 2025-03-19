# Documentation: `User.java`

## Overview
The `User` class is part of the `com.scalesec.vulnado` package and provides functionality for user management, including token generation, authentication, and database interaction. It encapsulates user-related data and operations, such as fetching user details from a database and verifying authentication tokens.

---

## Class: `User`

### Fields
| Field Name      | Type   | Description                                      |
|------------------|--------|--------------------------------------------------|
| `id`            | String | Unique identifier for the user.                 |
| `username`      | String | Username of the user.                           |
| `hashedPassword`| String | Hashed password of the user.                    |

### Constructor
#### `User(String id, String username, String hashedPassword)`
Initializes a new `User` object with the provided `id`, `username`, and `hashedPassword`.

---

### Methods

#### `String token(String secret)`
Generates a JSON Web Token (JWT) for the user using the provided secret key.

- **Parameters**:
  - `secret`: A string used as the secret key for signing the JWT.
- **Returns**:
  - A signed JWT containing the user's `username` as the subject.
- **Implementation Details**:
  - Uses the `io.jsonwebtoken` library to create and sign the token.
  - The secret key is derived using `Keys.hmacShaKeyFor`.

---

#### `static void assertAuth(String secret, String token)`
Validates the provided JWT token using the given secret key.

- **Parameters**:
  - `secret`: A string used as the secret key for verifying the JWT.
  - `token`: The JWT to be validated.
- **Throws**:
  - `Unauthorized`: If the token is invalid or verification fails.
- **Implementation Details**:
  - Uses the `io.jsonwebtoken` library to parse and validate the token.
  - Exceptions during validation are caught, logged, and rethrown as `Unauthorized`.

---

#### `static User fetch(String un)`
Fetches a user from the database based on the provided username.

- **Parameters**:
  - `un`: The username of the user to fetch.
- **Returns**:
  - A `User` object populated with data from the database, or `null` if no user is found.
- **Implementation Details**:
  - Establishes a connection to the database using `Postgres.connection()`.
  - Executes a SQL query to retrieve user details.
  - Constructs a `User` object from the query result.
  - Closes the database connection after execution.
- **Potential Issues**:
  - SQL query is vulnerable to SQL injection due to direct concatenation of user input (`un`) into the query string.

---

## Insights

### Security Concerns
1. **SQL Injection Vulnerability**:
   - The `fetch` method directly concatenates the `username` parameter into the SQL query string, making it susceptible to SQL injection attacks. Use prepared statements to mitigate this risk.

2. **Hardcoded Secret Key Handling**:
   - The `token` and `assertAuth` methods rely on a secret key passed as a string. Ensure the secret is securely stored and managed (e.g., environment variables or a secure vault).

3. **Exception Handling**:
   - The `assertAuth` method logs exceptions and rethrows them as `Unauthorized`. Ensure sensitive information is not exposed in logs.

### Best Practices
- **Database Interaction**:
  - Replace the direct SQL query in `fetch` with a parameterized query to prevent SQL injection.
  
- **Token Management**:
  - Use a robust key management system for handling secrets used in token generation and validation.

- **Error Handling**:
  - Avoid printing stack traces directly to the console in production environments. Use a logging framework with appropriate log levels.

### Dependencies
- **Libraries Used**:
  - `io.jsonwebtoken`: For JWT creation and validation.
  - `javax.crypto`: For cryptographic operations.
- **External Classes**:
  - `Postgres`: Assumed to provide database connection functionality.

### Potential Enhancements
- Implement input validation for the `username` parameter in the `fetch` method.
- Add unit tests for token generation, validation, and database interaction.
- Introduce a configuration mechanism for managing secrets securely.
