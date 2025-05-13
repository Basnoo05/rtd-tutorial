Server Backend Module
=====================

Documentation
-------------

The `Server` class is the main backend entry point for the application. It handles HTTPS connections, user authentication, post and event management, media serving, and real-time features via WebSockets. The backend integrates with a SQL database for persistent data storage and uses secure token-based authentication for all protected endpoints.

Usage – What Does the Software Do?
----------------------------------

- **User Authentication**:  
  Handles sign-in (`/signin`) and sign-up (`/signup`) endpoints, validating credentials using bcrypt and issuing tokens/UUIDs.

- **Post/Event Management**:  
  Handles creation of text, media, and event posts via `/textPost`, `/mediaPost`, and `/eventPost` endpoints (POST). Supports following and unfollowing users.

- **Feed and Profile Retrieval**:  
  Provides endpoints for retrieving user feeds (`/feed`), profiles (`/profile`), subjects (`/subjects`), and participant counts (`/getparticipantscount`).

- **Media Serving**:  
  Serves media files (profile images, posts, events) via HTTPS GET requests.

- **Suggestions/Search**:  
  Provides user suggestions for search queries via `/getUserSuggestionsFromName`.

- **WebSocket/Real-time**:  
  Upgrades HTTP connections to WebSocket for real-time chat and notifications.

- **Security**:  
  All endpoints except sign-in/up require a valid token in the `Authorization` header. SSL/TLS is enforced with a self-signed certificate.

Typical usage flow:
1. Client connects via HTTPS and authenticates (sign-in/sign-up).
2. Client performs CRUD operations (posts, events, follows, etc.) via authenticated requests.
3. Client retrieves media and feed data as needed.
4. Real-time features use WebSocket connections after authentication.

Maintenance – How Does the Software Do What It Does?
----------------------------------------------------

**Architecture Overview**::

    +-----------+      +-------------------+      +-------------+
    |   Client  | <--> |      Server       | <--> |   Database  |
    +-----------+      +-------------------+      +-------------+
                             |         |
                             |         |
                        +----v----+ +--v--+
                        | Websock. | | SQL |
                        +----------+ +-----+

**Key Components**:
- **HTTPS Server**: Uses Dart's `HttpServer` with SSL for secure connections.
- **TokenHandler**: Issues and validates JWT-like tokens for authentication.
- **SqlHandler**: Executes SQL queries for user, post, event, and follow data.
- **WebsocketHandler**: Manages real-time connections for chat and notifications.
- **Media Handling**: Serves media files from disk using secure GET requests.

**Data Flow**:
1. **Connection**:  
   - HTTPS server accepts connections on port 8080.
   - SSL context is loaded from `certificate.pem` and `private_key.pem`.

2. **Authentication**:  
   - `/signin`: Checks credentials, validates password with bcrypt, issues token/UUID.
   - `/signup`: Handles new user registration (expand logic as needed).

3. **Token Validation**:  
   - All protected endpoints require `Authorization` header.
   - Token is validated before processing request.

4. **Request Routing**:  
   - POST endpoints: Handle posts, events, follow/unfollow.
   - GET endpoints: Serve media, feed, profiles, subjects, suggestions.

5. **WebSocket Upgrade**:  
   - If request is a WebSocket upgrade, connection is handed off to `WebsocketHandler`.

6. **Media Upload/Download**:  
   - Media files are saved to `assets/` and served via GET requests.
   - Supports multipart/form-data for uploads.

7. **Error Handling**:  
   - Returns appropriate HTTP status codes and JSON error messages.
   - Catches and logs exceptions server-side.

**Interface Comments**

.. code-block:: dart

   /// Main server class: handles HTTPS, routing, and backend logic.
   class Server { ... }

   /// Starts the HTTPS server, loads SSL certs, and listens for requests.
   Future<void> start();

   /// Handles sign-in: validates credentials, issues token/UUID.
   void _handleSignIn(HttpRequest request);

   /// Handles sign-up: registers new users (expand as needed).
   void _handleSignUp(HttpRequest request);

   /// Handles text post creation (JSON body).
   Future<void> _handleTextPost(HttpRequest request, String username);

   /// Handles media post creation (multipart/form-data).
   Future<void> _handleMediaPost(HttpRequest request, String username);

   /// Handles event post creation (multipart/form-data).
   Future<void> _handleEventPost(HttpRequest request, String username);

   /// Handles following/unfollowing users.
   Future<void> _handleFollow(HttpRequest request, String username);
   Future<void> _handleUnfollow(HttpRequest request, String username);

   /// Retrieves user feed, profile, subjects, and participant counts.
   Future<void> _handleGetFeed(HttpRequest request, String username);
   Future<void> _handleGetProfile(HttpRequest request, String username);
   Future<void> _handleGetSubjects(HttpRequest request, String username);
   Future<void> _handleJoinEvent(HttpRequest request, String username);
   Future<void> _handleGetParticipantCount(HttpRequest request, String username);

   /// Serves media files from disk.
   Future<void> _getMedia(HttpRequest request);

   /// Provides user suggestions for search.
   Future<void> suggestion(HttpRequest request);

**Implementation Comments**

.. code-block:: dart

   // Loads SSL context for HTTPS server.
   SecurityContext context =
       SecurityContext()
         ..useCertificateChain('certificate.pem')
         ..usePrivateKey('private_key.pem');

   // Validates token for all protected endpoints.
   String? token = request.headers.value('Authorization')?.split(' ').last;
   if (token == null || !_tokenHandler.validateToken(token)) {
     request.response.statusCode = HttpStatus.unauthorized;
     await request.response.close();
     continue;
   }

   // Handles sign-in with bcrypt password check.
   String concatenated = salt + password;
   String computedHash = BCrypt.hashpw(concatenated, passwordHash);
   if (passwordHash == computedHash) { ... }

   // Handles media uploads and saves files to disk.
   Future<String> _saveMediaFile(HttpBodyFileUpload upload, String directory, {String? filenameOverride}) async { ... }

   // Converts DateTime to ISO string for JSON responses.
   void convertDateTime(Map<String, dynamic> data) { ... }

Best Practices
--------------

- Always use HTTPS for all connections.
- Store and validate tokens securely.
- Use bcrypt for password hashing and validation.
- Validate all input data before database operations.
- Return clear error messages and status codes.
- Log all server-side errors for debugging.
- Use prepared statements or parameterized queries to prevent SQL injection.
- Serve media files only from whitelisted directories.

Future Improvements
-------------------

- Expand sign-up logic with full validation and email verification.
- Add rate limiting and brute-force protection.
- Implement full search and suggestion functionality.

Dependencies
------------

- `dart:io`, `dart:convert`, `dart:math`, `dart:typed_data`: Core Dart libraries for server, encoding, and file handling.
- `server/sql_handler.dart`: Database operations.
- `server/token_validation.dart`: Token management and validation.
- `websocket_handler.dart`: Real-time WebSocket features.
- `bcrypt`: Password hashing.
- `http_server`, `path`: HTTP and file path utilities.
