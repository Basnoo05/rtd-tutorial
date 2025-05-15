Sign Up
=======

Overview
--------

The sign-up feature allows new users to create an account using a secure, multi-step process. This module implements a user-friendly sign-up flow in Flutter, including account setup, secure recovery phrase generation, and phrase verification.

Requirements
------------

- **Username**: Unique username for each user.
- **Password**: Strong password (user-defined).
- **Recovery Phrase**: Secure, randomly generated mnemonic phrase for account recovery.
- **Minimal Friction**: The form is kept short and simple, following best practices for high conversion rates.

Sign Up Process
---------------

1. **Account Setup**  
   Users enter their username and password on the sign-up page.  
   The frontend then sends this data to the backend, which checks if the username is unique and chekcs the password for complexity and security (see :doc:`httprequesthandler` and :doc:`engine`).  
   - The backend typically queries the database to check if the username already exists and applies password validation logic (such as length, character requirements, etc.).

2. **Recovery Phrase Generation**  
   The system generates a secure, random recovery phrase (using BIP39, 256-bit strength) and displays it to the user.  
   This phrase is included in the registration data sent to the backend for secure storage and future account recovery.  
   - The backend securely stores the recovery phrase, often encrypting it at rest and associating it with the user’s profile record (see :doc:`server` and :doc:`sqlhandler`).

3. **Phrase Verification**  
   Users are asked to re-enter the recovery phrase to confirm they have stored it safely.  
   This step is handled entirely on the frontend for user assurance.

4. **Completion**  
   If successfully verified, the frontend sends the username, password, and recovery phrase to the backend to create the account.  
   The backend performs several critical operations:
   - Hashes the password using a strong algorithm.
   - Creates a new user record in the database, storing the username, hashed password, and encrypted recovery phrase.
   - Optionally, creates a linked user profile record for additional user data.
   - Returns a success response or error if registration fails (e.g., username taken, weak password, database error).
.. note::

   The recovery phrase is essential for account recovery. Users should never share this phrase and must store it securely.

Error Handling
--------------

- **Missing Fields**: If required fields (username or password) are missing, the user is prompted to fill them.
- **Password Mismatch**: If the password and confirmation do not match, an error is displayed.
- **Incorrect Recovery Phrase**: If the recovery phrase is entered incorrectly during verification, the user is prompted to try again.

Best Practices
--------------

- **Minimize Form Fields**: Only ask for essential information to reduce friction and increase conversion rates.
- **Clear Value Proposition**: Clearly explain the benefit of signing up and why the recovery phrase is important.
- **Mobile Friendly**: The UI is responsive and works well on mobile devices.
- **No Password Confirmation Field**: Consider omitting the password confirmation for even less friction, using a password visibility toggle instead.
- **Social Proof**: Optionally, add testimonials or security assurances to further increase user trust.



