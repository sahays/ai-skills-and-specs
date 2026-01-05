---
name: react-tailwind
description:
  Build modern UI with React and Tailwind CSS v4 for landing pages, feature pages, and admin dashboards. Use when
  implementing responsive layouts and component styling. References separate skills for colors, gradients, animations,
  and typography.
---

- Setup and Configuration
  - Installation: use Tailwind v4 with Vite or Create React App
  - Theme-first approach: define comprehensive theme in config before building
  - JIT mode: always enabled, generates only used styles
  - Related skills: see tailwind-colors, tailwind-gradients, tailwind-animations, tailwind-typography for deep dives

- Utility-First Approach
  - Compose utilities: build components with utility classes, not custom CSS
  - Avoid custom CSS: use Tailwind utilities first, extract to components if repeating
  - Class order: layout → spacing → sizing → colors → typography → effects
  - Use cn() helper: combine class names with conditional logic (from Shadcn utils)

- Component Patterns
  - Extract reusable components: create React components for repeated utility combinations
  - Props for variants: control utilities via props (primary vs secondary)
  - Wrapper components: semantic components wrapping Tailwind utilities

- Responsive Design
  - Mobile-first: base styles for mobile, breakpoints for larger screens
  - Breakpoints: sm:640px, md:768px, lg:1024px, xl:1280px, 2xl:1536px
  - Container: container mx-auto px-4 for max-width layouts
  - Hide/show: hidden md:block for responsive visibility

- Layout Patterns
  - Hero section: min-h-screen flex items-center justify-center, bg-gradient-to-br from-blue-500 to-purple-600 (see tailwind-gradients)
  - Feature grid: grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8
  - Dashboard: sidebar (fixed left-0 top-0 h-full w-64), main (ml-64 p-6), responsive (lg:ml-64 with mobile hamburger)
  - Sticky nav: sticky top-0 z-50 bg-white shadow-sm

- Component Styling
  - Buttons: base (px-4 py-2 rounded-lg font-medium transition-colors duration-200), primary (bg-blue-500 text-white hover:bg-blue-600), with animation (hover:scale-105 transition-all, see tailwind-animations)
  - Cards: bg-white dark:bg-gray-800 rounded-lg shadow-sm border border-gray-200 dark:border-gray-700, hover (hover:shadow-md transition-shadow duration-200)
  - Inputs: border border-gray-300 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500

- Dark Mode
  - Class-based: add dark class to root element
  - Pattern: bg-white dark:bg-gray-900 text-gray-900 dark:text-white
  - Store preference: localStorage with initial system preference detection
  - See: tailwind-colors for complete dark mode color strategy

- Spacing and Sizing
  - Spacing scale: 4px base - p-1(4px), p-2(8px), p-4(16px), p-6(24px), p-8(32px)
  - Section padding: py-16 or py-24 for vertical spacing
  - Gaps: gap-4, gap-6, gap-8 for flex/grid
  - Width constraints: max-w-7xl, max-w-4xl for containers

- Landing Page Components
  - Hero: full viewport with gradient background, large heading, CTA
  - Feature grid: 3-column grid with icons, benefits-focused copy
  - Stats: large numbers in 2-4 column grid
  - Testimonials: cards with avatar, quote, attribution
  - See: web-design skill for component patterns

- Dashboard Components
  - Sidebar nav: fixed sidebar with hover states on items
  - Metrics cards: grid of cards with large numbers and trend indicators
  - Data tables: responsive table with hover rows and sticky headers
  - See: web-design skill for dashboard patterns

- Integration with Shadcn
  - Shadcn uses Tailwind: pre-styled components with Tailwind classes
  - Customize via Tailwind: modify by editing utility classes
  - Theme matching: Shadcn theme uses Tailwind config colors
  - Use together: Shadcn for complex components, Tailwind for layouts

- Dynamic Classes
  - Avoid string concatenation: "text-" + color doesn't work with JIT
  - Use full class names: write complete utilities
  - Conditional: use cn() helper or clsx for conditional classes
  - Data attributes: data-[state=active]:bg-blue-500 for state-based styling

- Performance
  - JIT mode: only generates used utilities
  - Content paths: specify template paths in config for accuracy
  - Avoid arbitrary values: use scale values for consistency and smaller bundles
  - Production build: automatically minified and optimized

- Accessibility
  - Focus states: always include focus:ring-2 on interactive elements
  - Color contrast: use tailwind-colors skill for contrast guidelines
  - Screen reader: sr-only for screen reader only content
  - Keyboard nav: ensure all interactive elements have visible focus

- Common Patterns
  - Centered container: container mx-auto px-4 max-w-7xl
  - Flex centering: flex items-center justify-center
  - Animated card: see tailwind-animations for hover effects
  - Gradient text: see tailwind-gradients for text gradient patterns
  - Glassmorphism: backdrop-blur-md bg-white/10 border border-white/20

- Best Practices
  - Do: use utility classes consistently, follow spacing scale, mobile-first responsive design, implement dark mode, include focus states, reference specific Tailwind skills for deep topics
  - Avoid: custom CSS instead of utilities, arbitrary values everywhere, forgetting dark mode, missing focus states, not using responsive utilities

- Production Checklist
  - Before deploying: dark mode implemented and tested, all breakpoints verified, focus states on interactive elements, responsive design works on mobile, touch targets minimum 44x44px, color contrast meets WCAG AA (see tailwind-colors), animations respect reduced motion (see tailwind-animations)

- Related Skills
  - Deep dives: tailwind-colors (color systems, palettes, dark mode), tailwind-gradients (gradient effects and patterns), tailwind-animations (transitions and animations), tailwind-typography (font scales and text styling), web-design (component patterns and layouts), react-development (React best practices)
