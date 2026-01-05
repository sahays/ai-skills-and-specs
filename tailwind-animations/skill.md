---
name: tailwind-animations
description:
  Implement smooth animations and transitions with Tailwind CSS for interactive, polished UI. Use when adding micro-interactions,
  page transitions, loading states, and hover effects. Focuses on performance and accessibility.
---

- Custom Animations in Config
  - Define keyframes: add custom animations to tailwind.config.js
  - Animation system: define @keyframes for animation steps, create animation utilities using keyframes, use animation utilities in components
  - Common custom animations: animate-fade-in (opacity 0→1), animate-slide-up (translate-y + fade), animate-scale-in (scale 0.95→1), animate-shimmer (loading shimmer effect)

- Built-in Animations
  - Spin: animate-spin - continuous rotation (loading spinners)
  - Ping: animate-ping - pulsing circle (notifications)
  - Pulse: animate-pulse - opacity pulse (loading states)
  - Bounce: animate-bounce - bounce effect (attention grabbers)
  - Use sparingly: built-in animations can feel dated, prefer custom subtle animations

- Transitions
  - Transition properties: transition-{property} - transition-all (all properties), transition-colors (colors only), transition-opacity (opacity only), transition-transform (transform only), transition-shadow (shadow only)
  - Prefer specific: use transition-colors not transition-all for better performance

- Transition Duration
  - Scale: duration-{time} - duration-75 (very fast, 75ms), duration-150 (fast, 150ms, quick interactions), duration-200 (normal, 200ms, standard hover), duration-300 (slow, 300ms, emphasized transitions), duration-500 (very slow, 500ms, dramatic effects), duration-700 (extra slow, 700ms, special cases), duration-1000 (1 second, rare, very noticeable)
  - Default: duration-200 for most interactions

- Timing Functions
  - Easing: ease-{type} - ease-linear (constant speed), ease-in (slow start), ease-out (slow end, most natural for UI), ease-in-out (slow start and end)
  - Prefer ease-out: most natural feeling for user interactions

- Hover Animations
  - Scale: hover:scale-105 - subtle grow effect
  - Lift: hover:-translate-y-1 hover:shadow-lg - card lift effect
  - Brightness: hover:brightness-110 - image brightening
  - Color shift: hover:bg-blue-600 transition-colors - button color change
  - Combined: hover:scale-105 hover:shadow-xl transition-all duration-200

- Group Hover
  - Pattern: parent has group class, children use group-hover:
  - Use case: hover card to animate inner elements
  - Example: card with group, icon with group-hover:scale-110
  - Multiple groups: use group/{name} for nested groups

- Focus Animations
  - Ring animation: focus:ring-2 focus:ring-blue-500 transition-shadow
  - Scale on focus: focus:scale-105 for emphasis
  - Smooth ring: always include transition for smooth ring appearance

- Loading Animations
  - Shimmer skeleton: gradient animation across placeholder
  - Pulse skeleton: animate-pulse on gray backgrounds
  - Spinner: animate-spin on circular element
  - Progressive reveal: stagger animate-fade-in with delays

- Stagger Animations
  - Animation delay: animation-delay-{time} (define in config)
  - Use case: list items appearing sequentially
  - Pattern: first item no delay, subsequent items increasing delay
  - Example: delay-0, delay-100, delay-200, etc.

- Page Transitions
  - Route changes: fade out old content, fade in new
  - Slide transitions: slide content left/right for navigation
  - Implementation: use with React Router or Next.js transitions

- Micro-interactions
  - Button click: active:scale-95 - slight shrink on click
  - Checkbox check: scale in checkmark
  - Toggle switch: smooth slide of indicator
  - Input focus: border color change + ring appearance
  - Dropdown open: fade + slide down

- Scroll Animations
  - Not built-in: use Intersection Observer or libraries
  - Fade on scroll: elements fade in as they enter viewport
  - Slide on scroll: elements slide up when visible
  - Stagger on scroll: sequential appearance of list items

- Performance Best Practices
  - Prefer transform and opacity: GPU-accelerated properties
  - Avoid: animating width, height, top, left (causes reflow)
  - Use will-change sparingly: only for complex animations
  - Debounce: limit animation frequency on scroll/resize
  - Reduced motion: respect prefers-reduced-motion

- Reduced Motion
  - Auto-respected in v4: Tailwind handles reduced motion automatically
  - Custom animations: ensure config animations respect reduced-motion
  - Disable on preference: animations disabled for users with motion sensitivity

- Animation Patterns
  - Entrance animations: fade in, slide up, scale in
  - Exit animations: fade out, slide down, scale out
  - Loading states: pulse, shimmer, spin
  - Attention: bounce, ping (use sparingly)
  - Feedback: scale on click, color change, shake on error

- Button Animations
  - Standard hover: hover:bg-blue-600 transition-colors duration-200
  - With scale: hover:scale-105 hover:shadow-lg transition-all duration-200
  - With lift: hover:-translate-y-0.5 hover:shadow-md transition-all duration-150
  - Active state: active:scale-95 for click feedback
  - Loading button: disable + add spinner animation

- Card Animations
  - Hover lift: hover:-translate-y-2 hover:shadow-xl transition-all duration-300
  - Scale: hover:scale-[1.02] transition-transform duration-200
  - Border glow: animate border color or shadow on hover
  - Content reveal: slide in additional content on hover

- Modal Animations
  - Backdrop: fade in opacity-0 to opacity-100
  - Modal content: scale in scale-95 to scale-100 + fade
  - Exit: reverse entrance animation
  - Fast entrance, slower exit: creates polished feel

- Notification Animations
  - Slide in: from top, right, or bottom based on position
  - Fade + slide: combine for smooth appearance
  - Auto-dismiss: fade out after timeout
  - Stacked notifications: stagger animation for multiple

- Form Animations
  - Error shake: horizontal shake on validation error
  - Success check: scale in checkmark icon
  - Field focus: smooth ring appearance
  - Label float: animate label to top on focus (floating labels)

- List Animations
  - Stagger entrance: items appear sequentially
  - Hover highlight: background color transition on hover
  - Expand/collapse: max-height transition (use with caution)
  - Reorder: smooth position changes (requires JS library)

- Text Animations
  - Gradient animation: animated gradient text
  - Type writer: sequential character appearance (requires JS)
  - Counter: animated number counting (requires JS)
  - Fade in words: stagger fade for emphasis

- Best Practices
  - Do: use subtle animations (duration-150 to duration-300), prefer transform and opacity, add transitions to interactive elements, use ease-out for natural feel, respect reduced motion preference, test on actual devices, animate one property at a time
  - Avoid: overly long animations (>500ms), animating layout properties, too many simultaneous animations, animations without purpose, ignoring reduced motion, animation every element, constant motion (distracting)

- Animation Triggers
  - Hover: most common, desktop-focused
  - Focus: accessibility-required
  - Active: click feedback
  - Group hover: related element animation
  - Scroll: entrance animations
  - Time-based: auto-play (use sparingly)
  - State change: loading→success, collapsed→expanded
