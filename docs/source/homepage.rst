Home Page
=========

Overview
--------

The Home Page is the main landing screen of the application, providing users with access to the interactive map, search functionality, and navigation bar. This section explains the implementation, usage, and maintenance of the Home Page and its components.

Usage
-----

The Home Page offers the following features:

- **Interactive Map**: Displays the main map interface for exploring locations, people, and events.
- **Search Bar**: Allows users to search for locations, people, or events using an auto-suggest feature.
- **Navigation Bar**: Provides quick access to other sections of the app.
- **Overlay Components**: Includes overlays such as search results or default layouts for enhanced interaction.

Typical usage flow:

1. **View Map**  
   The map is displayed as the background of the Home Page.

2. **Search**  
   Users can enter search terms in the rounded search bar at the top, triggering auto-suggestions.
   The search bar is connected to the backend via the `autoSuggest` method in the app's engine (:doc:`engine`), which sends the query to the backend using the HTTP request handler (:doc:`httprequesthandler`).
   The backend processes the query and returns suggestions for locations, people, or events, which can be displayed as overlays or in a dropdown.
   See :doc:`server` for the backend endpoint and :doc:`sqlhandler` for the database query logic.

3. **Navigate**  
   The navigation bar at the bottom allows users to switch to other parts of the app.

4. **Interact with Overlays**  
   Additional overlays (such as search results or quick add options) can be displayed as needed.


Backend Integration Details
--------------------------

The Home Page search feature is tightly integrated with the backend:

- **Auto-Suggest API**:  
  When a user types in the search bar, the frontend calls `Controller().engine.autoSuggest(text)`, which sends the search query to the backend using :doc:`httprequesthandler`.  
  - The backend receives the query via a dedicated endpoint (e.g., `/api/search` or `/api/getUserSuggestionsFromName`).
  - The backend processes the query, searching for matching locations, users, or events in the database (:doc:`sqlhandler`).
  - The backend returns a list of suggestions, which are then displayed in the UI.
  - For more details, see :doc:`engine`, :doc:`httprequesthandler`, :doc:`server`, and :doc:`sqlhandler`.

- **Security and Performance**:  
  - All search queries are sent over HTTPS.
  - The backend implements rate limiting and input validation to prevent abuse.
  - Search results are paginated or limited to ensure fast response times.

- **Extensibility**:  
  - The backend can be extended to support more advanced search features (e.g., filtering, ranking, or fuzzy matching).
  - The frontend can be updated to display richer search results as the backend evolves.

Maintenance
-----------

**How the Software Works:**

- The `HomePage` widget uses a `Stack` to layer the map, search bar, and navigation bar.
- The `RoundedSearchBox` provides a styled input for searching, calling `Controller().engine.autoSuggest` on text change.
- The `SearchOverlay` widget can be used to display search results or suggestions as an overlay.
- The `DefaultLayout`, `FindOnMap`, `QuickAdd`, and `FindLocations` widgets are placeholders for future features or quick actions.
- All widgets are built using Flutter's best practices for modular, reusable UI components.

Features
--------------

- **Layered UI**: Uses a `Stack` for flexible placement of map, search, and navigation elements.
- **Separation of Concerns**: Each widget handles a specific responsibility, making the codebase easier to maintain and extend.
- **Customizable Components**: Widgets like `RoundedSearchBox` and `DefaultLayout` can be easily customized for future requirements.
- **Responsive Design**: Layouts are adaptable to different screen sizes and devices.
