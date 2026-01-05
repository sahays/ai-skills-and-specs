---
name: flutter-gradients
description: Create linear, radial, and sweep gradients in Flutter for backgrounds, buttons, and visual effects. Use when implementing gradient backgrounds, overlays, or gradient text.
---

- Core Principles
  - BoxDecoration for containers: use decoration: BoxDecoration(gradient: ...) for gradient backgrounds
  - ShaderMask for text: apply gradients to text and icons using ShaderMask with gradient shader
  - Performance: gradients render on GPU, but avoid excessive complexity (too many color stops)
  - Accessibility: ensure text contrast over gradients meets WCAG standards (4.5:1 minimum)

- Linear Gradients
  - Basic pattern: two or more colors transitioning in straight line direction
  - Directions: use Alignment values - topLeft to bottomRight (diagonal), topCenter to bottomCenter (vertical), centerLeft to centerRight (horizontal)
  - Color stops: define specific positions (0.0 to 1.0) for each color, or omit for even distribution
  - Multiple colors: add three or more colors for complex multi-stop gradients

- Radial Gradients
  - Circular pattern: colors radiate from center point outward
  - Center position: adjust center using Alignment values for off-center gradients
  - Radius control: set radius (0.0 to 1.0+) relative to container size to control spread
  - Focal points: use focal and focalRadius for elliptical gradient effects

- Sweep Gradients
  - Conical pattern: colors rotate around center point like color wheel
  - Angles: define startAngle and endAngle in radians (0 to 2π for full circle)
  - Use cases: loading spinners, color pickers, circular progress indicators, decorative elements
  - Color repetition: repeat first color at end for seamless circular gradient

- Gradient Text
  - ShaderMask approach: wrap Text widget in ShaderMask, use gradient as shader via createShader
  - BlendMode: use blendMode: BlendMode.srcIn for clean gradient masking
  - Required color: set text color (gets masked but required for sizing)
  - Icons support: same ShaderMask pattern works for Icon widgets

- Gradient Buttons
  - Container wrapper: wrap ElevatedButton in Container with gradient BoxDecoration
  - Transparent button: set button backgroundColor and shadowColor to transparent
  - Ink widget: alternative approach with better Material ripple effects
  - Border radius: match button and container border radius for consistent shape

- Gradient Overlays
  - Image scrims: layer gradient Container over images using Stack for text readability
  - Common patterns: dark gradient at bottom for light text, light gradient at top for dark text
  - Transparency: use Colors.transparent as one gradient color for fade effect
  - Opacity control: adjust alpha channel of gradient colors for subtlety

- TileMode
  - Beyond bounds behavior: control how gradient repeats or extends past defined area
  - TileMode.clamp: default - extends edge colors infinitely
  - TileMode.repeated: repeats gradient pattern continuously
  - TileMode.mirror: alternates gradient direction with each repeat
  - TileMode.decal: renders transparent beyond gradient bounds

- Common Patterns
  - Glassmorphism: combine gradient with BackdropFilter blur effect
  - Neumorphism: layer multiple gradients with shadows for 3D depth illusion
  - Mesh gradients: overlay multiple RadialGradients with opacity for complex effects
  - Animated gradients: use TweenAnimationBuilder to animate color stop positions
  - Hero gradients: match gradient properties in Hero transitions for seamless animation

- Performance Tips
  - Limit color stops: keep under 5-6 colors for optimal performance
  - Cache decorations: store BoxDecoration instances in const or variables, reuse
  - Avoid nesting: minimize layered gradients - combine into single gradient when possible
  - Use const: mark BoxDecoration as const when gradient properties are compile-time constants

- Accessibility
  - Contrast testing: test text contrast using darkest gradient color as background reference
  - Solid fallbacks: provide solid color alternatives for reduced motion/effects preferences
  - Decorative only: don't use gradients to convey critical information - use for visual enhancement only
  - Color independence: combine with text labels or icons, don't rely on gradient colors alone
