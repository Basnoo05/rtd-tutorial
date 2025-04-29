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

3. **Navigate**  
   The navigation bar at the bottom allows users to switch to other parts of the app.

4. **Interact with Overlays**  
   Additional overlays (such as search results or quick add options) can be displayed as needed.

Maintenance
-----------

**How the Software Works:**

- The `HomePage` widget uses a `Stack` to layer the map, search bar, and navigation bar.
- The `RoundedSearchBox` provides a styled input for searching, calling `Controller().engine.autoSuggest` on text change.
- The `SearchOverlay` widget can be used to display search results or suggestions as an overlay.
- The `DefaultLayout`, `FindOnMap`, `QuickAdd`, and `FindLocations` widgets are placeholders for future features or quick actions.
- All widgets are built using Flutter's best practices for modular, reusable UI components.

API Documentation
-----------------

The Home Page does not directly interact with backend APIs, but the search functionality may trigger auto-suggest queries via the app's engine. A typical search API might look like:

.. code-block:: http

   GET /api/search?q=search_term HTTP/1.1
   Accept: application/json

   Response:
   {
       "results": [
           {"type": "location", "name": "Library"},
           {"type": "person", "name": "Alice"},
           {"type": "event", "name": "Study Group"}
       ]
   }

Example Code
------------

Below is a simplified example of the Home Page and its main widgets:

.. code-block:: dart

   import 'package:flutter/material.dart';
   import 'package:studubdz/UI/map_widget.dart';
   import 'package:studubdz/UI/nav_bar.dart';
   import 'package:studubdz/notifier.dart';

   class HomePage extends StatefulWidget {
     const HomePage({super.key});

     @override
     State<HomePage> createState() => _HomePageState();
   }

   class _HomePageState extends State<HomePage> {
     bool _isSearching = false;

     void _toggleSearch(bool isActive) {
       setState(() {
         _isSearching = isActive;
       });
     }

     @override
     Widget build(BuildContext context) {
       return const Stack(
         children: [
           MapWidget(),
           Positioned(
             top: 70,
             left: 20,
             right: 20,
             child: RoundedSearchBox(),
           ),
           Positioned(
             left: 0,
             right: 0,
             bottom: 20,
             child: NavBarWidget(),
           ),
         ],
       );
     }
   }

   class RoundedSearchBox extends StatelessWidget {
     const RoundedSearchBox({super.key});

     @override
     Widget build(BuildContext context) {
       return Container(
         padding: const EdgeInsets.symmetric(horizontal: 16),
         decoration: BoxDecoration(
           color: Theme.of(context).primaryColor,
           borderRadius: BorderRadius.circular(30),
         ),
         child: TextField(
           onChanged: (text) {
             Controller().engine.autoSuggest(text);
           },
           decoration: const InputDecoration(
             icon: Icon(Icons.search),
             hintText: "Search...",
             border: InputBorder.none,
           ),
         ),
       );
     }
   }

   class SearchOverlay extends StatefulWidget {
     final Function(bool) onFocusChange;

     const SearchOverlay({super.key, required this.onFocusChange});

     @override
     State<SearchOverlay> createState() => _SearchOverlayState();
   }

   class _SearchOverlayState extends State<SearchOverlay> {
     @override
     Widget build(BuildContext context) {
       return Positioned.fill(
         child: Container(
           color: Colors.white,
           child: const Placeholder(),
         ),
       );
     }
   }

   class DefaultLayout extends StatelessWidget {
     const DefaultLayout({super.key});

     @override
     Widget build(BuildContext context) {
       return const Column(
         children: [FindOnMap(), QuickAdd(), FindLocations()],
       );
     }
   }

   class FindOnMap extends StatelessWidget {
     const FindOnMap({super.key});

     @override
     Widget build(BuildContext context) {
       return const Placeholder();
     }
   }

   class QuickAdd extends StatelessWidget {
     const QuickAdd({super.key});

     @override
     Widget build(BuildContext context) {
       return const Placeholder();
     }
   }

   class FindLocations extends StatelessWidget {
     const FindLocations({super.key});

     @override
     Widget build(BuildContext context) {
       return const Placeholder();
     }
   }

Best Practices
--------------

- **Layered UI**: Uses a `Stack` for flexible placement of map, search, and navigation elements.
- **Separation of Concerns**: Each widget handles a specific responsibility, making the codebase easier to maintain and extend.
- **Customizable Components**: Widgets like `RoundedSearchBox` and `DefaultLayout` can be easily customized for future requirements.
- **Responsive Design**: Layouts are adaptable to different screen sizes and devices.
