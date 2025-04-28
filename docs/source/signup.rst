/// sign_up.dart
///
/// User Sign-Up and Recovery Phrase Module
///
/// This module provides the user interface and logic for the sign-up process in the application,
/// including secure mnemonic (recovery phrase) generation, display, and verification. It guides
/// the user through a multi-step account creation flow:
///
/// 1. **AccountSetup**: Collects username and password.
/// 2. **WordGeneration**: Generates and displays a secure recovery phrase (using BIP39).
/// 3. **WordVerification**: Asks the user to re-enter the recovery phrase for confirmation.
///
/// Main Classes:
///   - SignUpPage: Manages the sign-up flow and step transitions.
///   - AccountSetup: UI for entering username and password.
///   - WordGeneration: Displays and allows copying of the generated recovery phrase.
///   - WordVerification: Verifies the user's knowledge of their recovery phrase.
///
/// Example usage:
///   Navigator.push(context, MaterialPageRoute(builder: (_) => const SignUpPage()));
///
/// .. note::
///    The recovery phrase is generated using the `bip39` package with 256-bit strength.
///    Users must store their recovery phrase securely, as it is required for account recovery.
///
