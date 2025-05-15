Edit Profile
===================

Documentation
-------------

The Edit Profile module provides users with a simple interface to update their profile information, specifically their display name and bio. The module is designed for ease of use and integrates seamlessly with the rest of the application.

Usage – What Does the Software Do?
----------------------------------

- **Allows users to edit their profile name and bio** in a dedicated form.
- **Pre-fills the form fields** with the user's current name and bio.
- **Saves changes** when the user taps the save icon, returning the updated data to the previous screen.

Typical user flow:

1. User navigates to the Edit Profile page from their profile screen.
2. The fields for name and bio are pre-populated with the current values.
3. The user edits the fields as desired.
4. The user taps the save icon in the app bar.
5. The updated name and bio are returned to the previous screen.

Backend Integration Details
--------------------------

- **Profile Update API Call**:  
  When the user saves changes, the frontend sends the updated name and bio to the backend using the HTTP request handler (:doc:`httprequesthandler`).
  - The backend validates the input, updates the user record in the database (:doc:`server`, :doc:`sqlhandler`), and returns a success or error response.
  - The frontend updates the local display and shows a confirmation or error message as appropriate.

- **Security**:  
  - All profile update requests are authenticated using the user's token, managed by :doc:`authmanager`.
  - Data is transmitted securely over HTTPS.

- **Error Handling**:  
  - If the backend returns an error (e.g., invalid input, server error), the frontend displays a message to the user and does not update the profile locally.

Maintenance – How Does the Software Do What It Does?
----------------------------------------------------

- **Stateful Management**:  
  Uses `TextEditingController`s for the name and bio fields, initialized in `initState` and disposed in `dispose` for proper memory management.

- **Form Handling**:  
  The form fields are standard `TextField` widgets, with the bio field allowing multiple lines.

- **Saving Data**:  
  The `_saveProfile` method pops the current screen and returns a map containing the updated name and bio.

- **Navigation**:  
  Uses Flutter's Navigator to return data to the previous screen, enabling easy integration with profile display logic.

