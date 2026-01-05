---
name: flutter-forms
description: Build forms with validation, state management, and user input handling in Flutter. Use when creating login forms, settings, user profiles, or any data collection interface.
---

- Core Principles
  - Form widget for grouping: wrap related fields in Form widget for unified validation and state management
  - GlobalKey for access: use GlobalKey<FormState> to trigger validation and save from anywhere
  - Validation on submit: validate when user submits, not on every keystroke (unless specifically required)
  - Clear error messages: provide specific, actionable feedback for validation failures

- Basic Form Structure
  - Form with key: create Form widget with GlobalKey<FormState>, add TextFormField children
  - Validation method: call formKey.currentState.validate() to run all validators, returns bool
  - Save method: call formKey.currentState.save() to trigger all onSaved callbacks
  - Reset method: call formKey.currentState.reset() to clear all fields and errors

- TextFormField Validation
  - Validator function: return error string on failure, null on success
  - Common patterns: required field (check empty), email (regex), min length, numeric (tryParse), password strength
  - Email regex: use pattern like ^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$ for basic validation
  - Combine checks: validate multiple conditions, return first error encountered

- Validation Modes
  - AutovalidateMode: control when validation runs - disabled, always, onUserInteraction
  - Best practice: start with disabled, enable onUserInteraction after first submit attempt
  - Dynamic switching: change mode in setState after validation failure to provide immediate feedback

- Saving Form Data
  - onSaved callback: extract field values when form is saved, store in state variables
  - TextEditingController: alternative approach for programmatic access to field values
  - Controller lifecycle: initialize controllers, access via .text, dispose in dispose() method
  - Mixed approach: use onSaved for simple forms, controllers for complex logic

- Input Types
  - Keyboard types: set keyboardType for appropriate keyboard - emailAddress, number, phone, url
  - Text input actions: use textInputAction - next (move to next field), done (submit form)
  - Obscure text: enable for password fields to hide input characters
  - Max length: set character limit with optional counter display

- Dropdowns and Pickers
  - DropdownButtonFormField: dropdown integrated with Form validation
  - Items: provide list of DropdownMenuItem widgets with value and child
  - Validation: check if value is null for required dropdown fields
  - Date/time pickers: show dialog with showDatePicker or showTimePicker, update TextFormField controller
  - Read-only fields: set readOnly: true on TextFormField for picker-based inputs

- Checkboxes and Switches
  - FormField wrapper: use FormField<bool> to integrate checkboxes/switches with Form validation
  - Builder pattern: use FormField builder to access state and display errors
  - State management: call state.didChange(value) on checkbox change
  - Error display: check state.hasError and show state.errorText conditionally

- Form State Management
  - Local state: use StatefulWidget for simple forms with few fields
  - Provider/Riverpod: extract form logic to separate class for complex multi-screen forms
  - Bloc/Cubit: event-driven validation for multi-step or async validation workflows
  - Form models: create model class with validation methods for reusable form logic

- Async Validation
  - Async validator: return Future<String?> from validator for backend checks
  - Debouncing: delay validation to avoid excessive API calls (use debounce packages)
  - Loading states: show progress indicator during async validation
  - Error handling: handle network errors gracefully, provide fallback validation

- Common Patterns
  - Multi-step forms: use PageView with Form validation per step/page
  - Dynamic fields: use ListView.builder with list of controllers and validators
  - Focus management: use FocusNode to control keyboard focus programmatically
  - Submit loading: disable submit button and show loading indicator during async submit
  - Success feedback: show SnackBar confirmation or navigate to success screen
  - Error handling: display general form errors (network, server) above submit button

- Accessibility
  - Field labels: provide clear labelText in InputDecoration for every field
  - Hint text: add hintText for format examples or placeholder guidance
  - Error announcements: screen readers automatically announce validation errors
  - Focus order: ensure logical tab order, use FocusNode to customize if needed

- Performance
  - Controller reuse: create controllers in initState, reuse in build, dispose in dispose
  - Pure validators: keep validation functions pure and fast, avoid heavy computation
  - Selective rebuilds: use const widgets and ValueListenableBuilder to minimize rebuilds
  - Dispose properly: always dispose controllers and focus nodes to prevent memory leaks
