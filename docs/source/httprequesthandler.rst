HttpRequestHandler Module
========================

Documentation
-------------

The `HttpRequestHandler` class is responsible for all HTTP and media-related backend communication in the application. It supports authenticated requests, handles JSON and multipart/form-data, manages file downloads and caching, and provides connection health checks. It is designed to work with both simple data and file uploads/downloads, and integrates with the app’s authentication system.

Usage – What Does the Software Do?
----------------------------------

- **Send Data**:  
  Handles POST requests to backend endpoints, supporting both JSON and multipart/form-data (for file uploads such as images, videos, or audio).
- **Fetch Data**:  
  Performs authenticated GET requests to fetch data from backend endpoints, with optional query parameters.
- **Download Media**:  
  Downloads media files, caches them in the device’s temp directory, and returns them as `XFile` objects for further use in the app.
- **Sign In**:  
  Sends user credentials to the backend, retrieves and stores authentication tokens and user IDs.
- **Connection Check**:  
  Verifies connectivity to the backend server using a health-check endpoint.

Typical usage flow:
1. Instantiate with backend address and an `AuthManager` instance.
2. Use `sendData` for POST requests (including file uploads).
3. Use `fetchData` for GET requests with optional query parameters.
4. Use `saveMedia` to download and cache media files.
5. Use `signInRequest` to authenticate users.
6. Use `checkConnection` to verify backend availability.

Maintenance – How Does the Software Do What It Does?
----------------------------------------------------

**Architecture Overview**::

    +----------------------+
    |  AuthManager         |
    +----------+-----------+
               |
    +----------v-----------+
    |  HttpRequestHandler  |
    +----------+-----------+
               |
    +----------v-----------+
    |   Backend Server     |
    +----------------------+

**Key Components**:
- **AuthManager**: Provides authentication tokens and manages user session data.
- **HttpClient/IOClient**: Used for all HTTP requests, configured to accept self-signed certificates for development.
- **http.MultipartRequest**: Used for file uploads (multipart/form-data).
- **File Caching**: Downloads and caches media files in the device’s temporary directory.

**Data Flow**:
- **sendData**:  
  - Checks if any value in the data map is an `XFile` (file upload).
  - If so, constructs a `MultipartRequest`, adds files and fields, and sends using an authenticated client.
  - Otherwise, sends a JSON POST request.
  - Parses and returns the JSON response.
- **fetchData**:  
  - Constructs a GET request with optional query parameters.
  - Adds the authentication token to the headers.
  - Parses and returns the JSON response.
- **saveMedia**:  
  - Checks if the requested media file is already cached.
  - If not, downloads the file, determines the correct extension, saves it, and returns an `XFile`.
- **signInRequest**:  
  - Sends username and password as JSON.
  - On success, saves the received token and user ID via `AuthManager`.
- **checkConnection**:  
  - Sends a GET request to a `/ping` endpoint to verify connectivity.

Best Practices
--------------

- Always use authentication tokens for secure endpoints.
- Use multipart/form-data for file uploads; JSON for all other data.
- Cache downloaded media to reduce redundant network requests.
- Handle exceptions and provide clear error messages for all network operations.
- Dispose of the `HttpClient` properly if you add a close method in the future.

Future Improvements
-------------------

- Add support for PATCH, PUT, and DELETE HTTP methods.
- Implement retry logic for failed requests.
- Enhance error reporting and logging.
- Add progress indicators for large file uploads/downloads.
- Support for custom headers and timeout configuration.

Dependencies
------------

- `dart:io`: For low-level HTTP and file operations.
- `http`, `http_parser`, `http/io_client.dart`: For HTTP requests and multipart handling.
- `path`, `path_provider`: For file path operations and caching.
- `image_picker`: For handling media files.
- `AuthManager`: For authentication and session management.


