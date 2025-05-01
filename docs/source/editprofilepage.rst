Edit Profile Module
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

Comments
--------

### Implementation Comments

.. code-block:: dart

   // Initializes controllers with the current name and bio.
   @override
   void initState() {
     super.initState();
     nameController = TextEditingController(text: widget.currentName);
     bioController = TextEditingController(text: widget.currentBio);
   }

   // Disposes controllers to free resources.
   @override
   void dispose() {
     nameController.dispose();
     bioController.dispose();
     super.dispose();
   }

   // Saves the updated profile and returns data to previous screen.
   void _saveProfile() {
     Navigator.pop(context, {
       'name': nameController.text,
       'bio': bioController.text,
     });
   }

### Interface Comments

.. code-block:: dart

   /// EditProfilePage widget
   /// - currentName: String, initial value for the name field
   /// - currentBio: String, initial value for the bio field
   class EditProfilePage extends StatefulWidget { ... }

   /// State for EditProfilePage, manages form controllers and save logic.
   class _EditProfilePageState extends State<EditProfilePage> { ... }

   /// Returns a Map<String, String> with updated 'name' and 'bio' on save.
