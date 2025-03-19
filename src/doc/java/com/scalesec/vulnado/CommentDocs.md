# Comment.java: Comment Management

## Overview

This program manages comments, including creating, fetching, and deleting comments. It interacts with a PostgreSQL database to store and retrieve comment data.

## Process Flow

```mermaid
flowchart TD
    A["Create Comment"] --> B["Generate UUID"]
    B --> C["Get Current Timestamp"]
    C --> D["Create Comment Object"]
    D --> E["Commit Comment to Database"]
    E --> F{"Commit Successful?"}
    F -->|Yes| G["Return Comment"]
    F -->|No| H["Throw BadRequest Exception"]
    
    I["Fetch All Comments"] --> J["Create SQL Statement"]
    J --> K["Execute Query"]
    K --> L["Iterate Result Set"]
    L --> M["Create Comment Object"]
    M --> N["Add to List"]
    N --> O["Return List of Comments"]
    
    P["Delete Comment"] --> Q["Create SQL Statement"]
    Q --> R["Set Comment ID"]
    R --> S["Execute Update"]
    S --> T{"Delete Successful?"}
    T -->|Yes| U["Return True"]
    T -->|No| V["Return False"]
```

## Insights

- The program uses UUIDs to uniquely identify comments.
- Comments are timestamped with the creation time.
- The program handles database interactions for creating, fetching, and deleting comments.
- Error handling is implemented to manage database operation failures.

## Dependencies

```mermaid
flowchart LR
    Comment --- |"Calls"| Postgres
    Comment --- |"Uses"| java_sql
    Comment --- |"Uses"| java_util
    Comment --- |"Uses"| java_util_UUID
    Comment --- |"Uses"| org_apache_catalina_Server
```

- `Postgres`: Provides database connection for executing SQL queries.
- `java.sql`: Used for SQL operations and handling SQL exceptions.
- `java.util`: Used for date and list operations.
- `java.util.UUID`: Used for generating unique identifiers for comments.
- `org.apache.catalina.Server`: Used for server-related operations.

## Data Manipulation (SQL)

### Table: comments

| Attribute   | Type      | Description                  |
|-------------|-----------|------------------------------|
| id          | String    | Unique identifier for comment|
| username    | String    | Username of the commenter    |
| body        | String    | Content of the comment       |
| created_on  | Timestamp | Timestamp of comment creation|

### SQL Operations

- **INSERT**: Adds a new comment to the `comments` table.
- **SELECT**: Retrieves all comments from the `comments` table.
- **DELETE**: Removes a comment from the `comments` table based on the comment ID.
