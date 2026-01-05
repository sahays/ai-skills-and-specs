---
name: tailwind-gradients
description:
  Create stunning gradient effects with Tailwind CSS for backgrounds, text, borders, and animations. Use when
  implementing modern gradient designs for hero sections, cards, buttons, and UI elements.
---

- Gradient Directions
  - Linear gradients: bg-gradient-to-{direction}
  - Directions: to-r (left to right →), to-l (right to left ←), to-t (bottom to top ↑), to-b (top to bottom ↓), to-br (top-left to bottom-right ↘), to-bl (top-right to bottom-left ↙), to-tr (bottom-left to top-right ↗), to-tl (bottom-right to top-left ↖)
  - Most common: to-r (horizontal), to-b (vertical), to-br (diagonal)

- Color Stops
  - Two-color gradient: from-{color} and to-{color}
  - Three-color gradient: add via-{color} for middle stop
  - Example: bg-gradient-to-r from-blue-500 via-purple-500 to-pink-500
  - Stop positioning: Tailwind auto-distributes stops evenly

- Background Gradients
  - Hero backgrounds: bg-gradient-to-br from-blue-600 via-purple-600 to-pink-600
  - Subtle sections: bg-gradient-to-r from-gray-50 to-gray-100
  - Dark mode: dark:from-gray-900 dark:via-gray-800 dark:to-gray-900
  - Overlay gradients: bg-gradient-to-t from-black/80 to-transparent over images
  - Card accents: subtle gradient backgrounds for elevated cards

- Text Gradients
  - Pattern: use bg-gradient-*, bg-clip-text, text-transparent
  - Example: bg-gradient-to-r from-blue-500 to-purple-600 bg-clip-text text-transparent
  - Animated text: combine with animate-gradient for color shift effect
  - Headings: large gradient headings for hero sections
  - Logo text: gradient brand names for visual interest

- Border Gradients
  - Gradient border technique: use padding with gradient background, relative p-[1px] bg-gradient-to-r from-blue-500 to-purple-600 rounded-lg
  - Then inner element with bg-white dark:bg-gray-900 rounded-lg
  - Card borders: gradient borders for premium feel
  - Button outlines: animated gradient borders on hover

- Button Gradients
  - Solid gradient button: bg-gradient-to-r from-blue-600 to-purple-600
  - Hover transition: hover:from-blue-700 hover:to-purple-700
  - With animation: add hover:scale-105 transition-all
  - Ghost button with gradient: border gradient + transparent background

- Gradient Overlays
  - Image overlays: layer gradient over image for text readability
  - Pattern: bg-gradient-to-t from-black/60 to-transparent
  - Use cases: hero images with text, card images with captions
  - Multiple overlays: stack gradients for complex effects

- Animated Gradients
  - Moving gradient: use bg-[length:200%_200%] animate-gradient-move
  - Define in config: create gradient-move keyframe animation
  - Shimmer effect: diagonal gradient animation for loading states
  - Hover gradients: transition gradient colors on hover

- Theme Gradient Combinations
  - Define in config: create reusable gradient combinations
  - Brand gradients: primary, secondary, accent gradient presets
  - Consistent usage: use same gradient combinations throughout app
  - Example names: gradient-brand, gradient-success, gradient-sunset

- Gradient Patterns
  - Hero section: large diagonal gradient background
  - Feature cards: subtle top-to-bottom gradient
  - Pricing cards: gradient accent on featured plan
  - Stats section: gradient backgrounds for metric cards
  - Footer: subtle gradient for visual interest
  - CTA sections: bold gradient backgrounds for calls-to-action

- Radial Gradients
  - Not built-in: use arbitrary values or custom utilities
  - Add to config: define radial gradients as custom utilities
  - Use case: spotlight effects, hero backgrounds

- Conic Gradients
  - Not built-in: use arbitrary values or custom utilities
  - Add to config: for color wheel effects
  - Rare usage: specialized design needs only

- Gradient + Glassmorphism
  - Combine: gradient background + backdrop blur
  - Pattern: bg-gradient-to-br from-blue-500/20 to-purple-500/20 backdrop-blur-md
  - Modern look: translucent gradient panels

- Performance Considerations
  - Simple gradients: two-color gradients perform best
  - Complex gradients: multiple stops impact performance slightly
  - Animated gradients: use sparingly, can be CPU intensive
  - Mobile: test gradient performance on mobile devices

- Accessibility
  - Text on gradients: ensure contrast on all parts of gradient
  - Critical text: avoid text directly on complex gradients
  - Fallback colors: gradient-unsupported browsers show first color

- Dark Mode Gradients
  - Muted colors: desaturate gradient colors in dark mode
  - Different gradients: use completely different gradients for dark mode
  - Example: light mode bright blue→purple, dark mode dark blue→dark purple

- Common Combinations
  - Blue to purple: classic, modern, tech-focused
  - Pink to orange: warm, energetic, creative
  - Green to blue: natural, calm, trustworthy
  - Purple to pink: premium, luxury, creative
  - Gray to gray: subtle, professional, minimal

- Anti-Patterns
  - Avoid: too many gradient stops (over 3), high contrast gradients (jarring), gradients on all elements (overwhelming), poor text contrast on gradients, ignoring dark mode gradient variants, random gradient directions
  - Do: use 2-3 colors maximum, choose harmonious color combinations, use gradients strategically for emphasis, test text contrast thoroughly, design dark mode gradients intentionally, be consistent with gradient directions
