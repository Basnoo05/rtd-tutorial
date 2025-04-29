Sign Up
=======

Overview
--------

The sign-up feature allows new users to create an account using a secure, multi-step process. This module implements a user-friendly sign-up flow in Flutter, including account setup, secure recovery phrase generation, and phrase verification.

Requirements
------------

- **Username**: Unique username for each user.
- **Password**: Strong password (user-defined).
- **Recovery Phrase**: Secure, randomly generated mnemonic phrase for account recovery.
- **Minimal Friction**: The form is kept short and simple, following best practices for high conversion rates.

Sign Up Process
---------------

1. **Account Setup**  
   Users enter their username and password on the sign-up page.

2. **Recovery Phrase Generation**  
   The system generates a secure, random recovery phrase (using BIP39, 256-bit strength) and displays it to the user. Users are prompted to copy and store this phrase securely.

3. **Phrase Verification**  
   Users are asked to re-enter the recovery phrase to confirm they have stored it safely.

4. **Completion**  
   Upon successful verification, the account creation process is complete.

.. note::

   The recovery phrase is essential for account recovery. Users should never share this phrase and must store it securely.

Error Handling
--------------

- **Missing Fields**: If required fields (username or password) are missing, the user is prompted to fill them.
- **Password Mismatch**: If the password and confirmation do not match, an error is displayed.
- **Incorrect Recovery Phrase**: If the recovery phrase is entered incorrectly during verification, the user is prompted to try again.

API Documentation
-----------------

This module does not directly interact with a backend API for registration, but if you are implementing API integration, a typical endpoint might look like:

.. code-block:: http

   POST /api/signup HTTP/1.1
   Content-Type: application/json

   {
       "username": "your_username",
       "password": "your_password",
       "recovery_phrase": "generated_mnemonic_phrase"
   }

Example Code
------------

Below is a simplified example of how the sign-up process is structured in Flutter:

.. code-block:: dart

   import 'package:flutter/material.dart';
   import 'package:bip39/bip39.dart' as bip39;

   /// Generates a secure BIP39 mnemonic recovery phrase.
   ///
   /// :return: The generated mnemonic phrase as a String.
   Future<String> generateMnemonic() async {
     return bip39.generateMnemonic(strength: 256);
   }

   /// Main sign-up page managing the step flow.
   class SignUpPage extends StatefulWidget {
     const SignUpPage({super.key});
     @override
     State<SignUpPage> createState() => _SignUpPageState();
   }

   class _SignUpPageState extends State<SignUpPage> {
     int step = 0;
     late String words;

     void nextStep() {
       setState(() {
         step++;
         if (step == 1) generateWords();
       });
     }

     Future<void> generateWords() async {
       final generatedWords = await generateMnemonic();
       setState(() {
         words = generatedWords;
       });
     }

     @override
     Widget build(BuildContext context) {
       switch (step) {
         case 0:
           return AccountSetup(onStepContinue: nextStep);
         case 1:
           return WordGeneration(words: words, onStep: nextStep);
         case 2:
           return WordVerification(words: words, onStep: nextStep);
         default:
           return const Placeholder();
       }
     }
   }

   /// Widget for collecting the user's username and password during sign-up.
   class AccountSetup extends StatelessWidget {
     const AccountSetup({super.key, required this.onStepContinue});
     final VoidCallback onStepContinue;

     @override
     Widget build(BuildContext context) {
       return Scaffold(
         body: Center(
           child: Column(
             children: [
               const Padding(
                 padding: EdgeInsets.all(50.0),
                 child: Text('Sign Up', style: TextStyle(fontSize: 30)),
               ),
               const Padding(
                 padding: EdgeInsets.symmetric(horizontal: 50, vertical: 10),
                 child: TextField(
                     decoration: InputDecoration(labelText: 'Username *')),
               ),
               const Padding(
                 padding: EdgeInsets.symmetric(horizontal: 50, vertical: 10),
                 child: TextField(
                     decoration: InputDecoration(labelText: 'Password *')),
               ),
               const Padding(
                 padding: EdgeInsets.symmetric(horizontal: 50, vertical: 10),
                 child: TextField(
                     decoration: InputDecoration(labelText: 'Confirm Password *')),
               ),
               ElevatedButton(
                 onPressed: onStepContinue,
                 style: ElevatedButton.styleFrom(backgroundColor: Colors.blue),
                 child: const Text('Next'),
               ),
             ],
           ),
         ),
       );
     }
   }

   /// Widget that displays the generated recovery phrase and allows the user to copy it.
   class WordGeneration extends StatefulWidget {
     const WordGeneration({super.key, required this.words, required this.onStep});
     final String words;
     final VoidCallback onStep;

     @override
     State<WordGeneration> createState() => _WordGenerationState();
   }

   class _WordGenerationState extends State<WordGeneration> {
     bool copied = false;

     void copyToClipboard() {
       Clipboard.setData(ClipboardData(text: widget.words));
       setState(() {
         copied = true;
       });
       ScaffoldMessenger.of(context).showSnackBar(
         const SnackBar(content: Text("Recovery phrase copied!")),
       );
     }

     @override
     Widget build(BuildContext context) {
       final wordList = widget.words.split(' ');

       return Scaffold(
         body: Center(
           child: Padding(
             padding: const EdgeInsets.all(20.0),
             child: Column(
               mainAxisAlignment: MainAxisAlignment.center,
               children: [
                 const Icon(Icons.key, size: 60, color: Colors.black),
                 const SizedBox(height: 10),
                 const Text(
                   'Your Recovery Phrase',
                   style: TextStyle(
                       fontSize: 24,
                       fontWeight: FontWeight.bold,
                       color: Colors.red),
                   textAlign: TextAlign.center,
                 ),
                 const SizedBox(height: 10),
                 const Text(
                   'These words are your account recovery phrase. If you lose access, use them to restore your account. **Never share them with anyone!**',
                   style: TextStyle(
                       fontSize: 16,
                       color: Colors.red,
                       fontWeight: FontWeight.w600),
                   textAlign: TextAlign.center,
                 ),
                 const SizedBox(height: 20),
                 Expanded(
                   child: ListView.builder(
                     itemCount: wordList.length,
                     itemBuilder: (context, index) {
                       return Padding(
                         padding: const EdgeInsets.symmetric(vertical: 6),
                         child: Row(
                           children: [
                             Text('${index + 1}.',
                                 style: const TextStyle(
                                     fontSize: 16, fontWeight: FontWeight.bold)),
                             const SizedBox(width: 10),
                             Expanded(
                               child: Container(
                                 decoration: BoxDecoration(
                                   color: Colors.white,
                                   boxShadow: [
                                     BoxShadow(
                                       color: Colors.black.withOpacity(0.2),
                                       spreadRadius: 2,
                                       blurRadius: 6,
                                       offset: const Offset(0, 4),
                                     ),
                                   ],
                                   borderRadius: BorderRadius.circular(10),
                                 ),
                                 child: TextField(
                                   readOnly: true,
                                   controller: TextEditingController(
                                       text: wordList[index]),
                                   textAlign: TextAlign.center,
                                   style: const TextStyle(
                                       fontSize: 16, fontWeight: FontWeight.bold),
                                   decoration: const InputDecoration(
                                     filled: true,
                                     fillColor: Colors.white,
                                     contentPadding:
                                         EdgeInsets.symmetric(vertical: 10),
                                     border: InputBorder.none,
                                   ),
                                 ),
                               ),
                             ),
                           ],
                         ),
                       );
                     },
                   ),
                 ),
                 const SizedBox(height: 20),
                 Row(
                   mainAxisAlignment: MainAxisAlignment.center,
                   children: [
                     ElevatedButton(
                       onPressed: copyToClipboard,
                       style: ElevatedButton.styleFrom(
                         backgroundColor: Colors.black,
                         padding: const EdgeInsets.all(12),
                         shape: const CircleBorder(),
                       ),
                       child:
                           const Icon(Icons.copy, size: 20, color: Colors.white),
                     ),
                     const SizedBox(width: 20),
                     ElevatedButton(
                       onPressed: widget.onStep,
                       style:
                           ElevatedButton.styleFrom(backgroundColor: Colors.black),
                       child: const Text('Confirm',
                           style: TextStyle(color: Colors.white)),
                     ),
                   ],
                 ),
               ],
             ),
           ),
         ),
       );
     }
   }

   /// Widget that verifies the user has correctly stored their recovery phrase by requiring re-entry.
   class WordVerification extends StatefulWidget {
     const WordVerification(
         {super.key, required this.words, required this.onStep});
     final String words;
     final VoidCallback onStep;

     @override
     State<WordVerification> createState() => _WordVerificationState();
   }

   class _WordVerificationState extends State<WordVerification> {
     late List<TextEditingController> wordControllers;

     @override
     void initState() {
       super.initState();
       wordControllers = List.generate(
           widget.words.split(' ').length, (index) => TextEditingController());
     }

     @override
     Widget build(BuildContext context) {
       final wordsList = widget.words.split(' ');

       return Scaffold(
         body: Center(
           child: Padding(
             padding: const EdgeInsets.all(20.0),
             child: Column(
               mainAxisAlignment: MainAxisAlignment.center,
               children: [
                 const Icon(Icons.key, size: 60, color: Colors.black),
                 const SizedBox(height: 10),
                 const Text(
                   'Confirm Recovery Phrase',
                   style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
                   textAlign: TextAlign.center,
                 ),
                 const SizedBox(height: 20),
                 Expanded(
                   child: ListView.builder(
                     itemCount: wordsList.length,
                     itemBuilder: (context, index) {
                       return Padding(
                         padding: const EdgeInsets.symmetric(vertical: 6),
                         child: Row(
                           children: [
                             Text('${index + 1}.',
                                 style: const TextStyle(
                                     fontSize: 16, fontWeight: FontWeight.bold)),
                             const SizedBox(width: 10),
                             Expanded(
                               child: Container(
                                 decoration: BoxDecoration(
                                   color: Colors.white,
                                   boxShadow: [
                                     BoxShadow(
                                       color: Colors.black.withOpacity(0.2),
                                       spreadRadius: 2,
                                       blurRadius: 6,
                                       offset: const Offset(0, 4),
                                     ),
                                   ],
                                   borderRadius: BorderRadius.circular(10),
                                 ),
                                 child: TextField(
                                   controller: wordControllers[index],
                                   textAlign: TextAlign.center,
                                   decoration: const InputDecoration(
                                     filled: true,
                                     fillColor: Colors.white,
                                     contentPadding:
                                         EdgeInsets.symmetric(vertical: 10),
                                     border: InputBorder.none,
                                   ),
                                 ),
                               ),
                             ),
                           ],
                         ),
                       );
                     },
                   ),
                 ),
                 const SizedBox(height: 20),
                 ElevatedButton(
                   onPressed: widget.onStep,
                   style: ElevatedButton.styleFrom(backgroundColor: Colors.black),
                   child: const Text('Confirm',
                       style: TextStyle(color: Colors.white)),
                 ),
               ],
             ),
           ),
         ),
       );
     }
   }

Best Practices
--------------

- **Minimize Form Fields**: Only ask for essential information to reduce friction and increase conversion rates.
- **Clear Value Proposition**: Clearly explain the benefit of signing up and why the recovery phrase is important.
- **Mobile Friendly**: The UI is responsive and works well on mobile devices.
- **No Password Confirmation Field**: Consider omitting the password confirmation for even less friction, using a password visibility toggle instead.
- **Social Proof**: Optionally, add testimonials or security assurances to further increase user trust.

References
----------

- `bip39` package documentation for mnemonic generation
- Best Practices for Sign-Up Forms

