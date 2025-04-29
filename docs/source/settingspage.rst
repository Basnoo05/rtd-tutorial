Settings
========

Overview
--------

The Settings page allows users to manage their account preferences, security settings, and app configuration. This section explains the implementation and usage of the settings functionality.

Usage
-----

### Key Features:
- **Account Settings**: Manage profile visibility (public/private)
- **Notification Preferences**: Toggle notification enablement
- **Account Management**: Delete account or log out
- **Profile Visibility**: Control who can view the user's profile

### User Flow:
1. **Access Settings**: Navigate via app menu or profile icon
2. **Modify Preferences**:
   - Toggle profile visibility in Account Settings
   - Enable/disable notifications
3. **Account Actions**:
   - Delete account (requires confirmation)
   - Log out of current session

Maintenance
-----------

### Implementation Details:
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

API Documentation
-----------------

While current implementation uses placeholder logic, typical endpoints would include:

.. code-block:: http

   PATCH /api/settings/profile-visibility
   Content-Type: application/json

   {"public": true}

   DELETE /api/account
   Authorization: Bearer <token>

Example Code
------------

.. code-block:: dart

   class SettingsPage extends StatefulWidget {
     const SettingsPage({super.key});

     @override
     State<SettingsPage> createState() => _SettingsPageState();
   }

   class _SettingsPageState extends State<SettingsPage> {
     bool _notificationsEnabled = true;
     bool _isProfilePublic = true;

     @override
     Widget build(BuildContext context) {
       return Scaffold(
         appBar: AppBar(title: const Text('Settings')),
         body: Column(
           children: [
             const Icon(Icons.settings, size: 80),
             Expanded(
               child: ListView(
                 children: [
                   ListTile(
                     leading: const Icon(Icons.person),
                     title: const Text('Account'),
                     onTap: () => Navigator.push(
                       context,
                       MaterialPageRoute(
                         builder: (_) => AccountSettingsPage(...),
                       ),
                     ),
                   ),
                   ListTile(
                     leading: const Icon(Icons.notifications),
                     title: const Text('Notifications'),
                     trailing: Switch(
                       value: _notificationsEnabled,
                       onChanged: (val) => setState(() => _notificationsEnabled = val),
                     ),
                   ),
                   ListTile(
                     leading: const Icon(Icons.delete),
                     title: const Text('Delete Account'),
                     onTap: () => _confirmDeleteAccount(context),
                   ),
                 ],
               ),
             ),
           ],
         ),
       );
     }
   }

   class AccountSettingsPage extends StatefulWidget {
     final bool isProfilePublic;
     final ValueChanged<bool> onProfileVisibilityChanged;

     const AccountSettingsPage({...});

     @override
     State<AccountSettingsPage> createState() => _AccountSettingsPageState();
   }

Best Practices
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

Future Improvements
-------------------
- Add change password functionality
- Implement two-factor authentication settings
- Add app theme customization options
- Include data export functionality

References
----------

- Flutter State Management Documentation
- Material Design Settings Guidelines
- OWASP Mobile Security Guidelines

