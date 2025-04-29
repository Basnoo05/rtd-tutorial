Sign In
=======

Overview
--------

The sign-in feature allows existing users to securely access their accounts. This section explains the implementation and usage of the sign-in functionality within the Flutter application.

Usage
-----

The sign-in page provides a familiar authentication interface with the following features:

- **Username and Password Fields**: Users enter their credentials to authenticate.
- **Password Visibility Toggle**: Users can show or hide their password input for convenience.
- **Sign In Button**: Submits the credentials for validation.
- **Error Feedback**: If the credentials are invalid, an error message is displayed.

The sign-in process is structured as follows:

1. **Navigate to the Sign In Page**  
   Users are presented with the sign-in form on app launch or when authentication is required.

2. **Enter Credentials**  
   Users input their username and password.

3. **Submit Form**  
   Upon pressing the "Sign In" button, the app validates the credentials using the authentication engine.

4. **Feedback**  
   If authentication succeeds, the user is redirected to the home page. If it fails, an error message is shown.

Maintenance
-----------

**How the Software Works:**

- The `SignInPage` widget manages the sign-in UI and logic.
- The `SignInFormWidget` handles user input for username and password, including validation and password visibility.
- The `_handleSignIn` method in `_SignInPageState` retrieves the input, calls the authentication engine, and updates the UI based on the result.
- Error messages are shown using a `SnackBar` if authentication fails.
- The design follows Flutter's best practices for stateful widgets and form validation[2][3][6].

API Documentation
-----------------

While the current implementation uses a local authentication engine, a typical API endpoint for sign-in might look like:

.. code-block:: http

   POST /api/signin HTTP/1.1
   Content-Type: application/json

   {
       "username": "your_username",
       "password": "your_password"
   }

Example Code
------------

Below is a simplified example of the sign-in page implementation:

.. code-block:: dart

   import 'package:flutter/material.dart';
   import 'package:studubdz/notifier.dart';

   class SignInPage extends StatefulWidget {
     const SignInPage({super.key});

     @override
     State<SignInPage> createState() => _SignInPageState();
   }

   class _SignInPageState extends State<SignInPage> {
     final TextEditingController _usernameController = TextEditingController();
     final TextEditingController _passwordController = TextEditingController();

     @override
     Widget build(BuildContext context) {
       double screenWidth = MediaQuery.of(context).size.width;
       return Scaffold(
         body: Column(
           children: [
             const Expanded(
               flex: 2,
               child: SignInHeaderWidget(),
             ),
             Expanded(
               flex: 2,
               child: SignInFormWidget(
                 usernameController: _usernameController,
                 passwordController: _passwordController,
               ),
             ),
             Expanded(
               flex: 2,
               child: Center(
                 child: SizedBox(
                   width: screenWidth * 0.5,
                   child: ElevatedButton(
                     onPressed: () => _handleSignIn(),
                     child: const Text('Sign In'),
                   ),
                 ),
               ),
             ),
           ],
         ),
       );
     }

     void _handleSignIn() async {
       String username = _usernameController.text;
       String password = _passwordController.text;

       print("Validating login details.");

       bool success = await Controller().engine.logIn(username, password);

       if (success) {
         setState(() {
           Controller().setPage(AppPage.home);
         });
       } else {
         ScaffoldMessenger.of(context).showSnackBar(
           const SnackBar(
             content: Text('Invalid username or password'),
             backgroundColor: Colors.red,
           ),
         );
       }
     }
   }

   class SignInHeaderWidget extends StatelessWidget {
     const SignInHeaderWidget({super.key});

     @override
     Widget build(BuildContext context) {
       return const Column(
         mainAxisAlignment: MainAxisAlignment.end,
         children: [
           SizedBox(
             height: 20,
           ),
           Icon(
             Icons.person,
             size: 100,
             color: Colors.grey,
           ),
           SizedBox(height: 10),
           Text(
             'Sign In',
             textAlign: TextAlign.center,
             style: TextStyle(fontSize: 30),
           ),
           SizedBox(height: 20),
         ],
       );
     }
   }

   class SignInFormWidget extends StatefulWidget {
     const SignInFormWidget({
       super.key,
       required this.usernameController,
       required this.passwordController,
     });

     final TextEditingController usernameController;
     final TextEditingController passwordController;

     @override
     State<SignInFormWidget> createState() => _SignInFormWidgetState();
   }

   class _SignInFormWidgetState extends State<SignInFormWidget> {
     bool _obscurePassword = true;

     @override
     Widget build(BuildContext context) {
       return Padding(
         padding: const EdgeInsets.symmetric(horizontal: 50),
         child: Column(
           mainAxisAlignment: MainAxisAlignment.center,
           children: [
             TextFormField(
               controller: widget.usernameController,
               decoration: const InputDecoration(
                 labelText: 'Username *',
                 prefixIcon: Icon(
                   Icons.person,
                 ),
               ),
               validator: (String? value) {
                 if (value == null || value.isEmpty) {
                   return 'Please enter your username';
                 }
                 return null;
               },
             ),
             const SizedBox(height: 20),
             TextFormField(
               controller: widget.passwordController,
               obscureText: _obscurePassword,
               decoration: InputDecoration(
                 labelText: 'Password *',
                 prefixIcon: const Icon(
                   Icons.lock,
                 ),
                 suffixIcon: IconButton(
                   icon: Icon(
                     _obscurePassword ? Icons.visibility : Icons.visibility_off,
                   ),
                   onPressed: () {
                     setState(() {
                       _obscurePassword = !_obscurePassword;
                     });
                   },
                 ),
               ),
               validator: (String? value) {
                 if (value == null || value.isEmpty) {
                   return 'Please enter your password';
                 }
                 return null;
               },
             ),
           ],
         ),
       );
     }
   }

Best Practices
--------------

- **Simple and Clear UI**: Only essential fields are shown to minimize friction.
- **Immediate Feedback**: Users receive instant feedback on authentication success or failure.
- **Secure Handling**: Password fields are obscured by default, and sensitive actions are confirmed with user feedback.
- **Extensible Architecture**: The code is modular and can be extended to integrate with various authentication providers[2][3][4][6].

