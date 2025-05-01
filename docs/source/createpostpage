Create Post Module
==================

Documentation
-------------

The Create Post module allows users to create and submit different types of posts-text, media, or events-within the application. It offers a tab-like selector, dynamic form rendering, and a unified submission interface. The code is modular and easily extensible for future post types.

Usage – What Does the Software Do?
----------------------------------

- **Enables users to create posts** of three types: Text, Media, or Event.
- **Selector bar** at the top allows switching between post types.
- **Dynamic form rendering**: The form displayed adapts to the selected post type.
- **Submission**: All post types are submitted through a unified `submit` function, which delegates to the app's controller.
- **Navigation**: Includes a back button to return to the home page.

Typical user flow:

1. Open the Create Post page.
2. Select the post type using the selector ("Text", "Media", "Event").
3. Fill out the corresponding form.
4. Submit the post; the data is sent to the backend via the app's controller.

Maintenance – How Does the Software Do What It Does?
----------------------------------------------------

- **Stateful Management**:  
  The `selector` integer tracks which post type is currently selected. Changing the selector updates the displayed form.

- **Selector UI**:  
  The `buildSelector` and `_buildSelectorButton` methods render the selector bar. The selected option is highlighted in blue.

- **Form Rendering**:  
  The `buildForm` method chooses which form widget to display (`TextFormWidget`, `MediaFormWidget`, or `EventFormWidget`) based on the current selector value.

- **Submission Logic**:  
  The `submit` method is passed as a callback to each form widget. It calls `controller.engine.createPost(data)` to handle post creation.

- **Navigation**:  
  The back button calls `notifier.setPage(AppPage.home)` to return to the home page.

