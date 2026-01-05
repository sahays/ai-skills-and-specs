---
name: tailwind-typography
description:
  Create clear, readable typography systems with Tailwind CSS. Use when defining font scales, hierarchies, and text
  styling for consistent, accessible typography across applications.
---

- Typography Scale
  - Font sizes: systematic scale for consistency
  - Base scale: text-xs (0.75rem/12px, tiny text, captions), text-sm (0.875rem/14px, small text, labels), text-base (1rem/16px, body text default), text-lg (1.125rem/18px, emphasized body), text-xl (1.25rem/20px, small headings), text-2xl (1.5rem/24px, H4 headings), text-3xl (1.875rem/30px, H3 headings), text-4xl (2.25rem/36px, H2 headings), text-5xl (3rem/48px, H1 headings), text-6xl (3.75rem/60px, large display), text-7xl (4.5rem/72px, hero headings), text-8xl (6rem/96px, extra large display), text-9xl (8rem/128px, massive display)
  - Body text: text-base (16px) is optimal for readability
  - Mobile: often use smaller sizes, scale up on larger screens

- Font Families
  - Define in config: add custom fonts to theme
  - System fonts: font-sans, font-serif, font-mono
  - Custom fonts: import Google Fonts or local fonts
  - Headings vs body: different font families for hierarchy
  - Fallbacks: always include system font fallbacks

- Font Weights
  - Scale: 100-900 in increments of 100
  - Common weights: font-thin (100, rarely used), font-extralight (200, very light), font-light (300, light), font-normal (400, body text default), font-medium (500, slightly emphasized), font-semibold (600, strong emphasis), font-bold (700, headings, important text), font-extrabold (800, very strong), font-black (900, maximum weight)
  - Limit weights: use 2-3 weights max (e.g., 400, 500, 700)
  - Headings: font-semibold or font-bold
  - Body: font-normal
  - Emphasis: font-medium or font-semibold

- Line Height
  - Spacing between lines: critical for readability
  - Scale: leading-none (1, no extra space, large headings), leading-tight (1.25, tight, headings), leading-snug (1.375, slightly tight), leading-normal (1.5, body text default), leading-relaxed (1.625, comfortable reading), leading-loose (2, very spacious)
  - Body text: leading-normal (1.5) or leading-relaxed (1.625)
  - Headings: leading-tight (1.25) or leading-snug (1.375)
  - Responsive: adjust line height at different breakpoints if needed

- Letter Spacing
  - Tracking: space between characters
  - Scale: tracking-tighter (-0.05em, very tight), tracking-tight (-0.025em, tight, large headings), tracking-normal (0, default), tracking-wide (0.025em, wide, small caps), tracking-wider (0.05em, wider), tracking-widest (0.1em, widest, all caps)
  - Headings: tracking-tight for large sizes
  - All caps: tracking-wide or tracking-wider
  - Body: tracking-normal (don't adjust)

- Text Colors
  - Hierarchy with grays: text-gray-900 (primary text, headings, important body), text-gray-700 (secondary text, normal body), text-gray-500 (tertiary text, captions, meta), text-gray-400 (disabled text)
  - Dark mode: use lighter grays (text-gray-100, text-gray-300)
  - Brand colors: links, CTAs, emphasis
  - Semantic colors: success, error, warning text

- Text Alignment
  - Align: text-left, text-center, text-right, text-justify
  - Default: text-left for most text (English)
  - Center: headings, hero sections sparingly
  - Avoid justify: can create awkward spacing
  - Responsive: change alignment at breakpoints

- Text Transform
  - Case: uppercase, lowercase, capitalize, normal-case
  - Uppercase: labels, small headings (use with tracking-wide)
  - Capitalize: titles, names
  - Avoid: all caps for long text (reduces readability)

- Text Decoration
  - Underline: underline - links default
  - Line through: line-through - strikethrough
  - No underline: no-underline - remove default underline
  - Decoration color: decoration-blue-500
  - Decoration style: decoration-solid, decoration-dotted, decoration-dashed
  - Decoration thickness: decoration-1, decoration-2

- Heading Hierarchy
  - Consistent scale: clear size difference between levels
  - Pattern: H1 (text-4xl md:text-5xl font-bold), H2 (text-3xl md:text-4xl font-bold), H3 (text-2xl md:text-3xl font-semibold), H4 (text-xl md:text-2xl font-semibold), H5 (text-lg font-semibold), H6 (text-base font-semibold)
  - Responsive: larger sizes on desktop, smaller on mobile

- Body Text Patterns
  - Standard body: text-base text-gray-700 leading-relaxed
  - Large body: text-lg text-gray-700 leading-relaxed - landing pages
  - Small body: text-sm text-gray-600 leading-normal - dense UIs
  - Caption: text-xs text-gray-500 - metadata, timestamps

- Link Styling
  - Default: text-blue-600 hover:text-blue-700 underline
  - No underline: text-blue-600 hover:underline - cleaner, hover reveals
  - Bold links: font-medium or font-semibold for emphasis
  - Visited: consider visited:text-purple-600 for distinction

- List Styling
  - Disc/decimal: list-disc, list-decimal
  - Position: list-inside, list-outside
  - Custom markers: use before: pseudo-element
  - Spacing: space-y-2 between items

- Text Truncation
  - Single line: truncate - adds ellipsis
  - Multiple lines: line-clamp-{n} - clamps to n lines
  - Overflow: overflow-hidden with truncate

- Responsive Typography
  - Mobile-first: start with smaller sizes
  - Breakpoint scaling: text-lg md:text-xl lg:text-2xl
  - Hero headings: significant size increase on desktop
  - Body text: usually consistent, sometimes scale up on desktop

- Dark Mode Typography
  - Text colors: lighter grays on dark backgrounds
  - Contrast: ensure readability in both modes
  - Pattern: text-gray-900 dark:text-gray-100
  - Avoid: pure white text (harsh), use text-gray-100 instead

- Prose Typography
  - @tailwindcss/typography plugin: for rich text content
  - Prose classes: auto-styles markdown/HTML content
  - Customization: extend prose styles in config
  - Use case: blog posts, documentation, CMS content

- Accessibility
  - Minimum size: 16px (1rem) for body text
  - Contrast: WCAG AA minimum (4.5:1 for normal text)
  - Line length: 60-80 characters per line optimal
  - Line height: 1.5 minimum for body text
  - Scalability: support 200% text zoom

- Performance
  - Font loading: preload critical fonts
  - Font display: use font-display: swap in @font-face
  - Subset fonts: only include needed characters
  - System fonts: fastest loading option

- Custom Font Scale
  - Extend in config: add custom sizes between defaults
  - Maintain ratio: keep consistent scale relationship
  - Tools: use modular scale calculators (1.2, 1.25, 1.333 ratio)

- Text Effects
  - Text shadow: text-shadow-sm, text-shadow-md (define in config)
  - Gradient text: see tailwind-gradients skill
  - Glow effect: colored text shadow for neon effect

- Best Practices
  - Do: use systematic scale, limit font weights (2-3), set comfortable line height, left-align body text, test readability on devices, ensure sufficient contrast, use semantic heading levels
  - Avoid: random font sizes, too many font weights, tight line height on body text, center-aligned paragraphs, all caps for long text, low contrast text, skipping heading levels (H1→H3)

- Common Patterns
  - Hero heading: text-5xl md:text-6xl font-bold text-gray-900 dark:text-white leading-tight
  - Section heading: text-3xl font-bold text-gray-900 dark:text-white
  - Card title: text-xl font-semibold text-gray-900 dark:text-white
  - Body text: text-base text-gray-700 dark:text-gray-300 leading-relaxed
  - Caption: text-sm text-gray-500 dark:text-gray-400
  - Link: text-blue-600 hover:text-blue-700 dark:text-blue-400 dark:hover:text-blue-300
