Settings
========

Overview
--------

The Settings page allows users to manage their account preferences, security settings, and app configuration. This section explains the implementation and usage of the settings functionality.

Usage
-----

Key Features:
- **Account Settings**: Manage profile visibility (public/private)
- **Notification Preferences**: Toggle notification enablement
- **Account Management**: Delete account or log out
- **Profile Visibility**: Control who can view the user's profile

User Flow:
1. **Access Settings**: Navigate via app menu or profile icon
2. **Modify Preferences**:
   - Toggle profile visibility in Account Settings
   - Enable/disable notifications
3. **Account Actions**:
   - Delete account (requires confirmation)
   - Log out of current session

Backend Integration Details
--------------------------

The Settings page is integrated with the backend for all account management and privacy features:

- **Profile Visibility**:  
  When a user toggles profile visibility, the frontend sends an update to the backend using the HTTP request handler (:doc:`httprequesthandler`).  
  - The backend updates the user's profile visibility in the database (:doc:`server`, :doc:`sqlhandler`).
  - The change is reflected in the user's profile data on subsequent fetches.

- **Account Deletion**:  
  Deleting an account triggers a backend API call to permanently remove the user's account and all associated data.  
  - The backend authenticates the request using the user's token (:doc:`authmanager`).
  - The backend deletes user records, posts, and associated data from the database (:doc:`server`, :doc:`sqlhandler`).
  - The frontend logs the user out and navigates to the sign-in page.

- **Logout**:  
  Logging out calls the `logOut()` method in the engine (:doc:`engine`), which clears authentication data from secure storage using :doc:`authmanager`.

- **Error Handling**:  
  Any errors returned from the backend during account update or deletion are displayed to the user.

Maintenance
-----------

Implementation Details:
- **State Management**:
  - Uses `_isProfilePublic` and `_notificationsEnabled` state variables
  - State changes propagate via `onProfileVisibilityChanged` callback
- **UI Components**:
  - `ListView` with categorized settings tiles
  - `Switch` widgets for toggleable settings
  - Destructive actions use red icons for visual emphasis
- **Account Deletion Flow**:
  - Confirmation dialog via `AlertDialog`
  - Placeholder for actual deletion logic in `_deleteAccount()`
- **Navigation**:
  - Account settings accessed via `AccountSettingsPage` route
  - Uses Flutter's standard navigation stack

Features
--------------

1. **Security**:
   - Implement proper authentication for account deletion
   - Add password confirmation for sensitive actions
2. **State Management**:
   - Consider using state management solution (Provider, Bloc) for complex flows
3. **Accessibility**:
   - Add semantic labels for screen readers
   - Ensure proper color contrast ratios
4. **Validation**:
   - Add loading states for async operations
   - Implement error handling for network requests
