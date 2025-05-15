Profile Page Module
===================

Documentation
-------------

The Profile Page module provides users with a comprehensive view of their profile, including their avatar, stats, bio, and posts. It supports profile editing, navigation to friends and settings, and displays a grid of user posts. The code is modular and integrates with other app components such as navigation and editing.

Usage – What Does the Software Do?
----------------------------------

- **Displays user profile information**: Shows avatar, username, bio, and statistics (posts, friends).
- **Edit Profile**: Users can update their name and bio via the "Edit Profile" button.
- **Navigation**:
  - Settings: Access via the settings icon in the app bar.
  - Friends: Tap the "Friends" stat to view the friends list.
- **Posts Grid**: Shows a grid of user posts (images).
- **Bottom Navigation**: Persistent navigation bar for seamless app navigation.

Typical user flow:

1. User opens their profile page.
2. Views profile details and statistics.
3. Taps "Edit Profile" to update name or bio. Changes are reflected immediately.
4. Taps "Friends" to view their friends list.
5. Taps the settings icon to access account settings.
6. Scrolls through the grid of posts.

Backend Integration Details
--------------------------

The Profile Page is deeply integrated with the backend for all user data operations:

- **Profile Data Fetching**:  
  On page load, the frontend calls `Controller().engine.getUserProfile(userID: ...)`, which sends a request to the backend to retrieve the user’s profile data and posts.  
  - The backend fetches user info, avatar URL, bio, follower count, and posts from the database (:doc:`server`, :doc:`sqlhandler`).
  - The avatar is downloaded from the backend using the media endpoint (:doc:`engine`, :doc:`httprequesthandler`).

- **Editing Profile**:  
  When the user edits their profile, the frontend sends the updated name and bio to the backend, which updates the database and returns the new data on success.

- **Follow/Unfollow**:  
  The follow/unfollow buttons trigger requests to the backend to update the relationship between users.  
  - The backend updates the follow relationship and returns the new follower count (:doc:`server`, :doc:`sqlhandler`).

- **Posts Retrieval**:  
  The user’s posts are dynamically loaded from the backend and displayed in a grid.  
  - The backend returns a list of post metadata and media URLs, which are then fetched and displayed by the frontend.

- **Error Handling**:  
  If any backend request fails (e.g., network error, invalid user, server error), the frontend displays an appropriate error message and ensures the UI remains stable.

Maintenance – How Does the Software Do What It Does?
----------------------------------------------------

- **Stateful Management**:  
  - Uses local state for `username`, `name`, and `bio`.
  - Updates profile info on return from the edit profile page.
- **UI Components**:
  - `AppBar` with settings navigation.
  - Profile header with avatar, bio, and stats.
  - `ElevatedButton` for editing profile.
  - `GridView.builder` for displaying posts.
  - Persistent `NavBarWidget` at the bottom.
- **Navigation**:
  - Uses `Navigator.push` for editing profile.
  - Uses `Controller().setPage` for navigating to friends and settings.
- **Extensibility**:
  - Easily add more profile fields or post types.
  - Modular helper methods for stats columns.

Best Practices
--------------

- **Efficient Data Fetching**: Minimize API calls and use caching where possible.
- **Error Handling**: Display clear messages for backend errors and handle loading states gracefully.
- **Security**: All sensitive operations require authentication and are performed over HTTPS.
- **Extensibility**: Design UI and backend endpoints to support future features (e.g., profile pictures, badges, more post types).
