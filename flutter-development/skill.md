---
name: flutter-development
description:
  Develop secure, performant Flutter applications using language and framework best practices. Use when writing Flutter
  code requiring state management, async operations, security, or performance optimization. Focuses on development
  practices, not UI design.
---

- State Management
  - Prefer immutability: use immutable data classes, rebuild widgets instead of mutating state
  - State scope: keep state as local as possible, only lift when multiple widgets need access
  - Local state: setState for simple, widget-local state
  - InheritedWidget/Provider: for sharing state down widget tree
  - Riverpod: type-safe, testable, compile-time safe provider alternative
  - BLoC: for complex business logic with streams
  - Avoid: global mutable state, singletons with mutable state

- Widget Best Practices
  - const constructors: use const for widgets that don't change, prevents unnecessary rebuilds
  - Extract widgets: when build() exceeds 20 lines or logic repeats
  - Builder methods vs widgets: prefer separate widget classes over builder methods
  - Keys: use when reordering lists or preserving state (ValueKey for data-driven, ObjectKey for objects)
  - Avoid deep widget trees: extract subtrees into separate widgets

- Performance Optimization
  - Build method: keep pure and fast, don't perform expensive operations, move computations to state initialization
  - ListView: use ListView.builder for long lists, implement itemExtent when items have fixed height, use AutomaticKeepAliveClientMixin for expensive list items
  - Images: use cached_network_image, specify dimensions, use WebP format
  - Async operations: never block UI thread, use isolates for CPU-intensive work, debounce user input
  - Memory management: dispose controllers, streams, subscriptions, use weak references, avoid memory leaks from unclosed streams

- Security Best Practices
  - Secure storage: use flutter_secure_storage for sensitive data (tokens, credentials), never use SharedPreferences for secrets, never hardcode API keys
  - Network requests: always use HTTPS, certificate pinning for critical APIs, validate SSL certificates, timeout configurations
  - Input validation: validate all user input, sanitize before display, use TextInputFormatter for real-time validation
  - Authentication: store tokens securely, implement token refresh logic, clear sensitive data on logout, use OAuth 2.0/OIDC
  - Data exposure: don't log sensitive data, obfuscate code in release builds, remove debug prints, secure screenshots on sensitive screens

- Async Programming
  - Future best practices: always handle errors with catchError or try-catch, use async/await over then(), don't use async for synchronous code
  - Stream management: always close StreamController and subscriptions, use StreamBuilder for UI updates, prefer BroadcastStream for multiple listeners
  - FutureBuilder/StreamBuilder: handle all ConnectionState cases, show loading/error/data states, don't initiate async operations inside builders
  - Avoid nested callbacks: use async/await instead

- Error Handling
  - Graceful degradation: app should never crash, catch and handle all errors
  - Error boundaries: use runZonedGuarded for global error handling, FlutterError.onError for Flutter framework errors, PlatformDispatcher.instance.onError for platform errors
  - User-facing errors: show meaningful messages, not stack traces or technical details
  - Logging: log errors with context, use error tracking service (Sentry, Crashlytics)
  - Validation errors: validate early, show clear error messages inline

- Code Organization
  - Feature-based structure: organize by feature, not type
  - Separation of concerns: separate UI, business logic, and data layers
  - Dependency injection: use Provider or Riverpod for DI, testable and decoupled

- Platform-Specific Code
  - Platform channels: for native functionality not available in Flutter
  - Method channels: request/response pattern for calling native code
  - Event channels: stream events from native to Flutter
  - Conditional imports: use dart:io and kIsWeb for platform-specific code
  - Platform detection: check Platform.isIOS, Platform.isAndroid before platform-specific code

- Dependencies and Packages
  - Minimize dependencies: each package increases app size and potential vulnerabilities
  - Verify packages: check popularity, maintenance, and security before adding
  - Pin versions: use exact versions or compatible ranges, not any (^)
  - Update regularly: keep dependencies updated for security patches
  - Audit dependencies: review transitive dependencies for security issues

- Null Safety
  - Enable sound null safety: required for modern Flutter development
  - Avoid null checks: use non-nullable types by default, nullable only when needed
  - Late variables: use late for non-nullable variables initialized after declaration, be careful of LateInitializationError
  - Null-aware operators: use ?., ??, ??=, and conditional access

- Testing
  - Unit tests: test business logic, models, utilities, fast and isolated
  - Widget tests: test widget behavior and interactions, mock dependencies
  - Integration tests: test complete user flows, run on device/emulator
  - Test coverage: aim for high coverage on business logic, lower on UI
  - Mocking: use mockito or mocktail for mocking dependencies
  - Golden tests: for UI regression testing, catch unintended visual changes

- Build and Release
  - Obfuscation: enable code obfuscation in release builds
  - Minification: remove unused code with tree shaking
  - Build modes: Debug (development), Profile (performance), Release (production)
  - Environment configuration: use --dart-define for environment-specific config
  - Version management: semantic versioning, increment build numbers for each release

- Security Pitfalls to Avoid
  - Never store API keys in code or assets
  - Never use HTTP for sensitive data
  - Never trust user input without validation
  - Never log sensitive information
  - Never use SharedPreferences for secrets
  - Never expose debugging tools in production
  - Always use environment variables for configuration
  - Always validate and sanitize all inputs
  - Always implement certificate pinning
  - Always use secure storage for sensitive data
  - Always clear sensitive data on logout
  - Always test security on physical devices

- Performance Monitoring
  - DevTools: use Flutter DevTools for performance profiling
  - Frame rendering: keep frames under 16ms (60fps), use performance overlay to monitor
  - Memory profiling: monitor for memory leaks and excessive allocations
  - Network profiling: track API call frequency and payload sizes
  - App size: monitor app size, use deferred loading for features
