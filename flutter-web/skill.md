---
name: flutter-web
description:
  Develop Flutter applications specifically for web deployment. Use when building Flutter web apps with considerations
  for SEO, performance, browser compatibility, and web-specific features. Focuses on web platform specifics, not general
  Flutter development.
---

- Rendering Modes
  - HTML renderer: better text rendering, smaller download size, better SEO, default for mobile browsers
  - CanvasKit renderer: better performance for graphics-heavy apps, consistent rendering, default for desktop browsers
  - Auto mode: Flutter chooses based on device, recommended for most cases
  - Override: use --web-renderer html or --web-renderer canvaskit flag when building
  - Choose HTML for: text-heavy apps, SEO-critical content, smaller initial load
  - Choose CanvasKit for: games, complex graphics, custom painting, consistent cross-browser rendering

- Loading and Splash Screen
  - Initial load time: Flutter web downloads engine binaries before app starts, show splash screen during download
  - Custom splash screen: edit web/index.html to customize loading experience
  - Loading indicator: add CSS animation or spinner in index.html body, visible until Flutter takes over
  - Progress tracking: use window.flutter_loading_progress event to show download progress
  - Optimize perception: branded splash screen makes wait feel intentional, not broken
  - Keep simple: plain HTML/CSS only, no JavaScript frameworks, must load instantly

- SEO and Metadata
  - Meta tags: configure in web/index.html - title, description, keywords, Open Graph, Twitter cards
  - Structured data: add JSON-LD for rich search results
  - Dynamic metadata: use flutter_seo package or custom head management for route-specific metadata
  - Sitemap: generate sitemap.xml for search engines, update when routes change
  - robots.txt: configure crawling rules in web/ directory
  - Limitation: client-side rendering limits SEO, consider server-side rendering for SEO-critical apps

- URL Strategy
  - Hash-based routing: #/route is default, works everywhere, no server config needed
  - Path-based routing: /route has clean URLs, better SEO, requires server configuration
  - Configure path-based: import flutter_web_plugins/url_strategy.dart and call usePathUrlStrategy() before runApp()
  - Server requirement: serve index.html for all routes when using path-based routing

- Performance Optimization
  - Initial load time: minimize app size with tree shaking, use deferred loading for routes, lazy load images, compress assets, use CDN for static assets
  - Bundle size: HTML renderer produces smaller bundles, remove unused dependencies, use code splitting with deferred imports
  - Web workers: offload heavy computations to web workers, use flutter_isolate or native web workers
  - Caching: configure service worker for offline support, cache static assets aggressively, use appropriate Cache-Control headers
  - Images: use WebP format for better compression, implement lazy loading, specify image dimensions to avoid layout shifts, use responsive images with srcset

- Browser Compatibility
  - Target browsers: Chrome, Firefox, Safari, Edge (Chromium), test on all
  - Mobile browsers: iOS Safari, Chrome Mobile have different rendering characteristics
  - Feature detection: check browser capabilities before using web-specific APIs
  - Polyfills: not automatically included, add if supporting older browsers
  - Test extensively: different browsers handle Flutter web differently, especially with CanvasKit

- Responsive Design
  - Breakpoints: design for mobile, tablet, desktop, use MediaQuery for screen size
  - Adaptive layouts: use LayoutBuilder to adapt to available space
  - Navigation: drawer for mobile, sidebar for desktop, NavigationRail for medium screens
  - Touch vs mouse: support both touch and mouse interactions, hover states for desktop
  - Keyboard navigation: essential for web accessibility, support tab navigation and shortcuts

- PWA Features
  - Manifest: configure web/manifest.json for PWA features
  - Service worker: enable in web/flutter_service_worker.js for offline support
  - Install prompt: configure app installation on supported browsers
  - Offline functionality: cache critical assets and data, handle offline state gracefully
  - Push notifications: use web push notifications API, require user permission
  - App icons: provide icons in various sizes in web/ directory

- JavaScript Interop
  - js package: call JavaScript from Dart using package:js
  - @JS() annotation: define JavaScript APIs in Dart
  - dart:html: access browser APIs directly
  - postMessage: communicate between Flutter and external JavaScript
  - Avoid: excessive JS interop impacts performance and type safety

- Web-Specific Security
  - Content Security Policy: configure CSP headers to prevent XSS
  - CORS: handle cross-origin requests properly, configure server CORS headers
  - HTTPS only: always serve Flutter web apps over HTTPS in production
  - Iframe embedding: use X-Frame-Options or CSP frame-ancestors to control embedding
  - Secrets: never expose API keys in client-side code, use backend proxy
  - Input validation: validate all inputs, web apps are exposed to broader attack surface

- Routing and Navigation
  - go_router: recommended for web routing, type-safe, supports deep linking
  - Deep linking: all routes should be directly accessible via URL
  - Browser back button: must work correctly, test navigation history
  - Route parameters: extract from URL path or query parameters
  - Redirect handling: handle authentication redirects, route guards
  - 404 handling: show appropriate error page for invalid routes

- Asset Management
  - Asset optimization: compress images (WebP, optimized PNG/JPEG), minify JSON and other text assets, remove unused assets
  - Asset loading: lazy load non-critical assets, preload critical assets, use asset variants for different screen densities
  - Fonts: subset fonts to include only needed characters, use system fonts when possible for faster load, preload fonts to avoid FOUT (Flash of Unstyled Text)
  - CDN: host large assets on CDN for faster global delivery

- Web-Specific Packages
  - Prefer web-compatible packages: check pub.dev for web platform support
  - Common web packages: url_launcher_web (open URLs and emails), shared_preferences_web (browser localStorage), file_picker_web (file upload from browser), image_picker_web (image selection from browser)
  - Platform detection: use kIsWeb to conditionally use web-specific code

- Deployment
  - Build for production: flutter build web --release
  - Web server configuration: serve index.html for all routes (path-based routing), enable gzip compression, set appropriate cache headers, serve over HTTPS
  - Hosting options: Firebase Hosting (easy, supports Flutter web well), Netlify/Vercel (good for static sites), GitHub Pages (free for public repos), Cloud providers (AWS S3, GCP Cloud Storage, Azure Blob)
  - Environment configuration: use --dart-define for environment-specific config

- Performance Monitoring
  - Web vitals: monitor Largest Contentful Paint (LCP), First Input Delay (FID), Cumulative Layout Shift (CLS)
  - Lighthouse: run Lighthouse audits for performance, accessibility, SEO
  - Analytics: use firebase_analytics or Google Analytics for web
  - Error tracking: Sentry, Crashlytics for web error monitoring
  - RUM: Real User Monitoring to track actual user performance

- Accessibility
  - Semantic HTML: Flutter web generates DOM, ensure proper semantics
  - ARIA labels: use Semantics widget for screen reader support
  - Keyboard navigation: all interactive elements must be keyboard accessible
  - Focus management: proper focus order and visible focus indicators
  - Color contrast: meet WCAG AA standards minimum
  - Screen reader testing: test with NVDA, JAWS, VoiceOver

- Limitations and Workarounds
  - No native mobile features: camera, GPS, sensors not available on web, use web APIs instead
  - File system access: limited, use File System Access API (limited browser support) or downloads
  - Background tasks: limited compared to mobile, use service workers for background sync
  - App size: larger than mobile apps, optimize aggressively
  - Performance: generally slower than native web frameworks for simple apps, better for complex UI
  - Hot reload: works but slower than mobile hot reload

- Web-Specific Debugging
  - Browser DevTools: use Chrome/Firefox DevTools for debugging
  - Network inspection: monitor asset loading and API calls
  - Console logging: use debugPrint, appears in browser console
  - Performance profiling: use DevTools Performance tab
  - Responsive design testing: use browser responsive mode to test different screen sizes

- Common Pitfalls
  - Don't: use mobile-specific packages without checking web support, ignore SEO and metadata, assume all Flutter features work on web, neglect browser compatibility testing, use hash URLs without considering SEO impact, forget to configure server for path-based routing
  - Do: test on multiple browsers and devices, optimize for initial load time, implement proper error boundaries, use web-specific packages when available, configure PWA features for better UX, monitor web vitals and performance
