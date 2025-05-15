Schedule Page
=============

Overview
--------

The Schedule Page provides users with an interactive, visually engaging view of upcoming events. It displays event cards in a horizontally scrollable carousel, allowing users to flip each card for more details. The schedule is powered by dynamic data fetched from the backend.

Usage – What Does the Software Do?
----------------------------------

- **Displays a list of upcoming events**: Each event is shown as a card with name, location, and start time.
- **Card Flip Interaction**: Tapping a card flips it to reveal more details, such as description and address.
- **Carousel Navigation**: Users can scroll through events horizontally and use navigation arrows to move between them.
- **Persistent Navigation Bar**: The bottom navigation bar allows users to switch to other sections of the app.

Typical user flow:

1. User opens the Schedule Page.
2. The app fetches upcoming event data from the backend.
3. Events are displayed as cards in a carousel; user can scroll or use arrows to navigate.
4. Tapping a card flips it to show more details.
5. User can use the navigation bar to access other app features.

Backend Integration Details
--------------------------

The Schedule Page is tightly integrated with the backend for event data:

- **Event Data Fetching**:  
  On initialization, the `ScheduleWidget` calls `Controller().engine.getUpcomingEvents()`, which sends a request to the backend using :doc:`httprequesthandler`.
  - The backend endpoint (e.g., `GET /api/upcoming-events`) returns a list of event objects with details such as name, location, start/end times, description, and address.
  - Dates are parsed and converted to `DateTime` objects in the frontend.

- **Event Data Structure**:  
  Each event object returned by the backend includes:
  - `eventName`: Name of the event
  - `eventLocationName`: Location name
  - `eventStartAt`: Start time (ISO8601 string)
  - `eventEndAt`: End time (ISO8601 string)
  - `eventDescription`: Event description
  - `eventAddress`: Event address

- **Security and Performance**:  
  - All event data is fetched over HTTPS.
  - The backend may implement pagination or filtering for large event lists.
  - Only authenticated users can access event data (token-based authentication via :doc:`authmanager`).

- **Error Handling**:  
  - If event data cannot be fetched, the frontend displays "No upcoming events."
  - Backend errors are logged and can be surfaced to the user if needed.


Maintenance – How Does the Software Do What It Does?
----------------------------------------------------

- **Stateful Management**:  
  - Uses a `PageController` to manage the horizontal carousel of event cards.
  - Stores the current page and event data in state.
  - Fetches event data asynchronously on initialization.

- **UI Components**:
  - `ScheduleWidget`: Handles data fetching, carousel, and navigation arrows.
  - `EventCard`: Displays event information and handles flip animation for front/back views.
  - `NavBarWidget`: Persistent navigation bar at the bottom.

- **Navigation**:
  - Arrows and swipe gestures move between events in the carousel.
  - Tapping a card flips it to show more details.

Features
--------

- **Interactive Carousel**: Smooth horizontal scrolling and navigation arrows for browsing events.
- **Card Flip Animation**: Tap to reveal more information about each event.
- **Responsive Design**: Adapts to various screen sizes.
- **Error Handling**: Graceful fallback if no events are available.

Best Practices
--------------

- **Efficient Data Fetching**: Fetch only upcoming events and use pagination if needed.
- **User Experience**: Provide clear feedback if no events are available.
- **Security**: Authenticate all event data requests and use HTTPS.
- **Extensibility**: Easily add filtering, event creation, or RSVP features as needed.

