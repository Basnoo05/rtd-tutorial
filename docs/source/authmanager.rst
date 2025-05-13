AuthManager Module
=================

Documentation
-------------

The ``AuthManager`` class securely manages user authentication data (tokens, UUIDs, user IDs) using Flutter Secure Storage. It handles session persistence, token validation, and logout functionality while following security best practices.

Usage – What Does the Software Do?
----------------------------------

- **Secure Storage**:
  - Stores/retrieves JWT tokens, UUIDs, and user IDs
  - Uses platform-native secure storage (Keychain for iOS, Keystore for Android)

- **Session Management**:
  - Checks login status via ``isLoggedIn()``
  - Validates token expiration automatically
  - Clears credentials on logout

- **Token Validation**:
  - Decodes JWT tokens
  - Checks expiration time
  - Handles malformed/invalid tokens

Typical usage flow:
1. After login: ``saveAuthData(token, uuid)`` and ``saveUserId(userID)``
2. For authenticated requests: ``getToken()``
3. Session check: ``isLoggedIn()`` (auto-validates token)
4. Logout: ``logOut()``

Maintenance – How Does the Software Do What It Does?
----------------------------------------------------

**Architecture**::

    +-------------------+
    |  AuthManager      |
    +-------------------+
    | - Secure Storage  |
    | - Token Validation|
    +---------+---------+
              |
    +---------v---------+
    | FlutterSecureStorage
    +-------------------+

**Key Components**:
- ``FlutterSecureStorage``: Encrypted storage for sensitive data
- JWT Decoding: Manual JWT parsing for expiration checks
- Error Handling: Catches and rethrows storage exceptions

**Data Flow**:
1. **Storage**:
   - Data stored via key-value pairs: token, uuid, userID
   - All operations use AES encryption

2. **Token Validation**:
   - Splits JWT into header/payload/signature
   - Decodes base64URL payload
   - Checks ``exp`` claim against current time

3. **Error Cases**:
   - Missing credentials → throws exception
   - Invalid token format → returns false
   - Expired token → auto-logout

**Interface Comments**

.. code-block:: dart

   /// Manages authentication data storage and validation
   class AuthManager { ... }

   /// Saves JWT token and UUID to secure storage
   Future<void> saveAuthData(String token, String uuid)

   /// Retrieves user ID from secure storage
   Future<String> getUserId()

   /// Validates JWT structure and expiration
   Future<bool> validateToken(String token)

**Implementation Comments**

.. code-block:: dart

   // JWT validation logic without external dependencies
   final parts = token.split('.');
   if (parts.length != 3) return false; // Invalid format
   
   // Base64URL normalization for Dart compatibility
   final normalizedPayload = base64Url.normalize(payload);
   final decodedPayload = utf8.decode(base64Url.decode(normalizedPayload));

   // Expiration check using Dart's DateTime
   final exp = payloadMap['exp'] as int;
   final expirationDate = DateTime.fromMillisecondsSinceEpoch(exp * 1000);

Best Practices
--------------

1. **Token Security**:
   - Never store tokens in plaintext
   - Validate tokens before trusting them
   - Use short expiration times

2. **Error Handling**:
   - Catch storage exceptions
   - Re-throw with context for debugging
   - Auto-clear invalid credentials

3. **Session Management**:
   - Combine UUID+token for session tracking
   - Implement token refresh logic (future improvement)

Future Improvements
-------------------

- Add token refresh functionality
- Implement biometric authentication fallback
- Add encryption key rotation support
- Integrate with OAuth providers

Dependencies
------------

- ``flutter_secure_storage``: Secure credential storage
- ``dart:convert``: JWT decoding utilities
- ``dart:io``: Platform-specific security (indirect)
