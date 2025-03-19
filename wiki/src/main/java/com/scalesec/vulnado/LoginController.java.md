# Documentation for `LoginController.java`

## Overview

The `LoginController` class is a RESTful controller implemented using the Spring Boot framework. It provides an endpoint for user authentication by validating login credentials and returning a token upon successful authentication. The controller also handles unauthorized access by throwing a custom exception.

---

## File Metadata

- **File Name**: `LoginController.java`

---

## Components

### 1. **`LoginController` Class**
The main controller class that handles the `/login` endpoint.

#### **Attributes**
| Attribute | Type   | Description                                      |
|-----------|--------|--------------------------------------------------|
| `secret`  | String | A secret key injected from the application properties (`app.secret`). |

#### **Methods**
| Method Signature                                                                 | Description                                                                                     |
|----------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------|
| `LoginResponse login(@RequestBody LoginRequest input)`                           | Handles POST requests to the `/login` endpoint. Validates user credentials and returns a token. |

- **Endpoint Details**:
  - **Path**: `/login`
  - **HTTP Method**: `POST`
  - **Consumes**: `application/json`
  - **Produces**: `application/json`
  - **CORS**: Allows requests from all origins (`@CrossOrigin(origins = "*")`).

- **Logic**:
  1. Fetches the user details using the `User.fetch()` method based on the provided username.
  2. Hashes the input password using `Postgres.md5()` and compares it with the stored hashed password.
  3. If the credentials match, generates a token using the `user.token(secret)` method and returns it in the response.
  4. If the credentials do not match, throws an `Unauthorized` exception.

---

### 2. **`LoginRequest` Class**
A data structure representing the request body for the `/login` endpoint.

#### **Attributes**
| Attribute  | Type   | Description                     |
|------------|--------|---------------------------------|
| `username` | String | The username of the user.       |
| `password` | String | The password of the user.       |

- **Implements**: `Serializable`

---

### 3. **`LoginResponse` Class**
A data structure representing the response body for the `/login` endpoint.

#### **Attributes**
| Attribute | Type   | Description                     |
|-----------|--------|---------------------------------|
| `token`   | String | The authentication token.       |

- **Constructor**:
  - `LoginResponse(String msg)`: Initializes the `token` attribute with the provided message.

- **Implements**: `Serializable`

---

### 4. **`Unauthorized` Class**
A custom exception class used to handle unauthorized access.

#### **Attributes**
| Attribute   | Type   | Description                     |
|-------------|--------|---------------------------------|
| `exception` | String | The exception message.         |

- **Annotations**:
  - `@ResponseStatus(HttpStatus.UNAUTHORIZED)`: Maps the exception to an HTTP 401 Unauthorized status.

- **Constructor**:
  - `Unauthorized(String exception)`: Initializes the exception with a custom message.

---

## Insights

1. **Security Considerations**:
   - The `secret` key is injected from the application properties, which is a good practice for managing sensitive data.
   - The use of `Postgres.md5()` for password hashing may not be secure by modern standards. Consider using a more robust hashing algorithm like `bcrypt` or `PBKDF2`.

2. **Error Handling**:
   - The `Unauthorized` exception is used to handle invalid login attempts, ensuring a clear and consistent response for unauthorized access.

3. **CORS Policy**:
   - The `@CrossOrigin(origins = "*")` annotation allows requests from all origins. This may pose a security risk in production environments. It is recommended to restrict origins to trusted domains.

4. **Serialization**:
   - Both `LoginRequest` and `LoginResponse` implement `Serializable`, which is useful for potential object serialization needs, such as caching or distributed systems.

5. **Dependency on External Classes**:
   - The code relies on external classes (`User` and `Postgres`) for user fetching and password hashing. Ensure these classes are implemented securely and efficiently.

6. **Token Generation**:
   - The `user.token(secret)` method is used to generate the authentication token. Ensure the token generation logic is secure and follows best practices (e.g., using JWT).

---

## Summary of Endpoints

| Endpoint | HTTP Method | Request Body Type | Response Body Type | Description                     |
|----------|-------------|-------------------|--------------------|---------------------------------|
| `/login` | POST         | `LoginRequest`   | `LoginResponse`    | Authenticates a user and returns a token. |
