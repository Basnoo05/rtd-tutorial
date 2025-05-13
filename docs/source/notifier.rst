Notifier Module
=================

Documentation
-------------

The ``Controller`` class serves as the central state manager for the application. It implements the singleton pattern to ensure consistent state across all components and handles navigation, authentication state, and notifications.

Usage – What Does the Software Do?
----------------------------------

- **State Management**:
  - Tracks current page/view (``AppPage`` enum)
  - Manages login state (``loggedIn`` flag)
  - Stores notification counters

- **Navigation**:
  - Controls screen transitions via ``setPage()``
  - Enforces login checks before page changes

- **Notifications**:
  - Triggers background notifications using AwesomeNotifications
  - Groups notifications by channel and key

- **Singleton Access**:
  - Provides global access via ``Controller()`` factory
  - Initializes with app state on first launch

Typical usage flow:
1. App starts → ``init()`` checks login state
2. User navigates → ``setPage()`` updates UI
3. Background events → ``triggerNotification()`` creates alerts
4. State changes → ``notifyListeners()`` updates dependent widgets

Maintenance – How Does the Software Do What It Does?
----------------------------------------------------

**Architecture**::

    +-----------------+
    |    Controller   |
    +-------+---------+
            |
    +-------v---------+       +-----------------+
    |     Engine      |       | AwesomeNotifications
    +-----------------+       +-----------------+

**Key Components**:
- **Singleton Pattern**: Ensures single instance via ``_instance``
- **AppPage Enum**: Defines all possible application views
- **Notification Stack**: Manages notification IDs per channel-group

**Data Flow**:
1. **Initialization**:
   - Checks login status via ``engine.isLoggedIn()``
   - Sets initial page based on auth state

2. **Navigation**:
   - ``setPage()`` → updates ``currentPage`` → triggers UI rebuild

3. **Notifications**:
   - Checks ``isInBackground`` state
   - Increments notification counters per channel-group
   - Creates platform-specific alerts

**Error Handling**:
- Failsafe login checks prevent unauthorized access
- Notification ID management prevents duplicates

Comments
--------

### Implementation Comments

.. code-block:: dart

   // Singleton implementation ensures single control point
   static final Controller _instance = Controller._internal();
   factory Controller() => _instance;

   // Login check wrapper for navigation
   void setPage(AppPage page) {
     if (!loggedIn && logInCheck) {
       currentPage = AppPage.signIn; // Auth enforcement
     } else {
       currentPage = page;
     }
     notifyListeners();
   }

   // Notification grouping logic
   void triggerNotification(channelKey, groupKey, title, body) {
     String notificationKey = "$channelKey$groupKey";
     notifications[notificationKey] = (notifications[notificationKey] ?? 0) + 1;
     // ...
   }

### Interface Comments

.. code-block:: dart

   /// Central application state controller
   /// - Manages navigation, auth state, and notifications
   class Controller extends ChangeNotifier { ... }

   /// Changes current view with auth validation
   /// - page: Target AppPage enum value
   void setPage(AppPage page)

   /// Initializes controller state
   /// - Checks auth status
   /// - Sets initial page
   Future<void> init()

   /// Displays notification when app is backgrounded
   /// - channelKey: Notification category
   /// - groupKey: Notification thread ID
   void triggerNotification(String channelKey, String groupKey, String title, String body)


Best Practices
--------------

1. **State Management**:
   - Use ``Controller()`` factory for all access
   - Always call ``notifyListeners()`` after state changes

2. **Navigation**:
   - Use ``AppPage`` enum for all page references
   - Perform auth checks before sensitive page transitions

3. **Notifications**:
   - Use descriptive channel keys (e.g., 'messages', 'alerts')
   - Reuse group keys for notification threads
   - Test background/foreground states

Future Improvements
-------------------

- Add notification channels configuration
- Implement deep linking for navigation
- Add state persistence
- Support notification actions
- Integrate with native platform features

Dependencies
------------

- ``Engine``: Authentication and data handling
- ``awesome_notifications``: Cross-platform notifications
- ``config.dart``: Application configuration
