Sign In
=======

Overview
--------

The sign-in feature allows users to access their accounts securely. This section explains the implementation and usage of the sign-in function.

Usage
-----

The sign-in page provides a familiar authentication interface with the following features:

- **Username and Password Fields**: Users enter their credentials to authenticate.
- **Password Visibility Toggle**: Users can show or hide their password input for convenience.
- **Sign In Button**: Submits the credentials for validation.
- **Error Feedback**: If the credentials are invalid, an error message is displayed.

Sign In Process
---------------

1. **Navigate to the Sign In Page**  
   Users are presented with the sign-in screen as the app is launched or when authentication is required.

2. **Enter Credentials**  
   Users input their username and password.

3. **Submit Form**  
   Upon pressing the "Sign In" button, the app sends the credentials to the backend using the authentication engine (:doc:`engine`) and HTTP request handler (:doc:`httprequesthandler`).  
   - The frontend calls the backend’s `/api/signin` endpoint, sending the username and password as a JSON payload.
   - See :doc:`server` for backend endpoint implementation details.

4. **Backend Authentication**  
   The backend performs several critical operations:
   - **User Lookup**: Queries the database for the username (:doc:`sqlhandler`).
   - **Password Verification**: Compares the submitted password (after hashing) with the stored hash using bcrypt or a similar algorithm (:doc:`authmanager`).
   - **Token Issuance**: If credentials are valid, the backend generates a secure authentication token (e.g., JWT) and a user UUID, and returns them to the frontend.
   - **Session Storage**: The frontend stores the token and UUID securely using the :doc:`authmanager`.

5. **Feedback and Navigation**  
   - If authentication is successful, the user is redirected to the home page.
   - If authentication fails (invalid credentials, user not found, etc.), an error message is shown using a `SnackBar`.

.. note::

   All authentication data is transmitted over HTTPS and never stored in plaintext. Tokens are stored securely on the device.

Backend Integration Details
--------------------------

The backend is responsible for:

- **Credential Validation**:  
  - Verifying the username exists in the database.
  - Validating the password using secure hashing (bcrypt).
  - Returning appropriate error messages for invalid credentials.

- **Token Management**:  
  - Issuing a signed authentication token (JWT or similar) on successful login.
  - Ensuring token expiration and security best practices.
  - The frontend stores this token using :doc:`authmanager` and attaches it to future requests.

- **Session Security**:  
  - All sensitive operations require a valid token in the `Authorization` header.
  - Tokens are validated on every protected endpoint.

- **Error Handling**:  
  - Returns clear error messages for login failures (e.g., "Invalid username or password").
  - Handles server errors gracefully and securely.

Maintenance
-----------

**How the Software Works:**

- The `SignInPage` widget manages the sign-in UI and logic.
- The `SignInFormWidget` handles user input for username and password, including validation and password visibility.
- The `_handleSignIn` method in `_SignInPageState` retrieves the input, calls the authentication engine, and updates the UI based on the result.
- Error messages are shown using a `SnackBar` if authentication fails.
- The design follows Flutter's best practices for stateful widgets and form validation.


Features
--------------

- **Simple and Clear UI**: Only essential fields are shown to minimize friction.
- **Immediate Feedback**: Users receive instant feedback on authentication success or failure.
- **Secure Handling**: Password fields are obscured by default, and sensitive actions are confirmed with user feedback.
- **Extensible Architecture**: The code is modular and can be extended to integrate with various authentication providers.

