SQL Handler Module
=================

Documentation
-------------

The ``SqlHandler`` class manages MySQL database connections and CRUD operations for the application. It loads SQL queries from a JSON file, executes parameterized queries, and handles database connection lifecycle. Designed for flexibility and security, it prevents SQL injection by using prepared statements.

Usage – What Does the Software Do?
----------------------------------

- **Database Connection**:  
  Connects to MySQL/MariaDB using provided credentials (default: localhost:3306).

- **Query Execution**:  
  Executes SELECT, INSERT, UPDATE, and DELETE operations using predefined queries.

- **Query Management**:  
  Loads SQL queries from ``queries.json``, enabling easy modification without code changes.

- **Connection Management**:  
  Handles connection pooling and cleanup via ``close()``.

Typical usage flow:
1. Initialize ``SqlHandler`` (auto-connects to DB).
2. Call CRUD methods (select/insert/update/delete) with query keys and parameters.
3. Close connection when done via ``close()``.

Maintenance – How Does the Software Do What It Does?
----------------------------------------------------

**Architecture**::

    +-------------+       +-----------------+
    | Application | <---> |   SqlHandler    |
    +-------------+       +--------+--------+
                                   |
                          +--------v--------+
                          | MySQL Database |
                          +-----------------+

**Key Components**:
- **MySqlConnection**: Manages database connections from the `mysql1` package.
- **Query Loader**: Loads SQL queries from JSON file at initialization.
- **Parameterized Queries**: All methods use prepared statements for security.

**Data Flow**:
1. **Initialization**:
   - Connect to database using ``_connect()``
   - Load queries from JSON via ``_loadQueries()``

2. **CRUD Operations**:
   - Method called with query key and parameters
   - Query retrieved from loaded JSON
   - Executed as prepared statement
   - Results returned as List/Map or affected row count

3. **Error Handling**:
   - Catches and logs database errors
   - Returns empty results on failure (prevents app crashes)

Best Practices
--------------

1. **Query Management**:
   - Store all SQL in ``queries.json``
   - Use descriptive query keys (e.g., "get_active_users")

2. **Security**:
   - Always use parameterized queries
   - Never concatenate user input into SQL

3. **Performance**:
   - Reuse SqlHandler instance where possible
   - Close connections when done

4. **Error Handling**:
   - Wrap calls in try/catch blocks
   - Handle empty results gracefully

Future Improvements
-------------------

- Add connection pooling
- Support transactions
- Add query validation
- Implement connection retry logic
- Add migration support

Dependencies
------------

- ``mysql1``: MySQL database driver
- ``queries.json``: File containing SQL queries
- ``dart:io``: File system access for loading queries
