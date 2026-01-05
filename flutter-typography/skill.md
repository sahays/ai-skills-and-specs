---
name: flutter-typography
description: Implement text styling and typography systems in Flutter using Material Design type scales and custom fonts. Use when building text themes, font hierarchies, or custom typography.
---

- Core Principles
  - Theme-based text styles: define typography in TextTheme for consistent styling across app
  - Material Design type scale: use semantic names (displayLarge, headlineMedium, bodySmall) rather than arbitrary sizes
  - Context-aware access: get text styles via Theme.of(context).textTheme for dynamic theme updates
  - Responsive sizing: consider MediaQuery or responsive packages for adaptive text scaling

- Text Theme Setup
  - Define in MaterialApp: create TextTheme in ThemeData with all required text styles
  - Material 3 scale: displayLarge/Medium/Small, headlineLarge/Medium/Small, titleLarge/Medium/Small, bodyLarge/Medium/Small, labelLarge/Medium/Small
  - Auto-generated: use Typography.material2021() for platform-specific defaults
  - Custom sizing: override default sizes with project-specific font sizes and weights

- Accessing Text Styles
  - Get via context: access styles through Theme.of(context).textTheme.styleName
  - Null safety: use ?. operator since textTheme styles can be null
  - Extend with copyWith: modify theme styles using copyWith rather than replacing entirely
  - Direct application: apply to Text widget via style parameter

- Custom Fonts
  - pubspec.yaml setup: declare font families with asset paths and weights
  - Multiple weights: define multiple font files for same family (regular, bold, etc.)
  - Global application: set fontFamily in ThemeData to apply globally
  - TextTheme application: use Typography's apply method to apply font family to all styles
  - Google Fonts: use google_fonts package for easy integration without manual downloads

- Font Weights
  - Numeric weights: FontWeight.w100 through FontWeight.w900 (increments of 100)
  - Named weights: FontWeight.normal (400), FontWeight.bold (700)
  - Variable fonts: define multiple weight values in pubspec for font family
  - Weight selection: match font file weight declarations to ensure proper rendering

- Text Styling
  - Color inheritance: text inherits color from theme or set explicitly with TextStyle.color
  - Letter spacing: adjust tracking with letterSpacing property
  - Line height: control with height multiplier (1.5 = 1.5× font size)
  - Decorations: apply underline, line-through, or overline with decoration property

- Rich Text
  - TextSpan for inline: create RichText with TextSpan children for mixed styling
  - Default style: set base style in root TextSpan, override in children
  - Text.rich shorthand: use Text.rich constructor for simpler rich text
  - WidgetSpan: embed widgets (icons, images) inline within text flow

- Text Overflow
  - Ellipsis: use TextOverflow.ellipsis for single-line truncation with "..."
  - Fade: use TextOverflow.fade for gradient fade-out effect
  - Max lines: combine maxLines with overflow for multi-line clipping
  - Flexible wrapping: wrap Text in Expanded or Flexible to constrain width in flex layouts

- Text Alignment
  - Horizontal alignment: use textAlign - left, center, right, justify
  - Text direction: set textDirection (ltr/rtl) for internationalization support
  - Locale-specific: apply locale for language-specific typography rules

- Responsive Typography
  - Scale with screen: multiply base size by screen width percentage for responsive text
  - Text scale factor: respect user accessibility settings via MediaQuery.textScaleFactor
  - Clamp extremes: use min/max to prevent text from becoming too large or small
  - Breakpoint-based: define different text scales for mobile, tablet, desktop

- Common Patterns
  - Section headers: use headlineMedium or titleLarge with subtle color
  - Body content: apply bodyLarge for primary text, bodyMedium for secondary
  - Captions/labels: use labelSmall or bodySmall with reduced opacity
  - Button text: apply labelLarge (14px, medium weight) following Material guidelines
  - All caps labels: combine toUpperCase() with increased letterSpacing (1.5)

- Accessibility
  - Minimum sizes: keep body text at 16px minimum, labels at 14px minimum
  - Contrast compliance: ensure WCAG AA (4.5:1 normal text, 3:1 large text >18px)
  - Scale factor testing: test with textScaleFactor: 2.0 for vision accessibility
  - Semantic labels: use Semantics widget for screen readers when text is decorative

- Performance
  - Const styles: mark TextStyle as const when properties are compile-time constants
  - Cache theme access: store Theme.of(context).textTheme in variable for repeated use
  - Avoid rebuilds: don't create new TextStyle instances in build method unnecessarily
  - Font preloading: load custom fonts in splash screen to avoid flash of unstyled text (FOUT)
