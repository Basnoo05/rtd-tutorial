Chat Page
===========

Documentation
-------------

The Chat module provides a user interface for managing direct messages (DMs) and group chats in the application. It features chat sorting, pinning, swipe-to-delete, timestamp formatting, and a persistent navigation bar. The code is modular, with clear separation between UI components and chat logic.

Usage – What Does the Software Do?
----------------------------------

- **Displays a list of direct messages and group chats** with the most recent or pinned chats at the top of each section.
- **Allows users to pin/unpin chats** for quick access.
- **Enables swipe-to-delete** for removing chats from the list.
- **Formats timestamps** to show time for today, weekday for the past week, or date for older messages.
- **Lets users start a new chat** via a dialog (placeholder for future implementation).
- **Provides a bottom navigation bar** for seamless app navigation.

Typical user flow:

1. Open the Chat page to view all DMs and group chats.
2. Swipe a chat to delete it.
3. Tap the pin icon to pin or unpin a chat.
4. Tap the "+" button to initiate a new chat (future feature).
5. Tap a chat to open the conversation (to be implemented).

Maintenance – How Does the Software Do What It Does?
----------------------------------------------------

- **Stateful Management**:  
  Chat lists (`dms` and `groups`) are managed in the state of the `ChatPage` widget. Changes to the list (pin/unpin, delete) trigger UI updates via `setState`.

- **Sorting Logic**:  
  The `sortChats` function sorts chats so that pinned chats appear first, followed by the most recent chats.

- **Timestamp Formatting**:  
  The `formatTimestamp` function displays times as `HH:mm` for today, weekday for the past week, and `MM/DD/YYYY` for older messages.

- **UI Components**:  
  - `buildChatTile` builds each chat item with swipe-to-delete and pin functionality.
  - The chat list is rendered using a `ListView`, and each section (DMs, Groups) is clearly labeled.
  - The navigation bar is rendered using the `NavBarWidget`.

- **Extensibility**:  
  The code is modular and ready for integration with real chat data and navigation to chat screens.

Comments
--------

### Implementation Comments

.. code-block:: dart

   // Generates a random timestamp in the past week for demo purposes.
   DateTime getRandomTime() { ... }

   // Sorts chats: pinned first, then by most recent timestamp.
   List<Map<String, dynamic>> sortChats(List<Map<String, dynamic>> chats) { ... }

   // Formats the timestamp for display in the chat tile.
   String formatTimestamp(DateTime timestamp) { ... }

   // Handles swipe-to-delete action for chat tiles.
   void deleteChat(List<Map<String, dynamic>> list, int index) { ... }

### Interface Comments

.. code-block:: dart

   /// Chat data structure:
   /// - name: String (chat or group name)
   /// - lastMessage: String (last message preview)
   /// - timestamp: DateTime (last activity)
   /// - pinned: bool (is chat pinned)
   Map<String, dynamic> chat = {...};

   /// Builds a chat tile with swipe-to-delete and pin/unpin actions.
   Widget buildChatTile(Map<String, dynamic> chat, List<Map<String, dynamic>> list, int index) { ... }

   /// Main ChatPage widget (stateful).
   class ChatPage extends StatefulWidget { ... }

   /// State for ChatPage, manages chat lists and UI updates.
   class _ChatPageState extends State<ChatPage> { ... }

Code
------------

.. code-block:: dart

   class ChatPage extends StatefulWidget {
     const ChatPage({super.key});
     @override
     State<ChatPage> createState() => _ChatPageState();
   }

   class _ChatPageState extends State<ChatPage> {
     final Random _random = Random();

     DateTime getRandomTime() { ... }

     late List<Map<String, dynamic>> dms;
     late List<Map<String, dynamic>> groups;

     @override
     void initState() { ... }

     List<Map<String, dynamic>> sortChats(List<Map<String, dynamic>> chats) { ... }
