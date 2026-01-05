---
name: react-forms
description:
  Build performant, accessible React forms using modern patterns. Use when implementing forms with validation, error
  handling, and submission. Focuses on React Hook Form, Zod validation, and Shadcn form components.
---

- Modern Form Stack
  - React Hook Form: uncontrolled forms for better performance, minimal re-renders
  - Zod: type-safe schema validation, runtime validation with TypeScript types
  - Shadcn Form components: pre-built form components using React Hook Form and Radix UI, accessible by default
  - Pattern: Shadcn Form + React Hook Form + Zod = modern, performant, accessible forms

- React Hook Form Basics
  - Setup: useForm() hook to initialize form, register() to register inputs, handleSubmit() for form submission, minimal re-renders, uncontrolled by default
  - Form modes: onSubmit (validate on submit, default, best performance), onChange (validate on every change, immediate feedback), onBlur (validate when field loses focus, balanced), onTouched (validate after first blur, then on change)
  - Prefer onSubmit or onBlur: less aggressive validation, better UX

- Zod Schema Validation
  - Define schema with Zod: type-safe validation rules, automatic TypeScript types from schema, reusable schemas, custom error messages
  - Integrate with React Hook Form: use @hookform/resolvers/zod to connect Zod schema to form
  - Server-side validation: use same Zod schema on backend for consistency
  - Benefits: single source of truth for validation, type safety, clear error messages

- Shadcn Form Components
  - Use Shadcn Form components: pre-built, accessible, composable
  - Core components: Form (root wrapper with FormProvider), FormField (connects field to form state), FormItem (field container with spacing), FormLabel (accessible label), FormControl (input wrapper), FormDescription (help text), FormMessage (error message display)
  - Pattern: consistent structure across all form fields

- Form State Management
  - Field registration: register inputs with register() or Controller for custom components
  - Watch values: use watch() to observe field values for conditional logic
  - Set values programmatically: use setValue() to update fields
  - Reset form: use reset() to clear or restore default values
  - Form errors: access via formState.errors
  - Dirty/touched state: track user interaction with formState.isDirty and formState.touchedFields

- Validation Patterns
  - Required fields: Zod .min(1) for strings, .refine() for custom rules
  - Email validation: Zod .email() with custom error message
  - Password strength: use .refine() with custom validation function
  - Conditional validation: use Zod .refine() or .superRefine() for cross-field validation
  - Async validation: React Hook Form supports async validation (check username availability)
  - Client + server validation: always validate on server, client validation is UX enhancement

- Error Handling
  - Display errors: use FormMessage component or formState.errors
  - Error timing: show errors after user interaction (touched/blur), not immediately
  - Inline errors: display errors near the field, not just at form level
  - Error messages: clear, actionable, specific - "Email is required" not "Invalid input"
  - Field-level errors: for individual field issues
  - Form-level errors: for submit errors (API errors, network issues)
  - Accessible errors: use aria-invalid and aria-describedby, Shadcn handles this automatically

- Form Submission
  - Submission pattern: handleSubmit(onValid, onInvalid) wrapper, onValid (called with validated data when form is valid), onInvalid (optional callback for validation errors)
  - Loading states: disable submit button during submission, show loading indicator
  - Success handling: show success message, redirect, or reset form
  - Error handling: display server errors clearly, map backend errors to form fields when possible
  - Optimistic updates: update UI immediately, rollback on error (with TanStack Query)

- Accessibility
  - Labels: every input needs associated label, use FormLabel component
  - Error announcements: screen readers announce errors, use aria-live for dynamic errors
  - Focus management: focus first error field on validation failure
  - Keyboard navigation: all fields keyboard accessible, tab order logical
  - Required indicators: visual indicator (asterisk) and aria-required
  - Help text: use FormDescription for additional context
  - Field groups: use fieldset and legend for radio/checkbox groups

- Performance Optimization
  - Uncontrolled inputs: React Hook Form uses uncontrolled inputs by default, fewer re-renders
  - Isolated re-renders: only re-render components that need updates
  - Lazy validation: validate on blur/submit, not on every keystroke (unless needed)
  - Avoid watching all fields: watch only fields you need for conditional logic
  - Debounce async validation: debounce server-side validation (username check, email verification)
  - Large forms: use shouldUnregister: false and conditionally render sections

- Multi-Step Forms
  - Approach 1 - Single form: keep all data in one form, conditionally render steps
  - Approach 2 - Multiple forms: separate form per step, store data in Zustand between steps
  - Navigation: validate current step before allowing next step
  - Progress indicator: show current step and total steps
  - Back button: allow going back without losing data
  - Auto-save drafts: save progress to localStorage or backend

- Dynamic Fields
  - Field arrays: use useFieldArray() for dynamic lists (add/remove items)
  - Add field: append() to add new field
  - Remove field: remove(index) to delete field
  - Unique keys: use field.id from useFieldArray for stable keys
  - Validation: validate entire array with Zod array schema

- File Uploads
  - File inputs: use register() with type="file"
  - File validation: validate file type and size with Zod or custom validation
  - Preview: show image preview or file name after selection
  - Upload strategy: client upload (upload to cloud storage like S3, Cloudinary directly from client), server upload (send file to backend, backend uploads)
  - Large files: show progress indicator, consider chunked uploads

- Custom Components
  - Controlled components: use Controller for custom inputs (date pickers, rich text, etc.)
  - Third-party components: wrap with Controller to integrate with form
  - Custom validation: add validation via Zod schema or field rules

- Common Patterns
  - Search/filter forms: use watch() to trigger search on field changes, debounce for API calls
  - Auto-save forms: use watch() with debounced save function
  - Confirmation dialogs: warn before leaving form with unsaved changes (formState.isDirty)
  - Disabled state: disable submit while submitting or when form invalid
  - Reset after submit: call reset() after successful submission

- Server-Side Integration
  - API submission: POST form data to backend endpoint
  - Validation errors from server: map backend errors to form fields using setError()
  - Error format: backend should return field-specific errors
  - Success response: return created resource or success message
  - TanStack Query mutation: use for form submission with automatic loading/error states

- Form Testing
  - Test user flows: fill form, submit, verify success/error states
  - Test validation: invalid inputs show errors, valid inputs submit
  - Accessibility testing: check labels, ARIA attributes, keyboard navigation
  - Mock API calls: use MSW to test submission success/failure
  - Don't test library internals: test form behavior, not React Hook Form implementation

- Common Form Types
  - Login form: email/username + password, remember me checkbox optional
  - Registration form: email, password, password confirmation, terms acceptance
  - Profile form: pre-fill from user data, update specific fields
  - Search/filter form: real-time filtering with debounce, clear all button
  - Payment form: card details with validation, address fields
  - Multi-step wizard: progress indicator, navigation, data persistence

- Anti-Patterns
  - Avoid: controlled inputs everywhere (unnecessary re-renders), validating on every keystroke (aggressive, poor UX), client-side validation only (security risk), generic error messages ("Invalid input"), submitting without loading state, not disabling submit during submission, losing form data on navigation, mixing controlled and uncontrolled
  - Do: use uncontrolled inputs (React Hook Form default), validate on blur or submit, validate on both client and server, specific helpful error messages, show loading state during submission, disable submit button when loading, preserve unsaved changes or warn user, consistent controlled/uncontrolled pattern

- Production Checklist
  - Before deploying forms: all fields have labels, error messages are clear and helpful, server-side validation matches client-side, loading states work correctly, success/error states tested, keyboard navigation works, screen reader tested, mobile responsive, file uploads size limits enforced, rate limiting on submission
