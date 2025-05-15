Recovery Phrase
===============

Overview
--------

The Recovery Phrase page allows users to restore their account using a 24-word mnemonic phrase. This critical security feature ensures users can regain access to their accounts if credentials are lost.

Usage
-----

Key Features:
- **24-Word Input Grid**: Users enter each word of their recovery phrase in numbered fields.
- **Security Warnings**: Prominent visual warnings about phrase confidentiality.
- **Confirmation Button**: Validates and processes the entered phrase.

User Flow:
1. **Navigate to Recovery Page**: Accessed via "Forgot Phrase?" or similar link.
2. **Enter Words**: User inputs each word in the correct order.
3. **Confirm**: System validates the phrase and restores account access if correct.
Backend Integration Details
--------------------------

The backend is responsible for:

- **Phrase Validation**:  
  When the user submits their 24-word phrase, the frontend sends it to the backend via a secure API endpoint (see :doc:`server` and :doc:`httprequesthandler`).  
  - The backend checks that the phrase is a valid BIP39 mnemonic (see :doc:`engine` for backend-side phrase validation logic).
  - The backend verifies the phrase matches a stored, encrypted recovery phrase for a user in the database (see :doc:`sqlhandler`).

- **Account Restoration**:  
  If the phrase is valid and matches a user, the backend:
  - Authenticates the user and issues a new authentication token and UUID (see :doc:`authmanager`).
  - Returns the token and UUID to the frontend for secure storage.
  - May trigger additional security checks or notifications.

- **Security Considerations**:  
  - All phrase data is transmitted over HTTPS.
  - The backend never stores or transmits the phrase in plaintext; it is always encrypted at rest.
  - Rate limiting and brute-force protection are enforced on the recovery endpoint.

- **Error Handling**:  
  - If the phrase is invalid, the backend returns a clear error (e.g., "Invalid recovery phrase" or "Phrase does not match any account").
  - The frontend displays this error to the user and allows them to try again.

Maintenance
-----------

Implementation Details:
- **State Management**: Uses `TextEditingController` for each word input field.
- **UI Structure**:
  - `ListView.builder` creates a scrollable grid of 24 input fields.
  - Each field has:
    - Numbered label
    - White background with subtle shadow
    - Centered text alignment
- **Security Components**:
  - Red warning text about phrase confidentiality
  - Lock icon visual indicator
- **Validation Flow**:
  - Empty `recover()` method awaiting implementation
  - Current placeholder for account recovery logic


Features
--------------

1. **Phrase Validation**:
   - Implement checksum validation for BIP39 phrases
   - Add auto-suggest for valid dictionary words
2. **Security**:
   - Disable screenshots/captures on this screen
   - Auto-clear clipboard after phrase copy
3. **UX Improvements**:
   - Add paste-all functionality
   - Implement word position validation
4. **Error Handling**:
   - Highlight invalid words
   - Show specific error messages for:
     - Invalid word (not in BIP39 dictionary)
     - Incorrect word order
     - Checksum mismatch

