---
name: tailwind-colors
description:
  Design and implement comprehensive color systems with Tailwind CSS. Use when defining brand colors, semantic tokens,
  dark mode variants, and maintaining color consistency across projects.
---

- Color System Design
  - Theme-first: define complete color palette in config before building components
  - Brand palette: create 50-950 shade scale for each brand color (primary, secondary, accent)
  - Semantic colors: define by purpose - success (green), error (red), warning (yellow), info (blue)
  - Neutral grays: 50-950 scale from near-white to near-black, foundation for text, borders, backgrounds

- Color Scale Structure
  - 50-950 shades: consistent lightness progression across all colors
  - 50: lightest tint, subtle backgrounds
  - 100-200: very light, hover states, disabled backgrounds
  - 300-400: light, borders, muted text
  - 500: base color, primary usage
  - 600-700: darker, hover states, emphasis
  - 800-900: very dark, high contrast text
  - 950: darkest, maximum contrast
  - Consistency: all color families should have similar lightness at each level

- Custom Color Palettes
  - Define in config: theme.extend.colors for brand colors
  - Color naming: use semantic names (brand, accent) not generic (blue, purple)
  - Multiple brands: support multiple color schemes with prefixes (brand-primary, brand-secondary)
  - Color generation tools: use tools to generate consistent 50-950 scales from base color

- Semantic Color Tokens
  - Purpose-based naming: bg-success (success state backgrounds), text-error (error message text), border-warning (warning state borders), bg-info (info message backgrounds)
  - Benefits: change color meaning globally, maintain consistency, clearer intent in code

- Text Colors
  - Hierarchy with grays: text-gray-900 (primary text, headings), text-gray-700 (secondary text, body), text-gray-500 (tertiary text, captions), text-gray-400 (disabled text, placeholders)
  - Dark mode: reverse hierarchy - lighter grays for dark backgrounds
  - Brand text: use sparingly - links, CTAs, emphasis

- Background Colors
  - Layering strategy: bg-white / bg-gray-950 (base layer), bg-gray-50 / bg-gray-900 (raised surface), bg-gray-100 / bg-gray-800 (elevated surface), bg-white / bg-gray-700 (overlay/modal)
  - Subtle differences: small shade differences create depth without heavy shadows
  - Dark mode: lighter backgrounds for elevated surfaces (reverse of light mode)

- Border Colors
  - Subtle borders: border-gray-200 in light mode, border-gray-700 in dark mode
  - Interactive borders: border-gray-300 → border-blue-500 on focus
  - Error states: border-red-500 for validation errors
  - Dividers: divide-gray-200 for list/table dividers

- Opacity Utilities
  - Syntax: bg-blue-500/20 for 20% opacity
  - Overlay backgrounds: bg-black/50 for modal overlays
  - Glass effects: bg-white/10 with backdrop-blur
  - Hover states: increase opacity on hover - hover:bg-blue-500/30

- Dark Mode Strategy
  - Class-based: add dark class to html/root element
  - Color inversions: light (bg-white text-gray-900), dark (dark:bg-gray-900 dark:text-white)
  - Not just inversion: design dark mode colors intentionally, don't just invert
  - Desaturate in dark: bright colors are harsh in dark mode, use muted versions
  - Test both modes: design light and dark together, not sequentially

- Color Contrast
  - WCAG AA minimum: 4.5:1 for normal text, 3:1 for large text
  - Tools: use contrast checkers before committing to color choices
  - Text on brand colors: ensure sufficient contrast - text-white on bg-blue-600 usually safe
  - Gray text: text-gray-600 on bg-white meets minimum, text-gray-500 borderline

- Gradient Colors
  - Gradient-ready palette: design colors that work well together in gradients
  - Color harmony: use adjacent or complementary colors for smooth gradients
  - Reference: see tailwind-gradients skill for gradient implementation

- State Colors
  - Interactive states: default (base color), hover (darker shade, 100 higher on scale), active (even darker, 200 higher), focus (add ring in same color family), disabled (muted gray)
  - Form states: valid (success color), invalid (error color), warning (warning color)

- Color Consistency
  - Limit palette: use 2-3 brand colors max, too many colors = inconsistency
  - Systematic usage: document which colors for which purposes
  - Component variants: use same color scale for component variants
  - Avoid random colors: every color should come from theme, not arbitrary values

- Best Practices
  - Do: define complete palette upfront, use semantic color names, test color contrast, design dark mode intentionally, use opacity for variations, stick to defined palette
  - Avoid: adding colors ad-hoc, using arbitrary color values, ignoring dark mode, poor contrast ratios, too many brand colors, inconsistent color usage
