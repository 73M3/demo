# Documentation for `CommentsController.java`

## Overview

The `CommentsController` class is a RESTful controller implemented using the Spring Boot framework. It provides endpoints for managing comments, including fetching all comments, creating a new comment, and deleting an existing comment. The controller enforces authentication using a token-based mechanism and supports Cross-Origin Resource Sharing (CORS).

## Table of Contents

- [Class: CommentsController](#class-commentscontroller)
  - [Endpoints](#endpoints)
- [Class: CommentRequest](#class-commentrequest)
- [Class: BadRequest](#class-badrequest)
- [Class: ServerError](#class-servererror)
- [Insights](#insights)

---

## Class: `CommentsController`

The `CommentsController` class is annotated with `@RestController` and `@EnableAutoConfiguration`, making it a Spring Boot REST controller with automatic configuration. It handles HTTP requests related to comments.

### Fields

| Field Name | Type   | Description                                                                 |
|------------|--------|-----------------------------------------------------------------------------|
| `secret`   | String | A secret value injected from the application properties (`app.secret`).    |

### Endpoints

#### 1. **GET `/comments`**

- **Description**: Fetches all comments.
- **Method**: `GET`
- **Produces**: `application/json`
- **Headers**:
  - `x-auth-token` (String): Authentication token.
- **Response**: A list of `Comment` objects.
- **Authentication**: Validates the token using `User.assertAuth(secret, token)`.

#### 2. **POST `/comments`**

- **Description**: Creates a new comment.
- **Method**: `POST`
- **Produces**: `application/json`
- **Consumes**: `application/json`
- **Headers**:
  - `x-auth-token` (String): Authentication token.
- **Request Body**: A `CommentRequest` object containing the username and body of the comment.
- **Response**: The created `Comment` object.

#### 3. **DELETE `/comments/{id}`**

- **Description**: Deletes a comment by its ID.
- **Method**: `DELETE`
- **Produces**: `application/json`
- **Headers**:
  - `x-auth-token` (String): Authentication token.
- **Path Variables**:
  - `id` (String): The ID of the comment to delete.
- **Response**: A `Boolean` indicating whether the deletion was successful.

---

## Class: `CommentRequest`

The `CommentRequest` class is a simple data structure used to represent the request body for creating a comment. It implements the `Serializable` interface.

### Fields

| Field Name | Type   | Description                     |
|------------|--------|---------------------------------|
| `username` | String | The username of the commenter. |
| `body`     | String | The content of the comment.    |

---

## Class: `BadRequest`

The `BadRequest` class is a custom exception that maps to the HTTP status code `400 Bad Request`. It extends `RuntimeException`.

### Constructor

| Parameter   | Type   | Description                     |
|-------------|--------|---------------------------------|
| `exception` | String | The error message for the exception. |

---

## Class: `ServerError`

The `ServerError` class is a custom exception that maps to the HTTP status code `500 Internal Server Error`. It extends `RuntimeException`.

### Constructor

| Parameter   | Type   | Description                     |
|-------------|--------|---------------------------------|
| `exception` | String | The error message for the exception. |

---

## Insights

1. **Authentication**: The controller uses a token-based authentication mechanism. The `x-auth-token` header is required for all endpoints, and the token is validated using the `User.assertAuth(secret, token)` method.

2. **CORS Support**: All endpoints are annotated with `@CrossOrigin(origins = "*")`, allowing requests from any origin. This is useful for enabling cross-origin requests but may pose a security risk if not properly managed.

3. **Error Handling**: Custom exceptions (`BadRequest` and `ServerError`) are defined to handle specific HTTP error scenarios. These exceptions are mapped to appropriate HTTP status codes.

4. **Dependency Injection**: The `secret` field is injected using the `@Value` annotation, which retrieves the value from the application's configuration.

5. **Comment Management**: The actual logic for fetching, creating, and deleting comments is delegated to the `Comment` class. This class is not included in the provided code, but it is assumed to handle database operations or other business logic.

6. **Scalability**: The use of `@RestController` and Spring Boot's features makes the controller scalable and easy to extend with additional endpoints or functionality.
