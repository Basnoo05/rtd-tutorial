Feed Page
=========

Overview
--------

The Feed Page displays a scrollable list of posts and user recommendations fetched from the backend. It handles authentication checks, data loading states, and pagination. Users can view content and interact with posts while maintaining seamless navigation via a persistent bottom bar.

Usage – What Does the Software Do?
----------------------------------

- **Authentication Check**: Verifies user login status on initialization.
- **Dynamic Content Loading**: Fetches and displays posts/recommendations.
- **Pagination Support**: Loads additional content as needed (currently fetches page 1).
- **Content Categorization**: Displays posts and user recommendations in distinct UI components.

Typical user flow:
1. User opens the app → Feed Page checks authentication
2. If logged in, loads first page of feed content
3. Scrolls through posts and recommendations
4. (Future) Reaches bottom → loads next page automatically

Backend Integration Details
---------------------------

Key Components
1. **Authentication Check**:
   - Uses :doc:`engine` to verify login status via `isLoggedIn()`
   - Redirects to sign-in page if unauthorized

2. **Data Fetching**:
   - Calls `engine.getFeed(page: 1)` through :doc:`httprequesthandler`
   - Backend endpoint typically: `GET /api/feed?page=1`
   - Returns mixed content (posts + user recommendations)

3. **Content Rendering**:
   - `PostWidget` handles standard post formats
   - `UserRecommendationWidget` displays suggested profiles
   - Both components receive data from backend API responses

4. **Pagination**:
   - Current implementation fetches page 1
   - `onLoadMore` callback ready for pagination expansion

Maintenance
-----------

Key Components
- **FeedPage**:
  - Manages authentication state
  - Handles data loading and error states
  - Implements basic pagination structure

- **FeedWidget**:
  - Renders mixed content types
  - Uses `PostWidget` and `UserRecommendationWidget`
  - Ready for infinite scroll implementation

- **Engine Integration**:
  - Authentication checks via :doc:`authmanager`
  - Data fetching via :doc:`httprequesthandler`
  - Post interactions through :doc:`engine`

Error Handling
--------------

1. **Authentication Failures**:
   - Redirects to sign-in page
   - Clears invalid tokens via :doc:`authmanager`

2. **Network Errors**:
   - Shows loading spinner during retries
   - Implements exponential backoff (future)

3. **Empty States**:
   - Displays "No content" message when feed is empty
   - Shows "Load more" button when pagination fails


Best Practices
--------------

1. **Pagination**:
   - Implement lazy loading for better performance
   - Add scroll position detection for auto-loading

2. **Caching**:
   - Cache feed pages locally
   - Use ETag/Last-Modified headers for efficient updates

3. **Security**:
   - Sanitize all rendered content
   - Validate API response schemas

4. **Accessibility**:
   - Add semantic labels for screen readers
   - Ensure proper contrast ratios
