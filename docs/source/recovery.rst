Recovery Phrase
===============

Overview
--------

The Recovery Phrase page allows users to restore their account using a 24-word mnemonic phrase. This critical security feature ensures users can regain access to their accounts if credentials are lost.

Usage
-----

### Key Features:
- **24-Word Input Grid**: Users enter each word of their recovery phrase in numbered fields.
- **Security Warnings**: Prominent visual warnings about phrase confidentiality.
- **Confirmation Button**: Validates and processes the entered phrase.

### User Flow:
1. **Navigate to Recovery Page**: Accessed via "Forgot Phrase?" or similar link.
2. **Enter Words**: User inputs each word in the correct order.
3. **Confirm**: System validates the phrase and restores account access if correct.

Maintenance
-----------

### Implementation Details:
- **State Management**: Uses `TextEditingController` for each word input field.
- **UI Structure**:
  - `ListView.builder` creates a scrollable grid of 24 input fields.
  - Each field has:
    - Numbered label
    - White background with subtle shadow
    - Centered text alignment
- **Security Components**:
  - Red warning text about phrase confidentiality
  - Lock icon visual indicator
- **Validation Flow**:
  - Empty `recover()` method awaiting implementation
  - Current placeholder for account recovery logic

API Documentation
-----------------

While this screen primarily handles local validation, a typical account recovery endpoint might be:

.. code-block:: http

   POST /api/recover-account HTTP/1.1
   Content-Type: application/json

   {
       "phrase": "word1 word2 ... word24"
   }

Example Code
------------

.. code-block:: dart

   class RecoveryPhrasePage extends StatefulWidget {
     const RecoveryPhrasePage({super.key});

     @override
     State<RecoveryPhrasePage> createState() => _RecoveryPhrasePageState();
   }

   class _RecoveryPhrasePageState extends State<RecoveryPhrasePage> {
     late List<TextEditingController> wordControllers;

     @override
     void initState() {
       super.initState();
       wordControllers = List.generate(24, (_) => TextEditingController());
     }

     void recover() {
       // TODO: Implement phrase validation and account recovery
     }

     @override
     Widget build(BuildContext context) {
       return Scaffold(
         body: Center(
           child: Padding(
             padding: const EdgeInsets.all(20.0),
             child: Column(
               children: [
                 const Icon(Icons.key, size: 60),
                 const Text('Your Recovery Phrase', style: TextStyle(fontSize: 24)),
                 Expanded(
                   child: ListView.builder(
                     itemCount: 24,
                     itemBuilder: (context, index) => Row(
                       children: [
                         Text('${index + 1}.'),
                         Expanded(
                           child: TextField(
                             controller: wordControllers[index],
                           ),
                         ),
                       ],
                     ),
                   ),
                 ),
                 ElevatedButton(
                   onPressed: recover,
                   child: const Text('Confirm'),
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

1. **Phrase Validation**:
   - Implement checksum validation for BIP39 phrases
   - Add auto-suggest for valid dictionary words
2. **Security**:
   - Disable screenshots/captures on this screen
   - Auto-clear clipboard after phrase copy
3. **UX Improvements**:
   - Add paste-all functionality
   - Implement word position validation
4. **Error Handling**:
   - Highlight invalid words
   - Show specific error messages for:
     - Invalid word (not in BIP39 dictionary)
     - Incorrect word order
     - Checksum mismatch

